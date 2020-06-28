---
title: "Machine Learning Project: Clustering with K-means"
date: 2020-06-27
tags: [machine learning, data science, finding groups of data]
header:
  images: "/images/Background-01.jpg"
excerpt: "Machine Learning, Data Science"
mathjax: true
---
Clustering analysis is used to find groups of similar objects in a dataset. There are two main categories of clustering:

Hierarchical clustering: like agglomerative (hclust and agnes) and divisive (diana) methods, which construct a hierarchy of clustering.
Partitioning clustering: like k-means, which require the user to specify the number of clusters to be generated.
These clustering methods can be computed using the R packages stats (for k-means) and cluster (for pam, clara and fanny), but the workflow require multiple steps and multiple lines of R codes.

In this section, we cluster people into 5 groups using k-means clustering algorithm and implemented ggplot2 method for visualizing the results.


## Creating Dendrogram
```r
df.dist <- dist(df, method = "euclidean")
df.hc <- hclust(df.dist, method = "ward.D2")
plot(df.hc,cex=1,col="blue"
```
<img src="{{ site.url }}{{ site.baseurl }}/images/clusteringwithkmean/Dendrogram.png" alt="K-means clustering">


## Cluster Size
<img src="{{ site.url }}{{ site.baseurl }}/images/clusteringwithkmean/Clustersize.png" alt="K-means clustering">


## Cluster Plot
```r
fviz_cluster(cluster_5, df, palette = "Set2", labelsize = 8 ,main="Cluster Plot", ggtheme = theme_minimal())
```
<img src="{{ site.url }}{{ site.baseurl }}/images/clusteringwithkmean/ClusterPlot.png" alt="K-means clustering">
