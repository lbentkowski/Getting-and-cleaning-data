Getting-and-cleaning-data
=========================
Hello!
This is my getting nad cleaning data repository created to be evaluated under "Peer Assigment".

Below, please find short discription of assigment requirements:

 You should create one R script called run_analysis.R that does the following:

    1.Merges the training and the test sets to create one data set.
    2.Extracts only the measurements on the mean and standard deviation for each measurement. 
    3.Uses descriptive activity names to name the activities in the data set
    4.Appropriately labels the data set with descriptive variable names. 
    5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

So here is short disription in 6 steps of my script that is in run_analysis.R:

1. Downloading data for the project. All data are in zip file so unzip in R is used.
2. Second step is to read all necesery data from txt files that were in zip file.I'm doing this by


# Getting and cleaning data project
# 1. Downloading data for the project ----
library(reshape2)
library(dplyr)
temp <- tempfile()
# Downloading 60MB dataset in zip file - please be patient ;)
download.file("http://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip",temp)
data <-(unzip(temp))
unlink(temp)
# 2. Merges the 'train' and the 'test' sets to create one data set ----
  # 2.A Read features - features are colnames and are identical for train and test sets.
  features <- read.table("./UCI HAR Dataset/features.txt", header = FALSE)
  features <- features[,2] 
  # 2.B Loading train sets
  x_train <- read.table("./UCI HAR Dataset/train/x_train.txt")
  colnames(x_train) <- features
  # loading train labels 
  y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")
  train <- cbind(y_train,x_train)
  # loading train subjects
  subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")
  train <- cbind(subject_train,train)
  # 2.C Loading test sets
  x_test <- read.table("./UCI HAR Dataset/test/x_test.txt")
  colnames(x_test) <- features
  # loading test labels 
  y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
  test <- cbind(y_test,x_test)
  # loading test subjects
  subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")
  test <- cbind(subject_test,test)

  # 2.D Creating one data set by combining train and test set.
  df <- rbind(test, train)

# 3. Extracts only the measurements on the mean and standard deviation for each measurement. ----
# Note: Measurements with mean and standard deviation got label names with 'mean' and 'std'.
  # 3.A Creating data frame subset that contains only columns with 'mean' and 'std' strings thanks to grep function:
  df_subset <- df[,grep("mean|std", colnames(df))]
  # 3.B Adding columns with subject and activity number and giving it a name:
  df <- cbind(df[,1:2], df_subset)
  colnames(df)[1:2] <- c("Subject","ActivityNumber")

# 4. Uses descriptive activity names to name the activities in the data set ----
  # 4.A Loading descriptive activity names file:
  activities <- read.table("./UCI HAR Dataset/activity_labels.txt", header = FALSE)
  colnames(activities) <- c("ActivityNumber","ActivityName")
  # 4.B Merging loaded desctiptive activity names with datafreme and creating new merged dataframe:
  df <- merge(df, activities, by.y= "ActivityNumber", sort=FALSE)
  df <- df[,-1] # we don't need firts column

#5 Appropriately labels the data set with descriptive variable names ----
# I'm doing this simply by adding disctiptive activity names to dataframe using gsub:
  colnames(df) <- gsub("^t","Time.", colnames(df))
  colnames(df) <- gsub("Body","Body.", colnames(df))
  colnames(df) <- gsub("Acc","Acc.", colnames(df))
  colnames(df) <- gsub("^f","Freq.", colnames(df))
  colnames(df) <- gsub("Gyro","Gyro.", colnames(df))
  colnames(df) <- gsub("Jerk","Jerk.", colnames(df))
  colnames(df) <- gsub("Mag","Magn.", colnames(df))
  colnames(df) <- gsub("-","", colnames(df))
  colnames(df) <- gsub("\\(\\)",".", colnames(df))
  colnames(df) <- gsub("mean","Mean", colnames(df))
  colnames(df) <- gsub("std","Std", colnames(df))
  colnames(df) <- gsub("-X","X", colnames(df))
  colnames(df) <- gsub("-Y","Y", colnames(df))
  colnames(df) <- gsub("-Z","Z", colnames(df))
# To keep it clean:
rm(df_subset, features, subject_test, subject_train, temp, activities, x_train, y_train, x_test, y_test)
# 6. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
tidy_df <- df
tidy_df <- tidy_df %>% group_by(ActivityName, Subject) %>% summarise_each(funs(mean))

# 6.A I don't like this 7 digits numbers in tidy_df so i round all numbers to 3 digits only:
tidy_df[,c(3:81)] <- round(tidy_df[,c(3:81)], digits = 3) 
# 6.B Writiing clean data to txt file:
write.table(tidy_df, file ="tidy.txt", row.names=FALSE)
tidy<-read.table("tidy.txt", header=TRUE)
