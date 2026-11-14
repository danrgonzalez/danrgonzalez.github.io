---
title: "Running coding agents under a verification playbook"
description: "How I work with AI coding agents: a written playbook where every rule traces back to a mistake, and verification is part of every task."
date: 2026-11-14
order: 3
---

AI coding agents do much of my implementation work now. The agent writes the code; I frame the problem, make the judgment calls and review the result. What makes this reliable isn't the model. It's a written playbook that the agent reads at the start of every session.

## Every rule traces to a mistake

The playbook isn't a style guide written in advance. It grows out of corrections. When the agent, or I, gets something wrong, the lesson goes into the playbook that day, with a short dated note on what happened. Typical rules look like this:

- Before claiming a speedup, time both versions on the same input.
- Parse dates once when loading data. Never filter by comparing date strings.
- After writing a file, read it back before reporting success.

A rule with its reason attached gets followed more consistently, by agents and by people. It's also easier to delete once the reason no longer applies, which keeps the playbook short.

## Verification is part of the task

Each task's definition of done includes the evidence. That means the test run and its output, the query that confirms the row counts, and the before-and-after numbers. "This should work" doesn't count as done. A claim about the data needs a query behind it, and a claim about the code needs a test or a pointer to the line.

This matters more in statistical work than elsewhere. When correctness is a distribution rather than an exact value, a wrong result looks a lot like a right one.

## Prove, don't explain

Agents are very good at producing plausible explanations, and that is the risk. "Solver noise" and "late-arriving data" sound right whether or not they are. The playbook tells the agent to treat an unexplained difference as a bug and trace it to a cause, and to label a guess as a guess. (I make the same argument for pipeline migrations in [Reproduce before you replace](01-reproduce-before-you-replace.md).)

## A fixed task lifecycle

Every task follows the same steps:

1. **Intake.** The agent restates the task and lists its assumptions and open questions.
2. **Plan.** For anything nontrivial, we agree on the approach before any code is written.
3. **Implement** in small steps that can each be checked.
4. **Verify** with evidence, as above.
5. **Review**, then open a pull request.

I take part at two points: agreeing on the plan and reviewing the result. Everything between them is the agent's job.

## Review in more than one pass

For larger changes, review runs in stages:

1. Several reviewer agents run in parallel, each with one focus: correctness, tests, performance or security.
2. A fresh agent fact-checks every finding against the code, because reviewers hallucinate too.
3. Type checkers and linters run as part of the review. They have overruled me as often as they have overruled the agent.
4. I do the final review myself.

## What I've learned

- **Short beats long.** Remove rules that no longer catch anything.
- **Write rules that can be checked.** "Be careful with dates" changes nothing; "parse dates once at load" does.
- **Speed amplifies whatever verification you have.** With tests, drift gates and a reproduced baseline, a fast agent is safe. Without them, it produces wrong numbers faster.

[← All notes](../notes.qmd){.back-link}
