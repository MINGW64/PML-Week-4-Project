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
library(ranger)
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
####Step 4. Tune RF using fast RF package Ranger
```
#tune RF model

rf_result5 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=5)
rf_result5
rf_result7 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=7)
rf_result7
rf_result9 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=9)
rf_result9
rf_result11 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=11)
rf_result11
rf_result13 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=13)
rf_result13
rf_result15 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=15)
rf_result15
rf_result17 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=17)
rf_result17
rf_result19 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=19)
rf_result19
rf_result21 = ranger(classe~., data=dat_train3, num.trees=500, replace = FALSE , mtry=21)
rf_result21
```
####Step 5. Select the best one by the lowest OOB error
```
print(rf_result15)
rf_result15$confusion.matrix
```

```
Ranger result

Call:
 ranger(classe ~ ., data = dat_train3, num.trees = 500, replace = FALSE,      mtry = 15) 

Type:                             Classification 
Number of trees:                  500 
Sample size:                      19622 
Number of independent variables:  54 
Mtry:                             15 
Target node size:                 1 
Variable importance mode:         none 
OOB prediction error:             0.10 % 

    predicted
true    A    B    C    D    E
   A 5579    0    0    0    1
   B    1 3795    1    0    0
   C    0    5 3417    0    0
   D    0    0    9 3206    1
   E    0    0    0    2 3605
```
####Step 6. Prediction
```
#prediction
pred_test=predict(rf_result15, data = dat_test3)
pred_test$predictions
```
```
 [1] B A B A A E D B A A B C B A E E A B B B
Levels: A B C D E
```

###Notes

#### Why Random Forest?
RF has desired property of convenience in data processing and manipulation. It is robust to extreme values and outliers, naturally handle missing values, capable for categorical variables without dummy coding, etc...

#### How to do cross validation?
CV is not necessary in RF model since OOB error can be used. More specifically, the parameter REPLACE=FALSE is specified to sample data without replacement. In such case OOB errors are more close to generalized error.
