---
title: "CodeBook"
author: "Santosh Waikar"
date: "December 16, 2015"
output: html_document
---
##Purpose:-
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 
 
   http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
 
 Here are the data for the project: 
   
   https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
 
 You should create one R script called run_analysis.R that does the following.
 
 1. Merges the training and the test sets to create one data set.
 2. Extracts only the measurements on the mean and standard deviation for each measurement.
 3. Uses descriptive activity names to name the activities in the data set
 4. Appropriately labels the data set with descriptive variable names.
 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

##file URL to pull data
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
## get current working directory
filePath <- getwd()
##define file name with path
file <- file.path(filePath,"UciHarDataset.zip")
##down load file from given URL into your local direcotyr
download.file(fileUrl, file)

##unzip the file
unzip(zipfile="./UciHarDataset.zip",exdir="./dataSet")

##unzipped the file 
path_refarance <- file.path("./dataSet" , "UCI HAR Dataset")
## list all the files
allFiles <- list.files(path_refarance, recursive = TRUE)

####Read data from the files into the variables
##read activiry file for Test into table format
activityDataTest  <- read.table(file.path(path_refarance, "test" , "Y_test.txt" ),header = FALSE)

##read activiry file for Train into table format

```r
activityDataTrain  <- read.table(file.path(path_refarance, "train" , "Y_train.txt" ),header = FALSE)
```
##read subject file for Test into table format

```r
subjectDataTest  <- read.table(file.path(path_refarance, "test" , "subject_test.txt" ),header = FALSE)
```
##read subject file for Train into table format

```r
subjectDataTrain  <- read.table(file.path(path_refarance, "train" , "subject_train.txt" ),header = FALSE)
```
##read features file for Test into table format

```r
featuresDataTest  <- read.table(file.path(path_refarance, "test" , "X_test.txt" ),header = FALSE)
```
##read features file for Train into table format

```r
featuresDataTrain  <- read.table(file.path(path_refarance, "train" , "X_train.txt" ),header = FALSE)
```
##read features.txt file into table format

```r
featuresDataNames <- read.table(file.path(path_rf, "features.txt"),head=FALSE)
```

```
## Error in file.path(path_rf, "features.txt"): object 'path_rf' not found
```
##read descriptive activiry names fromactivity_lables.txt file

```r
activityLablesData <- read.table(file.path(path_rf, "activity_labels.txt"), head=FALSE)
```

```
## Error in file.path(path_rf, "activity_labels.txt"): object 'path_rf' not found
```
##activityLablesData


##Merges the Train and the Test sets to create one data set.

```r
meargeDataForActivity <- rbind(activityDataTest,activityDataTrain)
meargeDataForSubject <- rbind(subjectDataTest,subjectDataTrain)
meargeDataForFeatures <- rbind(featuresDataTest,featuresDataTrain)
```
##Appropriately labels the data set with descriptive variable names. 

```r
names(meargeDataForSubject)<-c("Subject")
names(meargeDataForActivity)<- c("Activity")
names(meargeDataForFeatures)<- featuresDataNames$V2
```
##coulumn bind for Subject and Activity

```r
combineDataForSubjActi <- cbind(meargeDataForSubject, meargeDataForActivity)
```
##coulumn bind for Features and combines

```r
combineDataForFeatCom <- cbind(meargeDataForFeatures, combineDataForSubjActi)
```

##take the measurements on the mean and standard devi for each

```r
subFeaturesNamesData<-featuresDataNames$V2[grep("mean\\(\\) | std\\(\\)", featuresDataNames$V2)]
```
##extract the subject and activiry

```r
selectedNames<-c(as.character(subFeaturesNamesData), "Subject", "Activity" )
```
##subset Data with selected Names

```r
FinalData <- subset(combineDataForFeatCom,select=selectedNames)
```
##FinalData

####Appropriately labels the data set with descriptive activity names.
##apply lables to the combineDataForFeatCom dataset
##combineDataForFeatCom; names(combineDataForFeatCom)

```r
names(combineDataForFeatCom)<-gsub("^t", "Time", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("tBody", "TimeBody", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("Acc", "Accelerometer", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("-mean()", "Mean", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("-std()", "STD", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("Gyro", "Gyroscope", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("BodyBody", "Body", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("Mag", "Magnitude", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("^f", "Frequency", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("-freq()", "Frequency", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("angle", "Angle", names(combineDataForFeatCom))
names(combineDataForFeatCom)<-gsub("gravity", "Gravity", names(combineDataForFeatCom))
```
##creates a second, independent tidy data set with the average of each variable for 
##each activity and each subject.

##make sure you have plyr package. if not install the pakage
##load the plyr package

```r
library(plyr);
```

```
## -------------------------------------------------------------------------
## You have loaded plyr after dplyr - this is likely to cause problems.
## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
## library(plyr); library(dplyr)
## -------------------------------------------------------------------------
## 
## Attaching package: 'plyr'
## 
## The following objects are masked from 'package:dplyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
## 
## The following objects are masked from 'package:Hmisc':
## 
##     is.discrete, summarize
```

```r
dataSetNew <-aggregate(. ~Subject + Activity, combineDataForFeatCom, mean)
```
##dataSetNew

##pull only subject and active rows

```r
dataSetNew <- dataSetNew[order(dataSetNew$Subject,dataSetNew$Activity),]
```
## write new dataset in table format and store in tidyDataSet.txt file

```r
write.table(dataSetNew, file = "tidyDataSet.txt", row.name=FALSE)
```

##END
