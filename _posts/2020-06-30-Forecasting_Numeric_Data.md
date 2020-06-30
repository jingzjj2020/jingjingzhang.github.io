---
title: "Predicting Medical Expenses Using Linear Regression"
date: 2020-06-30
tags: [Machine Learning, Regression Methods]
header:
  images: "/images/Background-01.jpg"
excerpt: "Machine Learning"
mathjax: true
---
## Exploring Relationships Among Features - The Correlation Matrix
```r
cor(insurance[c("age","bmi","children","expenses")])
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/Correlation_1.png" alt="Regression Methods">
**Findings:**
None of the correlations in the matrix are very strong, but there are some notable associations. For instance, *age* and *bmi* appear to have a weak positive correlation, meaning that as someone ages, their body mass tends to increase. There are also positive correlation between *age* and *expenses*, *bmi* and *expenses*, and *children* and *expenses*. These associations imply that as age, body mass, and number of children increase, the expected cost of insurance goes up.

## Visualizing Relationships Among Features - The Scatterplot Matrix
```r
pairs(insurance[c("age","bmi","children","expenses")])
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/ScatterplotMatrix_1.png" alt="Regression Methods">
**Findings:**
The relationship between *age* and *expenses* displays several relatively straight lines, while the *bmi* versus *expenses* plot has two distinct groups of points.

```r
pairs(insurance[c("age","bmi","children","expenses")])
pairs.panels(insurance [c("age","bmi","children","expenses")])
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/ScatterplotMatrix_2.png" alt="Regression Methods">
**Findings:**
1. The ellipse for *age* and *expenses* in much more stretched, reflecting its stronger correlation (0.30).
 age.
2. THe curve for *age* and *children* is an upside-down U, peaking around middle age. This means that the oldest and youngest people in the sample have fewer children on the insurance plan than those around middle

## Training A Model On The Data
```r
ins_model<-lm(expenses ~ ., data=insurance)
ins_model
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/LinearRegression_1.png" alt="Regression Methods">
**Findings:**
1. For each additional year of age, we would expect $256.80 higher medical expenses on average, assuming everything else is held equal.
2.  Each additional child results in an average of $475.70 in additional medical expenses each year, and each unit increase in BMI is associated with an average increase of $339.30 in yearly medical expenses, all else equal.

## Evaluating Model Performance
```r
summary(ins_model)
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/LinearRegression_2.png" alt="Regression Methods">
**Findings:**
1. The *Residuals* section provides summary statistics for the errors in our predictions. The majority of predictions were between $2,850.90 over the true value and $1383.90 under the true value.
2. For each estimated regression coefficient, the *p-value* provides an estimate of the probability that the true coefficient is zero given the value of the estimate. Our model has several highly significant variables, such as age, male, bmi, children, and smoker (p-values with stars ).  
3. The *Multiple R-squared* value provides a measure of how well our model as a whole explains the values of the dependent variable. Since the R-squared value is 0.7494, we know that the model explains nearly 75% of the variation in the dependent variable.
