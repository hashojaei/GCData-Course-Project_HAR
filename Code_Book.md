#### Original dataset ####

 The following files from UCI HAR dataset.zip were used for week 4 final assignment:

- `activity_labels.txt` 
- `features.txt`
- `subject_test.txt` 
- `X_test.txt`
- `y_test.txt`
- `subject_train.txt` 
- `X_train.txt`
- `y_train.txt`

#### Structure ####

- The `features.txt` contains 561 variable names which will be used for the column names of the `x_` data files.
- The `x_` files contains 561 columns/variables with measurements from the different sensors.
- The `y_` files contain a single column contains the 1:6 activity code. 
- The `subject_' files contain a single column with a identifier code for the subject (i.e. user) being observed
- The `_train` data, per the UCI source data page, contains data observed for the 70% of the volunteers selected for generating the training data. The `_test` data contains observations for the remaining 30%. 


#### Tidy data creations ####

1. Origial data were downloaed and unzipped directky into R.
2. At first, two files: `activity_labels.txt` and `features.txt` descriptive tables were read into R.
3. Using fread() all_test related files were loaded and read into R including: `subject_test.txt`, `X_test.txt`, `y_test.txt`.
4. Using fread() all_train related files were loaded and read into R including:`subject_train.txt`, `X_train.txt`, and `y_train.txt`. 
5. Using `cbind()`, all of the test related files (`subject_test.txt`, `X_test.txt`, `y_test.txt`)were attached together. 
6. Using `cbind()`, all of the train related files (`subject_train.txt`, `X_train.txt`, and `y_train.txt`)were attached together.
7. Using `rbind()`, the training and the test sets were merged to create one data set.
8. Using `grep()`,  only the measurements on the mean and standard deviation for each measurement were extracted.
9. Since `grep()` did not select the first two columns (` subject` & `act_num`), using `cbind()` these two variables were added to the out put of the previous file.
10. Using the `merge()` function, the two files of `sub_har_SPhones` and `activity_labels` were merge together.
11. Then using `select()` the variables were rearranged and then through `arrange()` function the variables `subject` and `act_num` were arranged. 
12. Tidy data were created through `gather()` function. 
13. Using `group_by()` and `summarise` functions,  a second, independent tidy data set with the average of each variable for each activity and each subject was created.
14. Finally, using `write.table()` the two datasets were written to a text file. 


#### The Output Files ####
There are two outputs for this analysis:

 1) tidy_HAR.txt
 
- `subject` - a numeric identifier for the user or subject being observed
- `actnum` - activity code identifier that matches up to an activity description such as 1 = Walking, 4 = Sitting, 5 = Standing etc.
- `actdesc` - description of activity being performed by the subject
- `features` - selected variables containing the mean() or std() of observed activity
- `measurement` - measured values


 2) Sum_Ave_HAR.txt

- `subject` - a numeric identifier for the user or subject being observed
- `actdesc` - description of activity being performed by the subject
- `features` - selected variables containing the mean() or std() of observed activity
- `avg_measurement` - average of measured values across time



