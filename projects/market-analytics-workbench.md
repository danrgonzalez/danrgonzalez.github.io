---
title: "Market Analytics Workbench"
description: "A local analytics workbench for equities, options, and crypto, with statistically corrected signal studies and LLM analysis over an engineered context."
categories: [Python, Streamlit, LLMs, Statistical inference, Testing]
order: 1
---

A personal analytics workbench, built in **Python and Streamlit**, that pulls quotes, price history, and option chains from a market-data REST API and a public crypto API. From those it computes technical indicators, options analytics (implied volatility, Greeks, gamma and delta exposure, liquidity, unusual activity), and a quarterly fundamentals panel with earnings dates from **SEC EDGAR** filings and its own next-quarter EPS forecasts.

## What makes it interesting

**Corrected inference.** Signal and seasonality studies are tested against resampling nulls: a circular-shift null that preserves overlap in forward returns and signal persistence, and label-permutation nulls, with a circular block bootstrap for intervals. Multiple-testing control uses Holm, Benjamini–Hochberg, and Westfall–Young-style max-statistic p-values. On simulated random walks, the corrections bring the day-of-week false-alarm rate down from 21% to the nominal 5%.

**Walk-forward validation.** An earnings-date estimator is checked walk-forward against dated SEC filings since 2021. It places 79% of releases in the correct week, versus 68% for the approach it replaced, and 96% fall inside its two-week window.

**LLM context engineering.** A context builder condenses everything the dashboard computes into a structured context block of about 4,000 tokens. Written guidelines keep that context consistent with what's on screen, prefer computed signals over raw aggregates, and set token budgets. Analyst agents share that context, and a side-by-side mode runs two to six Claude model and effort-level configurations on the same context. An optional research mode uses server-side web search, with its token cost measured and, with prompt caching, modeled at roughly a quarter of the uncached cost.

**Tested like production code.** More than 1,000 offline, deterministic tests (seeded randomness, no clock reads) run as a pre-commit hook alongside formatting, linting, and private-key detection.

## Stack

[Python · Streamlit · pandas · NumPy · SciPy · Plotly · Anthropic Claude API · SEC EDGAR · pytest · pre-commit]{.stack}

*Source is private. This is a personal tool, not investment advice.*

[← All projects](../projects.qmd){.back-link}
