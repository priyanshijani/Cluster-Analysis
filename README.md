# Clustering Analysis: numerical & categorical data

### Table of Contents

### K-Mean Clustering: Grouping European Countries together (with Numerical Variables)
1. Package installation in R 
2. Grouping country in Europe together? 
3. Normalisation 
4. Dissimilarity Measurement – Euclidean 
5. Determine the number of clusters 
6. Visualize & study the clusters 
### K-Mean Clustering: Grouping Beer Companies together (with Categorical Variables)
1. Understanding the Data
2. Determine the number of clusters
3. Determining the clusters
4. Visualize & study the clusters 

# K-Mean Clustering: Grouping European Countries together (with Numerical Variables)


In this project, I will attempt how to establish a pattern from data using K-Mean clustering. This
is one of the method in unsupervised learning in Machine learning.

## Package installation in R


To start with, install the following package which is necessary for determination of the right
number of k in k-mean clustering. Subsequently, add it to your R library:


NbClust package provides 30 indices for determining the number of clusters and proposes to
user the best clustering scheme from the different results obtained by varying all combinations
of number of clusters, distance measures, and clustering methods.
Once the package has been download and installed, we will import the dataset about European
countries.

```
install.packages ("NbClust")
library ("NbClust")
```
```
install.packages ("fastcluster")
library ("fastcluster")
```
```
install.packages ("cluster")
library ("cluster")
```

## Grouping country in Europe together?

We will use k-mean clustering to group countries in Europe together based on their economic
statistic. The idea is to create a group representation of country that are similiar together.

The dataset used is 'Europe.csv'. The dataframe should have 22 rows and 5 columns and is a data about basic information of
countries in Europe. (This is older dataset so the number may no longer reflect reality)

```
Name Population GDP Unemployment MinWage
Belgium 11258434 119 8.8 1501.
Bulgaria 7202198 45 9.6 184.
Croatia 4225316 59 15.4 395.
Czech Republic 10538275 84 4.8 331.
Estonia 1313271 73 5.7 390.
France 66352469 107 10.8 1457.
Germany 81174000 124 4.5 1473.
Greece 10812467 72 24.6 683.
Hungary 9849000 68 6.5 332.
Ireland 4625885 132 9.1 1461.
Latvia 1986096 64 9.9 360.
Lithuania 2921262 74 9.6 300.
Luxembourg 562958 263 5.9 1922.
Malta 429344 85 5.1 720.
Netherlands 16900726 130 6.8 1501.
Poland 38005614 68 7.2 409.
Portugal 10374822 78 12.3 589.
Romania 19861408 54 6.8 217.
Slovakia 5421349 76 11.1 380.
Slovenia 2062874 83 9.3 790.
Spain 46439864 93 21.8 756.
United Kingdom 64767115 108 5.2 1378.
```
Inside the data set, there are five variables.

1. Name - The name of the country.
2. Population - The size of population.
3. GDP - GDP as a unit of 1000 Euro
4. Unemployment - Percentage of population that is unemployed.
5. MinWage - The minimum wage in Euro.

Aside from the name, the rest of the variables are characteristic that can be use to determine
how country is similar or not similar to each other.
```
ed <- **read.csv** ("europe.csv", header=TRUE, stringsAsFactors = T)
ed
```


To conduct k-mean clustering, the function that you need to use is kmeans(). This is part of the
stats build-in package in R.
However, the function does not take in the original dataframe of the data about Europe, instead
you must provide them with dissimilarity matrix based on dist() similar to hierarchical cluster.
This must be calcu- lated based on normalise distance as well.

## Normalisation

Start by normalising all values in the dataframe. The reason for this is that the scale of each variables are
different and it is important to ensure that it is not a factor in the calculation. In this example, we will be using
min max normalization:


Essentially, the statistical formula for a min-max normalisation is as follow:
```
(X – min(X))/(max(X) – min(X))
```
For each value of a variable, we simply find how far that value is from the minimum value, then
divide by the range. We create a function for this then use lapply to apply the function to
numerical column in our dataset.
The data is now normalised.

```
head (ed)
```
```
Name Population GDP Unemployment MinWage
Belgium 0.1341153 0.3394495 0.2139303 0.
Bulgaria 0.0838799 0.0000000 0.2537313 0.
Croatia 0.0470121 0.064220 2 0.5422886 0.
Czech Republic 0.1251963 0.1788991 0.0149254 0.
Estonia 0.0109472 0.1284404 0.0597015 0.
France 0.8164395 0.2844037 0.3134328 0.
```
## Dissimilarity Measurement – Euclidean

The next step is to then calculate dissimilarity between each of the country over each of the
characteristic. To do this, we can use dist function. This function computes and returns the
distance matrix using the specified distance measure (there is a choice of ‘Euclidean’, ‘Manhattan’,
etc) to compute the distances between each rows of a data frame.

```
dissimatrix <- dist (ed[ 2 : 5 ])
```

The output is the Euclidean distance between each country to another country based on the
variable mentioned.

## Determine the number of clusters


As K-mean clustering is a clustering method that requires deter- mination of number of cluster
k before the process. We identify K using NbClust. NbClust provided the following information
using variety of methods to select K. The final ‘recommendation’ is based on the most popular

```
min_max_normalisation <- function (x) {
(x - min (x)) / ( max (x) - min (x)) }
```
```
ed[, 2 : 5 ] <- ( lapply (ed[, 2 : 5 ], min_max_normalisation))
```

method according to all of these calculation:
“kl”, “ch”, “hartigan”, “ccc”, “scott”, “marriot”, “trcovw”, “tracew”, “friedman”, “rubin”, “cindex”, “db”,
“silhouette”, “duda”, “pseudot2”, “beale”, “ratkowsky”, “ball”, “ptbiserial”, “gap”, “frey”, “mcclain”,
“gamma”, “gplus”, “tau”, “dunn”, “hubert”, “sdindex”, “dindex”, “sdbw”, “all”

```
res <- **NbClust** (ed[ 2 **:** 5 ], min.nc= 2 , max.nc= 15 , method="kmeans")
```
![image](https://github.com/user-attachments/assets/1550168b-cb0c-49e0-a0b9-934f8e823419)

```
## *** : The Hubert index is a graphical method of determining the number of clusters.
## In the plot of Hubert index, we seek a significant knee that corresponds
to a
## significant increase of the value of the measure i.e the significant peak
in Hubert ## index second differences plot.
##
```
![image](https://github.com/user-attachments/assets/02017ca4-cb67-4ec9-89a5-15f1a8867431)

```
## *** : The D index is a graphical method of determining the number of clusters.
## In the plot of D index, we seek a significant knee (the significant peak
in Dindex ## second differences plot) that corresponds to a significant increase of
the value of ## the measure.
##
##
*************************************************************
****** ## * Among all indices:
## * 3 proposed 2 as the best number of clusters
## * 2 proposed 3 as the best
number of clusters ## * 9 proposed 4 as
the best number of clusters ## * 1
proposed 12 as the best number of
clusters ## * 2 proposed 13 as the best
number of clusters ## * 3 proposed 14
as the best number of clusters ## * 3
proposed 15 as the best number of
clusters ##
## *****
Conclusion ***** ##
## * According to the majority rule, the best number of
clusters is 4 ##
##
## *******************************************************************
```
```
res **$** Best.nc
```

```
##	KL	CH Hartigan	CCC	Scott Marriot TrCovW TraceW
## Number_clusters 14.0000	4.0000	12.0000 4.0000 13.0000	4.0000 3.0000 4.0000
## Value_Index	19.3465 24.4839	14.2573 6.1467 91.5002	0.0844 0.6886 0.8257
##	Friedman		Rubin	Cindex	DB Silhouette	Duda PseudoT2 ## Number_clusters		15.000	14.0000 14.0000 15.0000	13.0000 2.0000	2.0000

## Value_Index	2483.251 -14.5522	0.1044	0.3588	0.6657 0.2958	11.9059
##	Beale Ratkowsky	Ball  PtBiserial  Frey  McClain	Dunn Hubert
##	Number_clusters	4.000	4.0000 3.0000	4.0000	1	2.0000 4.0000	0
##	Value_Index	1.294	0.4385 0.9449	0.8282	NA	0.4206 0.6133	0
##		SDindex	Dindex	SDbw
##	Number_clusters	4.0000	0 15.0000
##	Value_Index	8.0627	0	0.0305

```
```
res **$** Best.partition
```
```
#### ## [1] 3 1 1 1 1 2 2 4 1 3 1 1 3 1 3 1 1 1 1 1 4 2
```

Given that the NbClust recommended 4 cluster, we used k = 4 with function ‘kmeans’.
```
# k-means clustering on the data
km <- kmeans (ed[ 2 : 5 ], 4 )
```
```
# Call back clustering information
km
```
```
## K-means clustering with 4 clusters of sizes
13, 4, 3, 2 ##
## Cluster means:
## Population GDP Unemployment MinWage
## 1 0.10346896 0.1150318 0.2097206 0.
## 2 0.09793412 0.5321101 0.1567164 0.
## 3 0.87108160 0.3119266 0.1160862 0.
## 4 0.34920975 0.1720183 0.9303483 0.
##
## Clustering vector:
## [1] 2 1 1 1 1 3 3 4 1 2 1 1 2 1 2 1 1 1 1 1 4 3
##
## Within cluster sum of squares by cluster:
## [1] 0.64489884 0.38291525 0.08966237 0.
## (between_SS / total_SS = 80.3 %)
##
## Available components:
##
## [1] "cluster" "centers" "totss" "withinss" "tot.withinss"
## [6] "betweenss" "size" "iter" "ifault"
```



## Visualize & understand clusters

We can also see how data is clustered and sort based on cluster.
```
_# See how data has been clustered_
eddf <- **data.frame** (ed, km **$** cluster)
```
```
_# Sort data based on cluster number_
eddf[ **order** (eddf **$** km.cluster),]
```
![image](https://github.com/user-attachments/assets/6b278a3b-f750-4663-ab6d-21586cb0c4f7)


We can then try to visualize it on two axis with color as cluster, there does appear to be
good clustering based on k-mean as can be seen across different combination.

```
**library** (ggplot2)
```
```
_# Plot Population against GDP_
**plot** (ed[ 2 **:** 5 ], col=km **$** cluster)
```
![image](https://github.com/user-attachments/assets/bd4ea3be-64d3-431b-bbba-8696dcd4ec74)

# K-Mean Clustering: Grouping Beer Companies together (with Categorical Variables)
European countries were grouped together using numerical data. Clustering technique changes when there is categorical variables in the data. This is showed in the following example.

## Understanding the data 
The dataset used for analysis is 'Brewdog.csv'.
```
data <- read.csv("brewdog.csv", header=TRUE, stringsAsFactors = T)	
```
You will see that ABV, Bottle.Size and Price are numerical variables while ‘Style’ is a categorical variable, listing the type of beer:

![image](https://github.com/user-attachments/assets/d7993ea3-c73a-4908-a2b5-0fc559b854c8)

Categorical data cannot be measured using normal distance measures like Euclidean distance. The ‘dist’ function you used earlier to calculate the difference matrix will throw a warning if provided with categorical data and then calculate while ignoring text. You can check this using:
```
dismatrix <- dist(data[2:5])	
## Warning in dist(data[2:5]): NAs introduced by coercion
```
Fortunately, there is another function in R for calculating a dissimilarity matrix for hierarchical clustering when you have mixed data types. This function is called daisy and is built into the cluster package. Create a dissimilarity matrix with mixed variables using:

The function will automatically use Euclidean distance for numerical data and Gower’s distance for categor- ical data. To see an explanation of the Gower’s distance measure type:

## Determine the number of clusters

However, nbclust for k-mean clustering doesn’t work with dissimilarity matrix and Gower’s distance. It also doesn’t handle category with little deviation quite well (bottle size for instance here). As such, it will be determining number of clusters based on ABV and Price.
```
nb <- NbClust(data = data[c(3,5)],min.nc = 2,
              max.nc = 10, method = "kmeans")
```
![image](https://github.com/user-attachments/assets/bed29aad-2f99-41f8-8747-c21c306badef)

```
## *** : The Hubert index is a graphical method of determining the number of clusters.
##	In the plot of Hubert index, we seek a significant knee that corresponds to a
##	significant increase of the value of the measure i.e the significant peak in Hubert ##	index second differences plot.
##
##	KL	CH Hartigan	CCC	Scott Marriot TrCovW TraceW
## Number_clusters	2.0000	6.0000	4.0000 6.0000	4.0000	4.0000 3.0000 4.0000
```
![image](https://github.com/user-attachments/assets/e01260e7-9b5e-47b0-8370-f9f955c6b2f6)

```
## *** : The D index is a graphical method of determining the number of clusters.
##	In the plot of D index, we seek a significant knee (the significant peak in Dindex ##	second differences plot) that corresponds to a significant increase of the value of ##	the measure.
##
## ******************************************************************* ## * Among all indices:
## * 8 proposed 2 as the best number of clusters ## * 3 proposed 3 as the best number of clusters ## * 6 proposed 4 as the best number of clusters ## * 2 proposed 6 as the best number of clusters ## * 2 proposed 9 as the best number of clusters ## * 3 proposed 10 as the best number of clusters ##
##	***** Conclusion ***** ##
## * According to the majority rule, the best number of clusters is	2 ##
##
## *******************************************************************
```
```
nb$Best.nc
```
```
## Value_Index	27.1229 164.9889	25.1112 8.1863 34.3225	0.2374 0.2048 0.3509
##	Friedman		Rubin	Cindex	DB Silhouette	Duda PseudoT2 ## Number_clusters		9.0000	9.0000 10.0000 10.0000	2.0000 2.0000	3.0000
## Value_Index	173.6749 -33.5061	0.0814	0.4329	0.7114 0.3195	36.9266
##	Beale Ratkowsky	Ball  PtBiserial	Frey McClain	Dunn Hubert
##	Number_clusters	2.0000	2.0000 3.0000	2.0000	4.0000	2.0000 2.0000	0
##	Value_Index	1.9172	0.6268 0.2868	0.8873	1.8995	0.2585 0.7474	0
##		SDindex Dindex	SDbw			
##	Number_clusters	4.0000	0 10.000
##	Value_Index	8.8274	0	0.013
```
```
nb$Best.partition
```
```
##	[1] 1 1 2 2 2 2 2 1 1 1 1 2 2 1 1 2 1 1 1 1 1 1 2 2 1 2 1 1 1
```
## Determining the clusters

Now we can use the dissimilarity matrix (dm) calculated by daisy to perform k-mean clustering. To do this we can use the agnes() function with:
```
clust <- kmeans(x=dm,2)	
```
As the input (dm) is a dissimilarity matrix, the diss attribute is set to TRUE. Now you can plot the dendrogram using plot(clust, labels=data$Name, which.plots= 2) which gives:
```
plot(data[2:5], col=clust$cluster)
```
![image](https://github.com/user-attachments/assets/38aa30db-a552-467d-8718-51c6bfdd96e9)

 
## Visualize and study different Clusters
We can get the cluster that the beers is assigned to from clust object by using clust$cluster. We can then study the output to see how the beers have been clustered.

```
data$cluster <- clust$cluster
data[order(data[['cluster']]),]
```
![image](https://github.com/user-attachments/assets/ed5f1bf7-cccc-4e65-b00f-9c4b03d1f13f)






