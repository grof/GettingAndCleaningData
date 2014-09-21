README.MD

As the project description said, one of the most exciting areas in all of data science right now is wearable computing.  The `run_analysis.R` script submitted for this project reads data collected from the accelerometers from the Samsung Galaxy S smartphone and outputs a summary file that provides the mean of each measurement grouped by Subject and Activity.

Ddata for the project was obtained from: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

Information about the source project is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

## What the script does

As requestd in the project, the `run_analysis.R` reads in various files from the original data set and perform various operations in order to produce the desired summary table.  The script depends on the plyr and dplyr libaries. The steps are:

 * download the zip file from its source location
 * unzip the archive
 * load the various files in memory
 * merge the training and test sets into a main table
 * discard columns that carry measurements that are not means or standard deviations
 * add Subject information to the main table
 * add Activity information to the main table
 * grouping data by Subject and Activity, generate a summary table that provides the mean of each measurement.
 * write summary table to file "subject-activity-means.txt"

## Running the script
 * `cd` to the root of this project directory
 * 
From R or RStudio, simply type `source (


