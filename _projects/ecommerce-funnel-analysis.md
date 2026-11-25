---
title: E-commerce Funnel Analysis
description: Where does an online shop lose its shoppers? A funnel and conversion analysis of three months of real Google Analytics 4 (GA4) data from the Google Merchandise Store, using SQL in BigQuery.
date: 2026-11-16
status: Completed
tags: [SQL, BigQuery, GA4, Python, E-commerce]
github: https://github.com/maytarr/ecommerce-funnel-analysis
image: /assets/images/ecommerce-funnel/01_funnel.png
image_alt: Bar chart of the shopping funnel, from 267 116 visitors down to 4 419 buyers
---

Getting people to visit an online shop is one thing. Turning them into paying customers is another. The useful question is *where* potential customers drop out of the buying process. Is it when they first land on the website, when they look at a product, or halfway through checkout? Each answer points to a different fix.

To find out, I analysed three months of data (1 November 2020 to 31 January 2021) from the **Google Merchandise Store**, the online shop that sells Google-branded hoodies, mugs and stickers.

Google makes a sample of this shop's data publicly available in **Google BigQuery**, its cloud database. The data was collected through **Google Analytics 4 (GA4)**, the tool many online shops use to track what visitors do on their website. I used **SQL (Structured Query Language)**, the standard language for asking questions of a database, to analyse it.

## How online shops track the buying journey

GA4 records every action a visitor takes as an **event**. Lined up in order, the key events form a **funnel**: wide at the top, because everyone who visits is counted, and narrow at the bottom, because only a few people buy.

I used six events as the six steps of my funnel:

| Funnel step | GA4 event |
|---|---|
| Visited the shop | `session_start` |
| Viewed a product | `view_item` |
| Added to cart | `add_to_cart` |
| Started checkout | `begin_checkout` |
| Entered payment details | `add_payment_info` |
| Bought | `purchase` |

A **visitor** here is one anonymous ID that GA4 gives each browser. One visitor can make several visits over the three months, but I counted each visitor only once per step: for example, "Visited the shop" means the visitor started at least one visit, however many they made.

I then looked at how many visitors moved from one step to the next, and how many of all the original visitors eventually made a purchase. I also compared these patterns by device, by how visitors first arrived at the shop, and week by week.

One simplification is worth knowing. I counted each step separately, so the funnel does not check that a visitor did the steps in order or in the same visit. That is fine for a first overview, and making it stricter is one of my next steps.

## Key findings

**Most visitors leave long before checkout.** Of the 267 116 visitors, 23% viewed at least one product. Of those who viewed a product, 20% added something to their cart. Overall, 1.65% of all visitors bought something, which is within the typical range for online shops (roughly 1% to 3%).

**Once people add something to their cart, most go on to start checkout.** 77% of visitors who added an item to their cart started checkout.

**The biggest drop-off inside checkout is at the payment step.** Of the 9 715 visitors who started checkout, 5 751 went on to enter their payment details. About 3 960 people (41%) stopped at this point.

The data does not say why. Shipping costs or other unexpected charges often first appear at this stage, so they could be part of the reason. That makes this step a good candidate for further investigation and an A/B test.

**There is no clear difference between mobile and desktop.** 4 168 visitors used both a phone and a computer, so they would have been counted in both groups. To make a fair comparison, I set them aside and compared visitors who only ever used one device: 1.43% of the 104 030 mobile-only visitors bought something, compared with 1.42% of the 153 067 desktop-only visitors.

Small gaps like this can appear by chance, so I used a **two-proportion z-test**, a standard test for comparing two percentages. It gave a **p-value** of 0.82. In plain terms: if mobile and desktop visitors really bought at the same rate, a gap at least this large would turn up by chance about 82% of the time. Analysts commonly use a threshold of 0.05 to decide whether a result is statistically significant. At 0.82, the difference between mobile and desktop is not statistically significant.

My first comparison, which counted everyone, made mobile look slightly ahead (1.71% against 1.62%, with a p-value of 0.056). That small gap came from the visitors who used both devices: 16% of them bought something, compared with about 1.4% of everyone else. This does not mean that using two devices makes people buy. A more likely explanation is that people who are seriously considering a purchase come back several times, on whichever device is at hand.

![Purchase rate by traffic source](/assets/images/ecommerce-funnel/02_purchase_rate_by_source.png)

**Visitors who first arrived through paid ads were the least likely to buy.** GA4 records how each visitor first arrived at the shop: through a link on another website, by typing the address, through an unpaid Google search result, or by clicking a paid ad.

Of the visitors whose first visit came from a paid ad, 0.98% bought something. That is about half the rate of visitors who first arrived through a link on another website (1.89%), and lower than unpaid search (1.19%) or typing the address (1.40%).

Two cautions apply:

- **Only the first visit counts.** GA4 records the source of each visitor's first visit, so someone who first clicked an ad and later came back through Google search still counts as an ad visitor. Google has also anonymised the source for about a fifth of visitors, shown in grey in the chart.
- **A purchase rate does not show whether ads are worth paying for.** The shop pays for every ad click, but the dataset does not include those costs. The analysis can show *how often* ad visitors buy, but not *whether the ads pay for themselves*.

![Weekly trend](/assets/images/ecommerce-funnel/03_weekly_trend.png)

**Sales peaked in early December, not in the Black Friday week.** The week starting 7 December had the most visitors (31 083), buyers (737) and revenue. One possible explanation is that it was the last week to order in time for Christmas delivery, although the data cannot confirm this.

After 21 December, sales fell sharply. Early January is the most puzzling period: in the week starting 4 January, the shop had about four in five as many visitors as in the peak week (24 493), but less than a quarter of the buyers (161).

## A revenue drop that was not what it seemed

In the last week of January, the average amount spent per buyer fell to about US$18. In every earlier week, it was between about US$60 and US$105. Before reporting this as a finding, I checked the data day by day.

The number of purchases stayed within its usual range, at about 20 to 100 a day. What changed was how they were recorded.

Up to 25 January, only a small share of purchases (4% to 13% a day) had a value of US$0, which is normal for free or fully discounted orders. From 26 January, that share rose quickly:

| Date | Purchases recorded as US$0 |
|---|---|
| 18–25 January | 4% to 13% |
| 26 January | 60% |
| 28 January | 93% |
| 31 January | 100% |

This does not mean the shop had started giving its products away. Purchases were still being recorded at their usual rate, but the revenue values attached to them had become unreliable. The most likely explanation is a tracking problem, although the data alone cannot confirm the exact cause.

I therefore kept the purchase counts and purchase rates for those days, but left their revenue out of my conclusions. The weekly chart shows that week's revenue in grey, and none of the findings above rely on it.

## What I would do next

- **Build a sequential funnel.** Follow each visit through the buying journey and only count a step if the previous one happened first, in the same visit. This would give a more accurate picture of where visitors drop off.
- **Assess whether the ads pay for themselves.** Combine advertising costs with sales data to calculate the cost per purchase and the return on ad spend for each traffic source.
- **Test showing shipping costs earlier.** Run an A/B test to find out whether showing shipping costs on the cart page, rather than at payment, improves checkout completion without lowering the overall purchase rate.

The full SQL analysis, supporting charts and additional project documentation are available in the [GitHub repository](https://github.com/maytarr/ecommerce-funnel-analysis).
