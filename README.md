# Hw2
This repository stores Homework 2 for STOR 390. This consists of an exploration of the KNN classification algorithim on the iris dataset in R. 
# Code
This homework is meant to illustrate the methods of classification algorithms as well as their potential pitfalls.  In class, we demonstrated K-Nearest-Neighbors using the `iris` dataset.  Today I will give you a different subset of this same data, and you will train a KNN classifier.  

```{r, echo = FALSE}
set.seed(123)
library(class)

df <- data(iris) 

normal <-function(x) {
  (x -min(x))/(max(x)-min(x))   
}

iris_norm <- as.data.frame(lapply(iris[,c(1,2,3,4)], normal))

subset <- c(1:45, 58, 60:70, 82, 94, 110:150)
iris_train <- iris_norm[subset,] 
iris_test <- iris_norm[-subset,] 

iris_target_category <- iris[subset,5]
iris_test_category <- iris[-subset,5]


```

#
Above, I have given you a training-testing partition.  Train the KNN with $K = 5$ on the training data and use this to classify the 50 test observations.  Once you have classified the test observations, create a contingency table -- like we did in class -- to evaluate which observations your algorithm is misclassifying.   

```{r}
set.seed(123)
pr <- knn(iris_train, iris_test, cl = iris_target_category, k = 5)
table <- table (pr, iris_test_category)
table
```

#

Discuss your results.  If you have done this correctly, you should have a classification error rate that is roughly 20% higher than what we observed in class.  Why is this the case? In particular run a summary of the `iris_test_category` as well as `iris_target_category` and discuss how this plays a role in your answer.  

From our contingency table, we see that our knn misclassified 11 out of 50 testing points. These were 11 versicolors misclassified as virginicas. Let's examine iris_test_category and iris_target_category in the summary below. In our test category, we have 5 setosas, 36 versicolors, and 9 virginicas. On the other hand, our target category has 45 setosas, 14 versicolors, and 41 virginicas. Noticing the drastic difference between the number of virginicas (41) in the target category vs. the testing category (9) Our overprediction of virginicas (from versicolors) is explained by the difference in species representation between the testing and target categories.

```{r}
test <- summary(iris_test_category)
test
target<- summary(iris_target_category)
target
```



Choice of $K$ can also influence this classifier.  Why would choosing $K = 6$ not be advisable for this data? 

As discussed in class, choosing a k that is divisible by the number of explanatory variables is not advisible. This can lead to classification issues in which, if there are "classification ties" in R, there will be a random decision about a point's classification. We do not want our classifications to be a coin toss -- that completely defeats the purpose of doing classification! :)


