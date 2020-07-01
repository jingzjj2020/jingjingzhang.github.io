---
title: "Identifying Risky Bank Loans Using C5.0 Decision Trees"
date: 2020-06-30
tags: [Classification]
header:
  images: "/images/Background-01.jpg"
excerpt: "Machine Learning"
mathjax: true
---

The global financial crisis of 2007-2008 highlighted the importance of transparency and rigor in banking practices. As the availability of credit was limited, banks tightened their lending systems and turned to machine learning to more accurately identify risky loans.
Decision trees are widely used in the banking industry due to their high accuracy and ability to formulate a statistical model in plain language. Since governments in many countries carefully monitor the fairness of lending practices, executives must be able to explain why one applicant was rejected for a loan while another was approved.

## Exploring And Preparing The Data
```r
prop.table(table(credit$default))
```
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/DefaultRate_2.png" alt="Classification Trees">
**Findings:**
A total of 30% of the loans in this dataset went into default. A hight rate of default is undesirable for a bank because it means that the bank is unlikely to fully recover its investment. If we are successful, our model will identify applicants who are at high risk of default, allowing the bank to refuse the credit request before the money is given.
## Creating Random Training And Test Datasets
```r
RNGversion("3.5.2")
set.seed(123)
train_sample<-sample(1000,900)
str(train_sample)
credit_train<-credit[train_sample, ]
credit_test<-credit[-train_sample, ]
```
## Training A Model On The Data
```r
credit_model<-C5.0(credit_train[-17],credit_train$default)
credit_model
```
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/TreeSummary_0.png" alt="Classification Trees">
**Findings:**
The tree size is 57, which indicates that the tree is 57 decisions deep.
```r
summary(credit_model)
```
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/TreeSummary_1.png" alt="Classification Trees">
**Findings:**
The above output shows some of the first branches in the decision tree.
1. If the checking account balance is unknown or greater than 200 DM, then classify as "not likely to default."
2. Otherwise, if the chekcing account balance is less than zero DM or between one and 200 DM...
3. ... and the credit history is perfect or very good, then classify as "likely to default."
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/TreeSummary_2.png" alt="Classification Trees">
**Findings:**
After the tree, the summary output displays a confusion matrix, which is a cross-tabulation that indicates the model's incorrectly classified records in the training data.
The *Errors* heading shows that the model correctly classified about 133 of the 900 training instances for an error rate of 14.8%. A total of 35 actual *no* values were incorrectly classified as *yes* (false positives), while 98 *yes* (false negatives) values were misclassified as *no*.

##Evaluating Model Performance
Given the tendency of decision trees to overfit to the training data, the error rate reported here, which is based on training data performance, may be overly optimistic. Therefore, it's especially important to continue our evaluation by applying our decision tree to a test dataset.
```r
credit_pred<-predict(credit_model, credit_test)
CrossTable(credit_test$default,credit_pred,
           prop.chisq = FALSE, prop.c = FALSE,
           prop.r = FALSE, dnn=c('actual default',
           'predicted default'))
```
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/TreeSummary_3.png" alt="Classification Trees">
**Findings:**
Out of the 100 loan applications in the test set, our model correctly predicted that 59 didn't default and 14 did default, resulting in an accuracy of 73% and an error rate of 27%. The model only correctly predicted 14 our of 33 actual loan defaults in the test data, or 42%. This type of error is potentially a very costly mistake, as the bank loses money on each default.

## Improving Model Performance - Boosting The Accuracy Of Decision Trees
```r
credit_boost10<-C5.0(credit_train[-17],credit_train$default,
                     trials = 10)
summary(credit_boost10)
```
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/ImprovingModelPerformance_2.png" alt="Classification Trees">
**Findings:**
The classifier made 34 mistakes on 900 training examples for an error rate of 3.8%. This is quite an improvement over the 13.9% training error rate we noted before adding boosting. Let's see whether we have a similar improvement on the test data:
```r
credit_boost_pred10<- predict(credit_boost10,credit_test)
CrossTable(credit_test$default,credit_boost_pred10,
           prop.chisq=FALSE, prop.c=FALSE, prop.r = FALSE,
           dnn=c('actual default','predicted default'))
```
<img src="{{ site.url }}{{ site.baseurl }}/images/ClassificationTrees/ImprovingModelPerformance_3.png" alt="Classification Trees">
**Findings:**
We reduced the total error rate from 27% prior to boosting to 18% in the boosted model.
