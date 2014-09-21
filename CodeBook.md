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
 * 'train/subject_test.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

### Source data structure
David Hood, one of the community TAs, produce the following diagram that clearly depicts how the various files are related. I added some numbering to relate to the transformation steps described next.

![File Structure](/images/Slide2.png)

### Transformations
__Merging the training and test data sets__

The first step taken is to merge the training (train/X_train.txt) and test (test/X_test.txt) datasets using `rbind`.

__Dropping columns that are not needed__

As the project requirements state, we are to only keep the columns that provide measurements for mean and standard deviation.  WE interpreted this as keeping columns that matches "mean" and "std" (case sensitively) which is enough to match all column names that contain "mean()" and "std()".  `select` with the relevant `matches` was used here.

