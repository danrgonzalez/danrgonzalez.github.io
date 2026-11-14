---
title: "Reproduce before you replace"
description: "When you migrate a production pipeline, the new one first has to reproduce the old one's published output. Change the method only after that, one change at a time, and measure what each change does."
date: 2026-11-14
order: 1
---

Most pipeline migrations try to do two things at once. They move the system to a new platform, and they improve it along the way: a better solver, a fix for a known quirk, a step nobody understood removed. Then the new numbers come out different, and nobody can say how much of the difference is the improvement and how much is a bug.

My rule is to keep those two steps apart. The new pipeline first has to reproduce the old pipeline's published output, on the same inputs, for a period that has already shipped. Only then do I change the method, one change at a time, and measure what each change does.

## Reproduce what was published

The target is the output that was actually delivered. It isn't what the old code produces if you rerun it today, and it isn't what the documentation says the old system does.

- **Published output is the contract.** Downstream users built reports, models and decisions on it. Those numbers are what "unchanged" has to mean.
- **Rerunning old code drifts.** Its inputs may have been restated, its dependencies upgraded, or its configuration changed since that period shipped.
- **Documentation describes intent.** The system you are replacing is defined by its behavior, including the parts nobody wrote down.

Pick one frozen reference period whose inputs you can recover and whose outputs you have. That period becomes the target.

## Decide what "match" means before you look

Write the comparison down before you see any results.

- **Exact** for things that should be exact: row counts, keys, category assignments, which records are in or out.
- **Within a stated tolerance** for numbers that come from iterative solvers, which don't repeat bit for bit (see [Regression-testing nondeterministic solvers](02-regression-testing-nondeterministic-solvers.md)).

A tolerance chosen after you've seen the gap describes the gap. It doesn't test anything.

Then compare at several levels: counts first, then membership (which keys are in one output and not the other), then distributions, then record-level values. Membership differences usually lead to the root cause faster than aggregate ones.

## Prove the gap, don't explain it

The hard part is the residual. The new pipeline gets close, and the remaining difference arrives with a plausible story attached: solver noise, rounding, late data. A plausible story is a hypothesis. Until it's tested, treat the gap as a bug: take the records that differ and trace them back stage by stage until you find where the two pipelines part ways.

In my experience the residual is rarely noise. The usual causes are mundane:

- An upstream source that looks the same but isn't: a sibling table, a different snapshot, a vendor feed with a similar name.
- A different definition of a time boundary: the time zone of "a day", or which day a late-night event belongs to.
- Filters applied in a different order, or a join that quietly drops or duplicates rows.
- Defaults that differ between libraries or versions.
- A step the old system performs that no document mentions.

Reading the old code is often faster than reasoning about it. When the old system is the specification, its source code is the most accurate documentation you have.

Sometimes the difference turns out to be a real choice, such as a definition the new team thinks is wrong. Make it a configuration switch instead of a silent fix. Reproduce with the old setting, and change it later as a deliberate method change that you measure.

## Then replace, one change at a time

Once the baseline matches, every change is an experiment with a control. If you swap the solver, the difference from the reproduced baseline is the solver's effect. Report it: which estimates moved, by how much, in which direction, and whether that is what the change was supposed to do. "The numbers changed" stops being an incident and becomes a finding you can take to methodology partners before anything ships.

## What it costs, and what it buys

Reproduction feels slow, and matching quirks you plan to remove feels wasteful. The alternative is slower: a migration whose outputs differ for reasons no one can separate, argued about in meetings with no ground truth. Reproduction also tends to turn up real bugs in the old system, which is worth knowing before you retire it. And it gives the new pipeline a regression test for free, because the reproduced period becomes a fixture.

## Checklist

1. Pick a frozen reference period with recoverable inputs and published outputs.
2. Write down the comparison: what must match exactly, and the tolerance for everything else.
3. Compare counts, then membership, then distributions, then records.
4. Trace every residual to a cause. "Noise" counts as a finding only after you've measured it.
5. Turn real disagreements into configuration switches that default to the old behavior.
6. Change the method one step at a time, and report what each step moved.
7. Keep the reference period as a regression fixture.

[← All notes](../notes.qmd){.back-link}
