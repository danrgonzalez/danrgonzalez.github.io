---
title: "Stage, validate, promote"
description: "A production pipeline should write nothing its consumers can see until every check has passed. The pattern, the checks worth running, and why dry-run should be the default."
date: 2026-11-14
order: 4
---

The most expensive failure in a data pipeline isn't a crash. It's a run that succeeds and writes wrong numbers to a table that dashboards, models and reports read the next morning. By the time someone notices, the bad output has been copied downstream, and fixing it means tracking down every place it went.

The pattern I use to prevent that has three steps. Every run writes to a staging area first. A validation step checks the staged output. Only if every check passes does a promotion step make the output visible to consumers. If any check fails, the run stops and the production table is left exactly as it was.

## Stage

Write each run's output somewhere consumers never read: a staging table, a run-scoped schema, or a path that includes the run's ID. Two properties matter.

- **Isolation.** A half-finished or failed run can't leak into production, and two runs can't overwrite each other.
- **Inspectability.** When validation fails, the output is still there to look at. You debug the actual bad result, not a reconstruction of it.

## Validate

Validation is code that runs every time, not a notebook someone opens when they remember. I group the checks into four kinds.

- **Contract.** The schema, column order, types, keys and nullability are what consumers expect. This sounds trivial until a downstream write turns out to be positional and a reordered column silently shifts every value one place to the right.
- **Invariants.** Things that must hold for any correct output: totals that must add up, values that must stay inside known bounds, records that must appear exactly once.
- **Inputs.** Upstream data is present, complete and fresh enough for the period being processed. Check this first and fail fast, before spending an hour on computation.
- **Drift.** Compare the output with the previous accepted run: counts, distributions, key estimates. Fail past thresholds set from the history of normal movement. This check catches the errors that pass every rule but are still wrong.

A failed check should explain itself: which check failed, the expected and actual values, and where the staged output is.

## Promote

Promotion should be cheap, atomic and boring: a table swap, a view repointed, a pointer moved. Consumers see the old version or the new one, never a mix. Because promotion only flips a pointer, rolling back is the same operation in reverse.

Ideally only one component can write to production, and it is the validation step. If a separate step writes, someone will eventually call it without running the checks first.

## Dry run by default

The command that runs a pipeline should, by default, do everything except promote. It stages the output, validates it and reports what it would change. Writing to production requires an explicit flag such as `--execute`.

This reverses the usual risk. Forgetting a flag costs you a rerun, not an incident. It also makes every run a free rehearsal: you see the validation report and the size of the change before you commit to it.

## What it costs

You need storage for the staged copy and some extra seconds for the checks. You also need discipline when a check fails close to a deadline, because the temptation is to loosen the threshold. Don't loosen it under pressure. Promote by hand if you must, write down why, and fix the check afterwards. A threshold that moves whenever it fires stops protecting anything.

In return, bad output gets caught where it's cheap to fix. Over time, the checks also become the most accurate documentation of what the pipeline promises.

[← All notes](../notes.qmd){.back-link}
