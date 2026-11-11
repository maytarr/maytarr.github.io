---
title: Web Data Extraction
description: A growing series of Python scrapers, from simple websites to JavaScript-rendered pages, building up to a real-world data collection project.
date: 2026-06-17
status: Ongoing series
tags: [Python, Web Scraping, Automation]
github: https://github.com/maytarr/web-data-extraction
---

A lot of useful data lives on websites rather than in neat downloadable files. This series is about getting that data out, cleanly and politely, and into a format you can actually analyse.

## The series so far

**1. Static websites.** Scrapers for two sandbox sites built for learning: one collects all 1,000 books from [Books to Scrape](http://books.toscrape.com/) (title, price, stock and star rating), and the other collects 100 quotes from [Quotes to Scrape](http://quotes.toscrape.com/). They follow the "next page" links until there are none left, pause between requests so they don't overload the site, retry when a request fails, and save the results as CSV, a SQLite database and Excel.

**2. JavaScript-rendered websites.** The [JavaScript version of Quotes to Scrape](https://quotes.toscrape.com/js/) doesn't put its quotes in the page the usual way, so a normal scraper finds nothing. The quotes turned out to be hidden inside the page's code as a block of data, which the scraper finds, cuts out and converts into a table.

**3. A real-world project.** Coming next.

The code for each part is in the [GitHub repository](https://github.com/maytarr/web-data-extraction).
