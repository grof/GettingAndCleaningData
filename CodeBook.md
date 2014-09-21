As prescribed by Kirsten Frank in [this dicussion forum thread](https://class.coursera.org/getdata-007/forum/thread?thread_id=28), this CodeBook describes the variables and transformations they went through to produce the resulting data set.

### Source data files
The zip file that contains the source data for this project contains a number of different files.  The files that are of interest to us are:

 * 'features.txt': List of all features.
 * 'activity_labels.txt': Links the class labels with their activity name.
 * 'train/X_train.txt': Training set.
 * 'train/y_train.txt': Training labels.
 * 'test/X_test.txt': Test set.
 * 'test/y_test.txt': Test labels. 
 * 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
 * 'test/subject_test.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

### Source data structure
David Hood, one of the community TAs, produce the following diagram that clearly depicts how the various files are related.

![File Structure](/images/Slide2.png)

### Transformations
__Reading in features data__

The original data set has the name of each measurement stored in file features.txt.  This file is read in both columns are named "FeatureID" and "FeatureLabel" respectively. 

_Reading in training and test data and naming columns__

We extract a vector called "colNames" to us as the value of `col.names` when reading the training (train/X_train.txt) and test (test/X_test.txt) data sets.  We prefix each column name with "mean-" as our end goal is to provide the mean value of each measurement. We end up with two data frames that have the same column names.

__Merging the training and test data sets to create the main table__

We merge the training datasets using `rbind`.

__Dropping columns that are not needed__

As the project requirements state, we are to only keep the columns that provide measurements for mean and standard deviation.  WE interpreted this as keeping columns that matches "mean" and "std" (case sensitively) which is enough to match all column names that contain "mean()" and "std()".  `select` with the relevant `matches` was used here.

__Reading Subject data__

The source data uses data from separate subject files (train/subject_train.txt and test/subject_test.txt) to identify which subject was involved for each row of measurements found in the training and test data sets.  Combined, the two subject files count as many rows as the two data files.  We read both files into data frames and name the only column they contain "Subject". We use `rbind` to merge both sets.

__Adding Subject data to the main table__

As we have 1-to-1 correspondance between the merged data tables and the merged subject tables, we simply need to add a "Subject" column to our main table.  We use `mutate` to add a new "Subject" column to the main table.

__Reading Activity definitions__

Activity definitions are contained in file activity_labels.txt.  We read this file into a table and name its columns "ActivityID" and "Activity" respectively.  We later use this table in a join with the main table.

__Reading Activity data__

Activity data is stored in two separate files (train/y_train.txt and test/y_test.txt).  Similarly to how subject data is organzied, each row in in both activity data files corrspond 1-to-1 to a row in the associated training or test data files.  We thus first read both activity data files into data frames and name their only column "ActivityID".  Next, we merge both sets using `rbind`.

__Adding Activity data to the main table__

We add the merged activity data into the main table using `inner_join` on the "ActivityID" column.  Our main table is now complete.

__Grouping the main table by Subject and Activity__

As requested by the project requirements, we group the main table using `group_by` on "Subject" and then "Activity"

__Generating the summary table__

Finally, we use `summarise_each` on the grouped-by table to compute the mean of each column using the grouping prescribed in the previous step.


## Data Dictionary

As described in the file "features_info.txt" provided with the original data set:

_The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove no_

_Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag)._

_Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals)._

So the data contained in the summary table called "subject-activity-means.txt" is the following.  The table is keyed on "Subject" (which identifies the subject) and "Activity" (which idenfities one of the activity).  All the other data elements that are named "mean-<measurement>" identify the mean of one of the original <measurement>s listed in the original data set.

 * mean-tBodyAcc-mean()-X
 * mean-tBodyAcc-mean()-Y
 * mean-tBodyAcc-mean()-Z
 * mean-tBodyAcc-std()-X
 * mean-tBodyAcc-std()-Y
 * mean-tBodyAcc-std()-Z
 * mean-tGravityAcc-mean()-X
 * mean-tGravityAcc-mean()-Y
 * mean-tGravityAcc-mean()-Z
 * mean-tGravityAcc-std()-X
 * mean-tGravityAcc-std()-Y
 * mean-tGravityAcc-std()-Z
 * mean-tBodyAccJerk-mean()-X
 * mean-tBodyAccJerk-mean()-Y
 * mean-tBodyAccJerk-mean()-Z
 * mean-tBodyAccJerk-std()-X
 * mean-tBodyAccJerk-std()-Y
 * mean-tBodyAccJerk-std()-Z
 * mean-tBodyGyro-mean()-X
 * mean-tBodyGyro-mean()-Y
 * mean-tBodyGyro-mean()-Z
 * mean-tBodyGyro-std()-X
 * mean-tBodyGyro-std()-Y
 * mean-tBodyGyro-std()-Z
 * mean-tBodyGyroJerk-mean()-X
 * mean-tBodyGyroJerk-mean()-Y
 * mean-tBodyGyroJerk-mean()-Z
 * mean-tBodyGyroJerk-std()-X
 * mean-tBodyGyroJerk-std()-Y
 * mean-tBodyGyroJerk-std()-Z
 * mean-tBodyAccMag-mean()
 * mean-tBodyAccMag-std()
 * mean-tGravityAccMag-mean()
 * mean-tGravityAccMag-std()
 * mean-tBodyAccJerkMag-mean()
 * mean-tBodyAccJerkMag-std()
 * mean-tBodyGyroMag-mean()
 * mean-tBodyGyroMag-std()
 * mean-tBodyGyroJerkMag-mean()
 * mean-tBodyGyroJerkMag-std()
 * mean-fBodyAcc-mean()-X
 * mean-fBodyAcc-mean()-Y
 * mean-fBodyAcc-mean()-Z
 * mean-fBodyAcc-std()-X
 * mean-fBodyAcc-std()-Y
 * mean-fBodyAcc-std()-Z
 * mean-fBodyAcc-meanFreq()-X
 * mean-fBodyAcc-meanFreq()-Y
 * mean-fBodyAcc-meanFreq()-Z
 * mean-fBodyAccJerk-mean()-X
 * mean-fBodyAccJerk-mean()-Y
 * mean-fBodyAccJerk-mean()-Z
 * mean-fBodyAccJerk-std()-X
 * mean-fBodyAccJerk-std()-Y
 * mean-fBodyAccJerk-std()-Z
 * mean-fBodyAccJerk-meanFreq()-X
 * mean-fBodyAccJerk-meanFreq()-Y
 * mean-fBodyAccJerk-meanFreq()-Z
 * mean-fBodyGyro-mean()-X
 * mean-fBodyGyro-mean()-Y
 * mean-fBodyGyro-mean()-Z
 * mean-fBodyGyro-std()-X
 * mean-fBodyGyro-std()-Y
 * mean-fBodyGyro-std()-Z
 * mean-fBodyGyro-meanFreq()-X
 * mean-fBodyGyro-meanFreq()-Y
 * mean-fBodyGyro-meanFreq()-Z
 * mean-fBodyAccMag-mean()
 * mean-fBodyAccMag-std()
 * mean-fBodyAccMag-meanFreq()
 * mean-fBodyBodyAccJerkMag-mean()
 * mean-fBodyBodyAccJerkMag-std()
 * mean-fBodyBodyAccJerkMag-meanFreq()
 * mean-fBodyBodyGyroMag-mean()
 * mean-fBodyBodyGyroMag-std()
 * mean-fBodyBodyGyroMag-meanFreq()
 * mean-fBodyBodyGyroJerkMag-mean()
 * mean-fBodyBodyGyroJerkMag-std()
 * mean-fBodyBodyGyroJerkMag-meanFreq()
