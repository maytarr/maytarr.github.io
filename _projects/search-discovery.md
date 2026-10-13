---
title: Search Discovery Pipeline
description: An automated SEO discovery pipeline that turns a raw keyword list into prioritised, evidence-linked keyword clusters for a fintech product.
date: 2026-06-16
status: Pilot
tags: [Python, Claude API, SEO, Automation]
---

SEO discovery is usually slow, manual work: clean a keyword export, group it by hand, judge what each group means, then decide what to build. This pipeline automates that process for a fintech product.

## What it does

1. **Cleans** the keyword set and removes duplicates.
2. **Clusters** keywords with AI, within the existing keyword taxonomy rather than replacing it.
3. **Classifies** each cluster's search intent and suggests a page type.
4. **Scores** compliance risk, economic suitability and strategic priority.
5. **Links** every cluster to its search-results evidence, so each recommendation can be traced back to its source.

## Design choices

All product-specific settings live in one config file. Pointing the pipeline at a different product means changing that file, not the pipeline code.
