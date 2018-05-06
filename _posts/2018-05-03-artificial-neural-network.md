---
title: "Artificial Neural Network"
layout: post
title: "ML - Artificial Neural Network"
tags: [R, nnet package, artificial neural network, iris]
output: github_document
---
Today's goal is to understand the concept of Artificial Neural Network and to practice this with iris data in R. 

```{r setup, include=FALSE}
knitr::opts_chunk$set(fig.path = "README_figs/README-")
```

### 01. Load libraries
```{r}
library(caret)
library(nnet)
```


### 02. Partition of the iris data into trainnig and test data
```{r}
set.seed(0)
idx <- createDataPartition(iris$Species, p=0.7, list=F)
iris_train <- iris[idx, ]
iris_test <- iris[-idx, ]
```

```{r}
table(iris_train$Species)
table(iris_test$Species)
```


### 03. Standardization 
```{r}
iris_tr_scale <- as.data.frame(sapply(iris_train[-5], scale))
iris_test_scale <- as.data.frame(sapply(iris_test[-5], scale))
```

```{r}
iris_tr_scale$Species <- iris_train$Species
iris_test_scale$Species <- iris_test$Species
```


### 04. Data modeling 1
```{r}
iris_model <- nnet(Species~., data=iris_tr_scale, size=3)
```

```{r}
summary(iris_model)
```


### 05. nnet model visualization
```{r}
library(devtools)

# import the function from Github
source_url('https://gist.githubusercontent.com/Peque/41a9e20d6687f2f3108d/raw/85e14f3a292e126f1454864427e3a189c2fe33f3/nnet_plot_update.r')

plot.nnet(iris_model)
```


### 06. Prediction with the model
```{r}
iris_pred <- predict(iris_model, iris_test_scale, type="class") 
# type="class" -> shoing the predicted category by this model

iris_pred
```


### 07. Verification of the predicted values
```{r}
table(iris_pred, iris_test$Species)
```

```{r}
confusionMatrix(factor(iris_pred), iris_test$Species)
```

### Data modeling 2 - having more nodes in the hidden layer 
```{r}
iris_model2 <- nnet(Species~., data=iris_tr_scale, size=8)
summary(iris_model2)
```


### Prediction 
```{r}
iris_pred2 <- predict(iris_model2, iris_test_scale, type="raw") 
# type="raw" -> showing the percentage of each categoty 
round(iris_pred2, 6)
```


