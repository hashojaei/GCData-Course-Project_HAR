# Week Four Assignment 

This is a final assignment for the the use of data wrangling functions in R, the Johns Hopkins' [Getting and Cleaning Data].
The data were used from  [UCI Human Activity Recognition dataset analysis] (http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones#) which maintains a collection of datasets available to the machine learning community for analysis and research.

The course requested the using a datasets entitled "Human Activity Recognition Using Smartphones" from UCI Human Activity Recognition dataset analysis center perform the following analysis:

 1) a tidy data set as described.
 - Merges the training and the test sets to create one data set.
 - Extracts only the measurements on the mean and standard deviation for each measurement.
 - Uses descriptive activity names to name the activities in the data set
 - Appropriately labels the data set with descriptive variable names.
 - From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
 2) a link to a Github repository with your script for performing the analysis
 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md


##### R Scripts #####

setwd("F:/R analysis/Prastice R/Practice")

# 1) Load  the required packages
library(downloader)
library(data.table)
library(dplyr)
library(tidyr)

# 2) Download the Original Data Into My R WD
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download(url, dest="dataset.zip", mode="wb")
unzip ("dataset.zip", exdir = "./Habib")


# 3) Read the Activity Labels and Features list

activity_labels <- fread("./Habib/UCI HAR Dataset/activity_labels.txt", col.names = c("act_num", "act_desc"))

features <- fread("./Habib/UCI HAR Dataset/features.txt", col.names = c("fea_num", "fea_desc"))


# 4) Read the subject_test, X_test, y_test

subject_test <- fread("./Habib/UCI HAR Dataset/test/subject_test.txt" , col.names = "subject")

X_test <- fread("./Habib/UCI HAR Dataset/test/X_test.txt", col.names = features$fea_desc)

y_test <- fread("./Habib/UCI HAR Dataset/test/y_test.txt", col.names = "act_num")


# 5) Read the subject_train, X_train and y_train

subject_train <- fread("./Habib/UCI HAR Dataset/train/subject_train.txt" , col.names = "subject")

X_train <- fread("./Habib/UCI HAR Dataset/train/X_train.txt", col.names = features$fea_desc)

y_train <- fread("./Habib/UCI HAR Dataset/train/y_train.txt", col.names = "act_num")



# 6) Combine the test and training data sets (x_test and x_train)
all_test <- cbind(subject_test,y_test,X_test)
all_train <- cbind(subject_train, y_test, X_test)

# 7) Merges the training and the test sets to create one data set.
har_SPhones<- rbind(all_train, all_test)

# 8) Extracts only the measurements on the mean and standard deviation for each measurement.

sub_har_SPhones<- har_SPhones[, grepl(".*-(mean|std)[^0-9a-zA-Z]-*", colnames(har_SPhones)), with = FALSE]

sub_har_SPhones<- cbind(har_SPhones[,1:2], sub_har_SPhones)


# 9) Uses descriptive activity names to name the activities in the data set
sub_har_SPhones <- merge(sub_har_SPhones, activity_labels, by = "act_num")

sub_har_SPhones <- sub_har_SPhones %>% select(2, 1, 69, 3:68) %>% arrange(subject, act_num)


str(sub_har_SPhones)
sub_har_SPhones$subject <- as.factor(sub_har_SPhones$subject)
sub_har_SPhones$act_num <- as.factor(sub_har_SPhones$act_num)


# 10) Create a 'tidy' dataset

tidy_Sub_har <- sub_har_SPhones %>% gather("features","measurements", 4:69)

# 11) From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

har_Summary_Ave <- tidy_Sub_har %>% group_by(subject, act_desc, features) %>%   summarise(avg_measurement = mean(`measurements
+                                            `))%>% ungroup() %>% ungroup() %>% 
                                           +     arrange(subject, act_desc, features) 


# 12) Finally Write Dataseta to files
write.table(tidy_Sub_har , file = "tidy_HAR.txt", quote = FALSE, sep = " ", row.names = FALSE, col.names = TRUE)

write.table(har_Summary_Ave , file = "Sum_Ave_HAR.txt", quote = FALSE
, sep = " ", row.names = FALSE, col.names = TRUE)



