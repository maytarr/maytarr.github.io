---
title: E-commerce Funnel Analysis
description: Where does an online shop lose its shoppers? An analysis of three months of real Google Analytics 4 data from the Google Merchandise Store, using SQL in Google BigQuery.
date: 2026-11-16
status: Completed
tags: [SQL, BigQuery, GA4, Python, E-commerce]
github: https://github.com/maytarr/ecommerce-funnel-analysis
image: /assets/images/ecommerce-funnel/01_funnel.png
image_alt: Bar chart of the shopping funnel, from 267 116 visitors down to 4 419 buyers
---

Every online shop has the same problem: many people visit, but very few buy. The real question is *where* they drop out of the buying process. Is it when they first land on the website, when they look at a product, or halfway through checkout? Each answer points to a different fix.

To find out, I analysed three months of real data (1 November 2020 to 31 January 2021) from the **Google Merchandise Store**, the online shop that sells Google-branded hoodies, mugs and stickers. Google makes this data freely available in **Google BigQuery**, its cloud database, in the same **Google Analytics 4 (GA4)** format that many online shops use. I used **SQL (Structured Query Language)**, the standard language for asking questions of a database, to analyse it.

## How online shops track the buying journey

GA4 records every action a visitor takes as an **event**: starting a visit, viewing a product, adding it to the cart, starting checkout, entering payment details and buying. Lined up in order, these steps form a **funnel**. It is wide at the top, because everyone who visits is counted, and narrow at the bottom, because only a few people buy.

I counted how many visitors reached each step, then compared those numbers by device, by how visitors arrived at the shop, and week by week.

## Key findings

**Most visitors leave long before checkout.** Of 267 116 visitors, only 23% looked at a single product, and only 20% of those added something to their cart. Overall, 1.65% of visitors bought something, which is within the normal range for online shops (roughly 1% to 3%).

**Once people add something to their cart, most go on to buy.** 77% of people who added an item to their cart started checkout.

**The biggest drop-off inside checkout is at the payment step.** Of the 9 715 people who started checkout, 5 751 went on to enter their payment details. That means about 3 960 people (41%) stopped at this point. This is usually the stage where shipping costs and delivery details appear, so unexpected costs are a likely reason. It would be my first choice for an A/B test (see *What I would do next*).

**Mobile and desktop shoppers buy at similar rates.** 1.71% of mobile visitors bought something, compared with 1.62% of desktop visitors. That looks like a difference, but small gaps can appear by chance. A statistical test gave a **p-value** of 0.056, which is the probability of seeing a gap this size if there were no real difference. Analysts usually require a p-value below 0.05 before calling a difference real, so the honest conclusion is that there is no clear difference.

![Purchase rate by traffic source](/assets/images/ecommerce-funnel/02_purchase_rate_by_source.png)

**Visitors who arrive through paid ads are the least likely to buy.** The shop records how each visitor first arrived: through a link on another website, by typing the address, through an unpaid Google search result, or by clicking a paid ad. Of the visitors who arrived through a paid ad, 0.98% bought something, about half the rate of visitors who arrived through a link on another website (1.89%).

That does not automatically mean the ads are a waste of money. Each ad click costs the shop money, but the dataset does not include those costs. If clicks are cheap, a low purchase rate can still be profitable. If they are expensive, the same purchase rate could mean the shop loses money on every sale. Without the cost data, the analysis can show *how well* ad visitors convert, but not *whether the ads pay for themselves*.

![Weekly trend](/assets/images/ecommerce-funnel/03_weekly_trend.png)

**Sales peaked in early December, not on Black Friday.** The busiest week started on 7 December, which was most likely the last safe week to order for Christmas delivery. After 21 December, sales fell sharply. Early January is harder to explain: the shop had almost as many visitors as in peak season, but very few of them bought anything.

## A revenue drop that turned out to be a tracking error

In the last week of January, the average amount spent per buyer fell to about US$18. In every earlier week, it was between about US$60 and US$105. Before reporting this as a finding, I checked the data day by day.

The number of purchases stayed within its usual range, at about 20 to 100 a day. What changed was how they were recorded. Up to 25 January, only a small share of purchases (4% to 13% a day) had a value of US$0, which is normal for free or fully discounted orders. From 26 January, that share rose quickly:

| Date | Purchases recorded as US$0 |
|---|---|
| 18–25 January | 4% to 13% |
| 26 January | 60% |
| 28 January | 93% |
| 31 January | 100% |

The shop did not suddenly start giving its products away. People were still buying, but the tracking stopped recording how much they paid. This means the purchase counts from the last week are still reliable, but the revenue figures are not. I therefore excluded revenue from 26 January onwards from my conclusions.

## What I would do next

**1. Follow each visitor through the steps in order.** At the moment, each step is counted separately: a person counts as "added to cart" if they did it at any point in the three months, even if they never viewed a product beforehand, or did so in a different visit. A stricter funnel would follow each visit and only count a step if the previous step happened first, in the same visit. In SQL, this means finding the time each visitor first reached each step and checking that the times are in the right order. This would give more accurate drop-off rates, and would also show how long people take between steps.

**2. Add advertising costs to work out the cost per purchase.** If the shop's ad spend were available, I could divide it by the number of purchases from ad visitors to get the **cost per purchase**. For example, if 15 000 ad clicks cost US$7 500 (US$0.50 a click) and produced 150 purchases, each purchase cost US$50 to win. If the average order is about US$75, the ads are profitable. If each click costs US$2, the same 150 purchases cost US$200 each, and the ads lose money. *(These costs are made-up examples; the dataset does not include ad spend.)* This would turn "ad visitors buy less often" into a clear answer to the question a business actually cares about: should we keep paying for these ads?

**3. Test whether showing shipping costs earlier reduces the checkout drop-off.** An **A/B test** shows two versions of a page to two randomly chosen groups of visitors at the same time, and compares the results.

- **Version A (current):** shipping costs appear at the payment step.
- **Version B (test):** shipping costs are shown on the cart page, before checkout starts.
- **Main measure:** the share of people who start checkout and go on to buy. Currently this is about 45%.
- **Size of the test:** decide in advance how big an improvement is worth detecting. To reliably detect an increase from 45% to 48%, each version needs about 4 300 checkout visitors. At this shop's traffic, that would take roughly three months.
- **Deciding the winner:** when the planned number of visitors is reached, compare the two purchase rates with the same kind of significance test I used for mobile and desktop. Version B wins only if its rate is higher and the p-value is below 0.05.
- **Side effects to watch:** showing shipping costs earlier could put some people off adding items to their cart at all. So I would also check that the overall share of visitors who buy does not fall.

The SQL queries, results and charts are in the [GitHub repository](https://github.com/maytarr/ecommerce-funnel-analysis).
