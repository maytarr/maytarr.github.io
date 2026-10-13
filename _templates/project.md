---
# Copy this file to _projects/your-project-name.md
# The filename becomes the URL: _projects/wine-quality.md -> /projects/wine-quality/

title: Project title
description: One or two sentences. Shown on the home-page card, under the page title, and in Google results.
date: 2026-10-13          # controls order on the home page (newest first)
status: Completed         # optional, e.g. Completed, In progress, Pilot
tags: [Python, Machine Learning]   # always use [square brackets]
github: https://github.com/maytarr/repo-name   # optional, adds a "View code" button
demo:                     # optional, adds a "Live demo" button
image: /assets/images/your-project-name/cover.png   # optional cover image for the card and page
published: true           # set to false to hide the page while you work on it
---

Start with the problem: what question were you answering, and why does it matter?

## Data

Where the data came from and how you cleaned it.

## Approach

```python
import pandas as pd
df = pd.read_csv("data.csv")
```

Put charts in assets/images/your-project-name/ and show them like this:

![Describe the chart for screen readers](/assets/images/your-project-name/chart.png)

## Results

What you found, in plain words.
