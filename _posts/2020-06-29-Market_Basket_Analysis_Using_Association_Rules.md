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
Association rules help us understand the relationships among items in the itemsets.
The analysis involves:
* Using simple performance measures to find associations in large databases
+ Understanding the peculiarities of transactional data
- Knowing how to identify the useful and actionable patterns

## Transaction Summary
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/TransactionSummary_1.png" alt="Market Basket Analysis">
**Findings:**
1. The grocery store data contains 9,835 transactions and 169 different items.
2. Whole milk is the most frequent item, accounting for 25.6% of transactions, followed by other vegetables and rolls/buns.
3. 2,159 transactions contained only a single item, while one transaction had 32 items. Each transaction contains 4.4 items on average.

## Visualizing Item Support - Item Frequency Plots
```r
itemFrequencyPlot(groceries,topN=20,
  main="Support levels for the top 20 grocery items")
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/ItemFrequencyPlots_2.png" alt="Market Basket Analysis">

```r
image(sample(groceries,100))
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/SparseMatrix_1.png" alt="Market Basket Analysis">
**Findings:**
The above matrix diagram shows that a few columns seem fairly heavily populated, indicating some very popular items at the store.

## Training A Model On The Data
```r
groceryrules<-apriori(groceries,parameter=list(support = 0.006, confidence=0.25,minlen=2))
groceryrules
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/Groceryrules_1.png" alt="Market Basket Analysis">
**Findings:**
1. We use Apriori algorithm to find associations among shopping cart items.
2. Set parameters:
"support"=0.006, selecting items that are purchased twice a day (about 60 times in a month of data: 60 out of 9,835 equals 0.006).
"confidence"=0.25, indicating that in order to be included in the results, the rule has to be correct at least 25% of the time.
"minlen=2", eliminating rules that contain fewer than two items.
3. There are 463 association rules.

## Improving Model Performance
### Sorting the set of association rules
```r
inspect(sort(groceryrules, by="lift")[1:5])
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/Sort_1.png" alt="Market Basket Analysis">
**Findings:**
1. The first rule, with a *lift* of about 3.96, implies that people who buy herbs are nearly four times more likely to buy root vegetables than the typical customer - perhaps for a stew of some sort?
2. Rule two shows that whipped cream is over three times more likely to be found in a shopping cart with berries versus other carts, suggesting perhaps a dessert pairing?

### Taking subsets of association rules
```r
berryrules<- subset(groceryrules,items %in% "berries")
inspect(berryrules)
```
<img src="{{ site.url }}{{ site.baseurl }}/images/MarketBasketAnalysis/Subset_1.png" alt="Market Basket Analysis">
**Findings:**
There are four rules involving berries, two of which seem to be interesting enough to be called actionable. In addition to whipped cream, berries are also purchased frequently with yogurt.
