---
title: "Evaluating activity recognition honestly"
description: "Lessons from my doctoral research on wearable-sensor models: split by person, report more than accuracy, compare under the same protocol, and test on activities the model has never seen."
date: 2026-11-14
order: 6
---

Human activity recognition classifies what a person is doing, such as walking, cycling or climbing stairs, from wearable sensors. Published accuracies in this area are often very high. Many of them would not survive contact with a new user. My doctoral research on [TSCResNet](../projects/tscresnet.md) taught me that the evaluation deserves as much care as the model.

## Split by person, not by window

Sensor streams are cut into short, overlapping windows, and each window becomes one example. If you shuffle the windows and split them randomly, neighboring windows from the same recording land in both training and test. They are nearly identical, so the model can score well by recognizing the person and the session rather than the activity.

The fix is to hold out whole subjects: train on some people and test on people the model has never seen. That is the question a deployment actually asks. On the public [PAMAP2](https://doi.org/10.24432/C5NW2H) benchmark I used the standard held-out-subject split. Expect a held-out-subject number to be lower than a random-split number on the same data. The lower number is the honest one.

## Report more than accuracy

Activity datasets are imbalanced: people spend far more time walking than rope jumping. Accuracy rewards getting the common activities right and can hide a model that fails on the rare ones. I report accuracy together with weighted F1 and mean (macro) F1. Mean F1 gives every activity equal weight, so a model can't hide a weak class behind a strong majority. When the three numbers diverge, look at the per-class results.

## Compare under the same protocol

A new result is comparable with a published one only if the preprocessing, windowing, sensor channels and split match. Otherwise, the difference you report may come from the protocol rather than the model. For both papers I used the standard protocols of the prior work I compared against. A smaller gain under the same protocol is worth more than a larger one under a different protocol.

## Test what the model hasn't seen

Closed-set evaluation assumes every activity at test time appeared in training. Real users don't cooperate: they do things the model was never trained on, and a closed-set model will confidently force each of them into some known class.

Open-set evaluation tests that directly. In my follow-up study the model was trained once per activity with that activity held out, and a confidence threshold on the softmax output rejected windows that looked like none of the known activities. Each run then asks two questions: does the model still recognize the known activities, and does it reject the one it has never seen?

Two cautions:

- **Choose the threshold without the unknown class.** If you tune it on the held-out activity, the "unknown" isn't unknown any more, and the result is optimistic.
- **Report the trade-off, not a single point.** A stricter threshold rejects more unknowns but also more known activities. Show both sides.

## The general lesson

Each of these choices makes a model look worse in the paper and better in the field. A benchmark measures a model under a particular set of assumptions. Before trusting the result, check that those assumptions match how the model will actually be used.

[← All notes](../notes.qmd){.back-link}
