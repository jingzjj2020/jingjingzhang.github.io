---
title: "Predicting Medical Expenses Using Linear Regression"
date: 2020-06-30
tags: [Regression Methods]
header:
  images: "/images/Background-01.jpg"
excerpt: "Machine Learning"
mathjax: true
---
In order for a health insurance company to make money, it needs to collect more in yearly premiums than it spends on medical care to its beneficiaries. Consequently, insurers  invest a great deal of time and money to develop models that accurately forecast medical expenses for the insured population.
The goal of this analysis is to use patient data to forecast the average medical care expenses for such population segments. These estimates could be used to create actuarial tables that set the price of yearly premiums higher or lower according to the expected treatment costs.
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
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/LinearRegression_2.png" alt="Regression Methods">
**Findings:**
1. The *Residuals* section provides summary statistics for the errors in our predictions. The majority of predictions were between $2,850.90 over the true value and $1383.90 under the true value.
2. For each estimated regression coefficient, the *p-value* provides an estimate of the probability that the true coefficient is zero given the value of the estimate. Our model has several highly significant variables, such as age, male, bmi, children, and smoker (p-values with stars ).  
3. The *Multiple R-squared* value provides a measure of how well our model as a whole explains the values of the dependent variable. Since the R-squared value is 0.7494, we know that the model explains nearly 75% of the variation in the dependent variable.

## Improving Regression Model - Adding Interaction Effects
```r
insurance$age2<-insurance$age^2
insurance$bmi30<-ifelse(insurance$bmi>=30,1,0)
ins_model2<-lm(expenses~ age + age2 + children + bmi
  + sex + bmi30 * smoker + region, data=insurance)
summary(ins_model2)
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/LinearRegression_3.png" alt="Regression Methods">
**Findings:**
1. Relative to the first model, the R-squared value has improved from 0.75 to about 0.87, indicating that our model is now explaining 87% of the variation in the medical treatment costs.
2. The higher-order age2 term is statistically significant, as is the obesity indicator, bmi30. The interaction between obesity and smoking suggests a massive effect; in addition to the increased costs of over $13,404 for smoking alone, obese smokers spend another $19,810 per year. This may suggest that smoking exacerbates diseases associated with obesity.

## Making Predictions With A Regression Model
```r
insurance$pred<-predict(ins_model2, insurance)
cor(insurance$pred,insurance$expenses)
plot(insurance$pred,insurance$expenses)
```
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/LinearRegression_4.png" alt="Regression Methods">
<img src="{{ site.url }}{{ site.baseurl }}/images/RegressionMethods/LinearRegression_5.png" alt="Regression Methods">
**Findings:**
The correlation of 0.93 suggests a very strong relationship between the predicted and actual values. This is a good sign - it suggests that the model is highly accurate! It can also be useful to examine this finding as a scatterplot.
