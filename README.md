# PML-Week-4-Project
##Multinominal Classification of Weight Lifting Patterns

###Background
The task of the project is to construct a predictive model of a multi-level categorical target variable. In statistics and machine learning study it is usually referred to multinominal classification problem. In this case the target variable is the weight lifting modes containing 5 different classes, A, B, C, D and E. The predictors include the data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants.

The report is organized as the steps taken from the begining to the end of producing prediction.

####Step 1. Downloading data
```
dat_train = read.csv("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv")
dat_test = read.csv("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv")
```

####Step 2. Initialization
```
#loading necessary packages and set random seed
library(caret)
library(rpart)
library(rattle)
library(rpart.plot)
set.seed(123)
```

####Step 3. Data Sanity work
```
#remove constant or near constant variables
dat_train1 = dat_train[, -nearZeroVar(dat_train)]
dat_test1 = dat_test[, -nearZeroVar(dat_train)]

#remove high missing percentage variables
MissingInd = sapply(dat_train1, function(x) mean(is.na(x))) > 0.95
dat_train2 = dat_train1[, MissingInd==FALSE]
dat_test2 = dat_test1[, MissingInd==FALSE]

#delete ID related variables
dat_train3 = dat_train2[, -c(1, 3:5)]
dat_test3 = dat_test2[, -c(1, 3:5)]
```
