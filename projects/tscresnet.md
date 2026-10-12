---
title: "TSCResNet: Residual CNN for Activity Recognition"
description: "Doctoral research: a 1-D residual CNN for wearable-sensor activity recognition, with peer-reviewed closed-set and open-set results."
categories: [PyTorch, Deep learning, Time series, Research]
order: 2
---

**TSCResNet** is a one-dimensional residual convolutional network for time-series classification, developed during my doctoral research and applied to human activity recognition on the public [PAMAP2](https://doi.org/10.24432/C5NW2H) wearable-sensor benchmark.

## How it works

- **Input.** 5.12-second windows of 40 sensor channels (three inertial measurement units plus heart rate) × 172 time steps.
- **Architecture.** Strided convolutions and bottleneck residual blocks (a width-1 convolution that halves the channels, a width-3 convolution that restores them, plus the skip connection), with batch normalization and leaky ReLU. The layer stack is defined declaratively in a config and built programmatically.
- **No pooling or dense head.** The final convolutions shrink the time axis to exactly as many positions as there are classes, and those positions serve directly as the class logits.
- **Open set.** For the open-set study the model is trained once per activity with that activity held out, and a softmax-confidence threshold rejects windows that look like none of the known activities.

## Results

On the standard held-out-subject split (closed set), TSCResNet reached **95.6% accuracy, 95.5% weighted F1, and 95.5% mean F1**, improving on the best previously published results it was compared against (93.7% accuracy). The open-set follow-up is published in IEEE CSCE 2023. See [Research](../research.qmd) for both papers.

## Engineering

Experiments are tracked in MLflow (hyperparameters, training loss, and per-epoch validation and test accuracy and F1), and the training environment is containerized with Docker.

[PyTorch · scikit-learn · NumPy · pandas · MLflow · Docker]{.stack}

[Code on GitHub](https://github.com/danrgonzalez/TSCResNet)

[← All projects](../projects.qmd){.back-link}
