---
title: "Market Basket Analysis Using Association Rules"
date: 2020-06-29
tags: [machine learning, marketing analysis]
header:
  images: "/images/Background-01.jpg"
excerpt: "Machine Learning"
mathjax: true
---
In this section, we are using Market Basket Analysis using association rules to learn purchasing patterns in the massive transaction data bases of large retailers. For example, on a late night trip for diapers and formula you picked up a six-pack of beer. You buys are no coincidence, as retailers use sophisticated data analysis techniques to identify patterns that will drive retail behavior.
Specifically, association rules help us understand the relationships among items in the itemsets.
The analysis involves:
* Using simple performance measures to find associations in large databases
+ Understanding the peculiarities of transactional data
- Knowing how to identify the useful and actionable patterns

## Transaction Summary
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/TransactionSummary_1.png" alt="Market Basket Analysis">
### Findings:
1. The grocery store data contains 9,835 transactions and 169 different items.
2. Whole milk is the most frequent item, accounting for 25.6% of transactions, followed by other vegetables and rolls/buns.
3. 2,159 transactions contained only a single item, while one transaction had 32 items. Each transaction contains 4.4 items on average.

## Visualizing item support - item frequency plots
```r
itemFrequencyPlot(groceries,topN=20, main="Support levels for the top 20 grocery items")
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/ItemFrequencyPlots_2.png" alt="Market Basket Analysis">

```r
image(sample(groceries,100),main="Visualization of the sparse matrix for 100 randomly-selected transactions")
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/SparseMatrix_1.png" alt="Market Basket Analysis">
### Findings:
The above matrix diagram shows that a few columns seem fairly heavily populated, indicating some very popular items at the store.

## Training a model on the data
