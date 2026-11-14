---
title: "Regression-testing nondeterministic solvers"
description: "Iterative solvers don't return bit-identical answers across runs, machines or versions. How to write regression tests that tolerate that and still catch real changes."
date: 2026-11-14
order: 2
---

Snapshot tests that demand exact equality break on numerical pipelines. Rerun a convex solver or a raking algorithm and the weights differ in the last few digits. Change the thread count, the linear algebra library or the solver version, and they differ by more. Teams usually respond in one of two ways: they loosen the tolerance until the test never fails, or they delete the test. Either way the protection is gone.

You can do better than both, but first it helps to know where the variation comes from.

## Where the nondeterminism comes from

- **Floating-point order.** Parallel reductions add numbers in a different order on each run, and floating-point addition isn't associative.
- **Stopping rules.** Iterative solvers stop at a tolerance, so the answer is only defined to within that tolerance.
- **Starting points.** Warm starts, random initialization and row order all change the path the solver takes, and so where it stops.
- **Versions.** A new solver release or a different linear algebra backend changes the arithmetic.
- **Non-unique optima.** If the problem has many optimal solutions, the solver can return a different one each time, all with the same objective value.

The last one matters most, because it means that comparing the solution vector can be the wrong test. Two correct runs can return weight vectors that are far apart.

## Test in layers

Rather than one snapshot comparison, I write four kinds of check, from strictest to loosest.

1. **Contract checks.** These hold for any correct answer from any solver: constraints met within the solver's tolerance, bounds respected, no negative or missing values. Property-based testing ([Hypothesis](https://hypothesis.readthedocs.io/)) is a good fit here: generate many small random problems and assert that the contract holds for each, and that the answer doesn't depend on row order.
2. **Objective value.** The optimal objective is unique even when the solution isn't, so compare it to the fixture's.
3. **Solution against a fixture.** Compare the solution to a frozen earlier run, at a tolerance you measured (see the next section).
4. **What people consume.** Compare the downstream estimates built from the solution. This is often the most meaningful layer: the weights can wander along a flat direction while every estimate stays put.

## Measure the tolerance, then prove the test can fail

Don't guess the tolerance. Rerun the solver many times, varying only what is allowed to vary: shuffle the rows, change the thread count, change the seed. Measure the spread of each quantity you plan to test, and set the tolerance a clear margin above the largest spread you saw. Record that number, and how you measured it, next to the test.

Then check the other direction. Make the smallest change you would want the test to catch, such as moving one target by 1%, and confirm that the test fails. A tolerance test that can't detect the smallest change you care about isn't testing anything.

```python
import numpy as np

# Max relative spread over 20 reruns with shuffled rows and 1-8 threads was
# 1e-5 (measured 2026-11-14). Tolerance = 10x that.
RTOL = 1e-4
MARGIN_TOL = 1e-6  # solver feasibility tolerance


def test_raking_regression(sample, margins, fixture):
    w = rake(sample, margins)

    # 1. Contract: margins met, weights valid
    np.testing.assert_allclose(sample.X.T @ w, margins.targets, rtol=MARGIN_TOL)
    assert np.all(w > 0)

    # 2-4. Objective, solution and estimates against the frozen run
    np.testing.assert_allclose(objective(w, sample), fixture.objective, rtol=RTOL)
    np.testing.assert_allclose(w, fixture.weights, rtol=RTOL)
    np.testing.assert_allclose(estimates(sample, w), fixture.estimates, rtol=RTOL)


def test_regression_test_can_fail(sample, margins, fixture):
    # The smallest change we care about must break the comparison.
    w = rake(sample, margins.scale_target("age_18_34", 1.01))
    assert not np.allclose(estimates(sample, w), fixture.estimates, rtol=RTOL)
```

## Gate production runs on distributions

In production there is often no fixed fixture, because the data is new every week. There, compare each run with the previous one before writing anything: weight quantiles, the largest weight, the share of total weight held by the top 1% of records, the effective sample size, and the key estimates. If any of them moves past its threshold, fail the run and write nothing. Set the thresholds from the history of normal week-to-week movement, not from intuition.

## Make every run replayable

Store a small manifest with each run: hashes of the inputs, the configuration, library and solver versions, solver settings and seeds. When a regression test fails, the first question is always whether the inputs changed or the code did. The manifest answers it in seconds.

## Change the fixture on purpose

An intended method change should change the fixture. Regenerate it on purpose, in its own commit, with a note saying what moved and why. If the fixture gets regenerated whenever the test fails, it isn't a fixture any more.

[← All notes](../notes.qmd){.back-link}
