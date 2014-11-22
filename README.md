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

So here is short description in 6 steps of my script that is in run_analysis.R:

A. Downloading data for the project. All data are in zip file so unzip in R is used.

1. In first step I'm reading all necessary data using "read.table". There are train and test data sets which are merged using "rbind" - train and test data has the same length of 536 variables.

2. In second step I'm creating a data frame that contains only columns with mean and standard deviation for each measurment using "grep". Note: in this step second requirement is fulfilled.

3. In third step to the existing data frame the column with activity names is added using "merge".

4. In fourth step to appropriately label data frame with descriptive variable names gsub function is used to clean column names to make them easier to read.

5. Finally, thanks to dplyr I'm creating a independent and tidy data set with the average of each variable for each activity and each subject. And that's it.

For more details please check run_analysis which is quite nicely described R code.
In codebook you can find further discription on features(column names) that are measured.

Regards.
