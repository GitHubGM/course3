One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants.
Data Preprocessing
library(caret)
library(rpart)
library(rpart.plot)
library(randomForest)
library(corrplot)
Download the Data
trainFile <- "./data/pml-training.csv"
testFile  <- "./data/pml-testing.csv"
if (!file.exists("./data")) {
  dir.create("./data")
}
if (!file.exists(trainFile)) {
  download.file(trainUrl, destfile=trainFile, method="curl")
}
if (!file.exists(testFile)) {
  down
And read
trainRaw <- read.csv("./data/pml-training.csv")
testRaw <- read.csv("./data/pml-testing.csv")
dim(trainRaw)
Then we will slice it
set.seed(22519) 
inTrain <- createDataPartition(trainCleaned$classe, p=0.70, list=F)
trainData <- trainCleaned[inTrain, ]
testData <- trainCleaned[-inTrain, ]
Data Modeling

We constructed an activity recognition predictive model employing the Random Forest technique due to its inherent capability to identify significant features and its resilience to correlated predictors and outliers. A 5-fold cross-validation approach will be utilized during the implementation of this algorithm.
controlRf <- trainControl(method="cv", 5)
modelRf <- train(classe ~ ., data=trainData, method="rf", trControl=controlRf, ntree=250)
modelRf
## Random Forest
##
## 13737 samples
##    52 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E'
##
## No pre-processing
## Resampling: Cross-Validated (5 fold)
##
## Summary of sample sizes: 10989, 10989, 10991, 10990, 10989
##
## Resampling results across tuning parameters:
##
##   mtry  Accuracy   Kappa      Accuracy SD   Kappa SD    
##    2    0.9904636  0.9879361  0.0006534224  0.0008263772
##   27    0.9913374  0.9890405  0.0015731292  0.0019917940
##   52    0.9850766  0.9811193  0.0027732182  0.0035098533
##
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 27.
1.	Correlation Matrix Visualization
 

estimated accuracy of the model is 99.30% and the estimated out-of-sample error is 0.70%.
