---
title: E-commerce Funnel Analysis
description: Where does an online shop lose its shoppers? A funnel and conversion analysis of three months of real GA4 data from Google's online merchandise store, in SQL on BigQuery.
date: 2026-11-16
status: Completed
tags: [SQL, BigQuery, GA4, Python, E-commerce]
github: https://github.com/maytarr/ecommerce-funnel-analysis
image: /assets/images/ecommerce-funnel/01_funnel.png
image_alt: Bar chart of the shopping funnel, from 267,116 visitors down to 4,419 buyers
---

Every online shop has the same problem: lots of people visit, very few buy. The question is *where* they give up. Is it when they first land, when they look at a product, or halfway through checkout? Each answer points to a different fix.

To find out, I analysed three months of real data from the **Google Merchandise Store** (the shop that sells Google-branded hoodies, mugs and stickers). Google publishes this data for free in BigQuery, in the same GA4 format that most online shops use.

## How a shop "sees" its visitors

GA4 records every action a visitor takes as an **event**: starting a visit, viewing a product, adding to cart, starting checkout, entering payment details, buying. String those steps together and you get a **funnel**: wide at the top (everyone who visits), narrow at the bottom (the few who buy). I wrote SQL queries to count how many people reached each step, then split those counts by device, by how visitors arrived, and by week.

## What I found

**Most shoppers are lost long before checkout.** Of 267,116 visitors, only 23% looked at a single product, and only 20% of those added something to their cart. Overall, 1.65% of visitors bought something, which is within the normal range for online shops.

**Once people commit, most follow through.** 77% of people who add to cart start checkout. The one leak inside checkout is between starting it and entering payment details, where 41% leave. That's usually where shipping costs appear, so it would be my first candidate for an A/B test.

**Phone or computer barely matters.** Mobile visitors bought at 1.71% and desktop visitors at 1.62%. That looks like a difference, but a statistical test says it could easily be chance (p = 0.056), so the honest answer is "no clear difference".

![Purchase rate by traffic source](/assets/images/ecommerce-funnel/02_purchase_rate_by_source.png)

**Visitors from ads buy least.** People who clicked a paid ad bought about half as often as people who arrived through a link on another website. The data has no ad spend, so I can't say whether the ads pay for themselves. That would need ad-platform cost data joined to these results.

![Weekly trend](/assets/images/ecommerce-funnel/03_weekly_trend.png)

**The peak wasn't Black Friday.** The busiest week was the one starting 7 December, most likely the last safe week to order for Christmas delivery. After that, sales collapse. Early January is a puzzle: nearly as many visitors as the peak, but very few buyers.

## The number that didn't add up

In the last week of January, revenue per buyer suddenly dropped from about $75 to $18. Rather than report that as a finding, I checked it day by day. People were still buying at the usual rate, but from 26 January more and more purchases were recorded as **$0**, until on 31 January all of them were. It was a tracking problem, not a change in shopping, so I excluded that revenue from my conclusions.

## What I'd do next

- Follow individual visitors step by step through the funnel, instead of counting each step separately.
- Join ad-spend data to work out cost per purchase for each traffic source.
- Design an A/B test for the checkout step, for example showing shipping costs earlier.

The SQL queries, results and charts are in the [GitHub repository](https://github.com/maytarr/ecommerce-funnel-analysis).
