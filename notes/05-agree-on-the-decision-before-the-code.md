---
title: "Agree on the decision before the code"
description: "In methodology-heavy work, the costly mistakes are decisions nobody explicitly made. Put the options and trade-offs in front of the people who own the method before building, and write down what was agreed."
date: 2026-11-14
order: 5
---

In statistical production systems, the code is rarely the hard part. The hard part is the decisions buried inside it. Which records count as eligible? Which definition of a day applies? What happens when two targets conflict? Is it acceptable for the new version to move an estimate by half a percent? Engineers make these calls all the time, often without noticing that they are calls at all.

When those decisions surface only at code review, or after release, they are expensive. The work gets redone, or the method quietly changes without anyone who owns the method having agreed to it. My rule is to lay out the decisions, the options and their trade-offs for the people who own the method before I write the implementation, and to record what we agreed.

## Find the decisions

Most decisions hide in places that look like implementation details:

- Definitions, such as eligibility, time boundaries, or what counts as a duplicate.
- Exclusions: which records are dropped and why.
- Handling of edge cases, such as missing values, conflicting targets, or tiny groups.
- Order of operations, when filtering before or after a join changes the result.
- Tolerances: how much change counts as "the same".
- Defaults that will apply when nobody sets a value.

A useful test: if two careful engineers could reasonably implement the requirement differently, and the outputs would differ, then it's a decision and not a detail.

## Present options, not a proposal

For each decision, write a short note.

1. **The question**, in one sentence, in the method's terms rather than the code's.
2. **Two or three options**, including the current behavior if one exists.
3. **The trade-offs**, with numbers where possible. Run each option on real data and show what moves. "Option B changes this estimate by 0.3% and these two groups by more than 2%" is far easier to decide on than a description.
4. **A recommendation**, clearly labeled as one.

Number the decisions so they can be referred to later. Keep each one short enough that a busy reviewer can decide in a few minutes.

## Get the decision from the right people

Methodology owners decide questions of method. Engineering owners decide questions of operations, cost and maintenance. Most real decisions involve both, so put both in the same conversation. Splitting it means one side signs off on something the other can't support.

The aim isn't consensus on every detail. The aim is that every decision with consequences has an owner who made it knowingly.

## Record it where the code lives

Write the outcome down next to the code: a short decision record with the question, the options, what was chosen, who chose it and the date. Refer to it from the code and from the pull request. Six months later, when someone asks why the pipeline behaves this way, the answer is a link, not an archaeology project.

## Why it pays

- **Less rework.** A decision changed on paper costs minutes. The same change after implementation costs days, plus the risk of what it breaks.
- **Faster reviews.** Reviewers check that the code implements the agreed decisions instead of reopening them.
- **Changes that can be defended.** When a number moves, you can show it moved because of a decision someone made on purpose, and how much it was expected to move.
- **Trust.** Methodology partners stop finding out about method changes from the output.

It feels slower at the start of a project, and it is, by about a day. It's much faster by the end.

[← All notes](../notes.qmd){.back-link}
