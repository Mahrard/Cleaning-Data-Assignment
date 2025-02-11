Getting and Cleaning Data Week 4 - Assignment
================

The Source
----------

This data is taken from the [Human Activity Recognition Using Smartphones Data Set](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones#), a publicly acessible dataset available on the UCI Machine Learning Repository. It was collected by Jorge L. Reyes-Ortiz(1,2), Davide Anguita(1), Alessandro Ghio(1), Luca Oneto(1) and Xavier Parra(2).

For more information, see [**Code\_Book.md**](https://github.com/Mahrard/Cleaning-Data-Assignment/blob/master/Code_Book.md)

The Work
--------

We collected their training and testing sets into one data set and kept the **means** and **standard deviations** of the main variables:

-   tBodyAcc-XYZ
-   tGravityAcc-XYZ
-   tBodyAccJerk-XYZ
-   tBodyGyro-XYZ
-   tBodyGyroJerk-XYZ
-   tBodyAccMag
-   tGravityAccMag
-   tBodyAccJerkMag
-   tBodyGyroMag
-   tBodyGyroJerkMag
-   fBodyAcc-XYZ
-   fBodyAccJerk-XYZ
-   fBodyGyro-XYZ
-   fBodyAccMag
-   fBodyAccJerkMag
-   fBodyGyroMag
-   fBodyGyroJerkMag

The description of these variable can be found in the code book. Note that the authors had included some other variable's means in the data but they did not have a standard deviation associated to them so we decided to drop them.

We then computed the means of all these variables grouped by subjects and activities and stored them in a new table.

The Code
--------

First we make a vector of Subjects from both the training and the testing set. Then we turn these into factors for easy grouping.

``` r
Subj_table<-rbind(read.table("UCI HAR Dataset/train/subject_train.txt",col.names = 'subjects'),
                  read.table("UCI HAR Dataset/test/subject_test.txt",col.names = 'subjects'))
Subj_table$subjects<-as.factor(Subj_table$subjects)
```

    ## 'data.frame':    10299 obs. of  1 variable:
    ##  $ subjects: Factor w/ 30 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...

We make a data frame with the types of activities done in the experiment. We collect the activity data from the training and tesing set, label the column and set the names as factors appropriately.

``` r
Act<-read.table('UCI HAR Dataset/activity_labels.txt')
Act_table<-rbind(read.table("UCI HAR Dataset/train/y_train.txt",col.names = 'activity'),
                 read.table("UCI HAR Dataset/test/y_test.txt",col.names = 'activity'))
Act_table$activity<-factor(Act_table$activity,levels=Act[[1]],labels=Act[[2]])
```

    ## 'data.frame':    10299 obs. of  1 variable:
    ##  $ activity: Factor w/ 6 levels "WALKING","WALKING_UPSTAIRS",..: 5 5 5 5 5 5 5 5 5 5 ...

We collect the names of the variables as a character vector, so as to later pass them easily to the main data as column names.

``` r
names<-read.table("UCI HAR Dataset/features.txt",as.is=TRUE)[[2]]
```

    ##  chr [1:561] "tBodyAcc-mean()-X" "tBodyAcc-mean()-Y" ...

We finally collect the data from the experiment, keep the rows corresponding to means of the given variables. add the Subject and Activity factors and clean up.

``` r
Data_table<-rbind(read.table("UCI HAR Dataset/train/x_train.txt",col.names = names),
                  read.table("UCI HAR Dataset/test/x_test.txt",col.names = names))%>%
    select(matches('std\\(\\)|(mean\\(\\))',vars=names))
All_table<-cbind(Subj_table,Act_table,Data_table)
rm(Subj_table,Act_table,Data_table)
```

    ## 'data.frame':    10299 obs. of  68 variables:
    ##  $ subjects                   : Factor w/ 30 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ activity                   : Factor w/ 6 levels "WALKING","WALKING_UPSTAIRS",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ tBodyAcc.mean...X          : num  0.289 0.278 0.28 0.279 0.277 ...
    ##  $ tBodyAcc.mean...Y          : num  -0.0203 -0.0164 -0.0195 -0.0262 -0.0166 ...
    ##  $ tBodyAcc.mean...Z          : num  -0.133 -0.124 -0.113 -0.123 -0.115 ...
    ##  $ tBodyAcc.std...X           : num  -0.995 -0.998 -0.995 -0.996 -0.998 ...
    ##  $ tBodyAcc.std...Y           : num  -0.983 -0.975 -0.967 -0.983 -0.981 ...
    ##  $ tBodyAcc.std...Z           : num  -0.914 -0.96 -0.979 -0.991 -0.99 ...
    ##  $ tGravityAcc.mean...X       : num  0.963 0.967 0.967 0.968 0.968 ...
    ##  $ tGravityAcc.mean...Y       : num  -0.141 -0.142 -0.142 -0.144 -0.149 ...
    ##  $ tGravityAcc.mean...Z       : num  0.1154 0.1094 0.1019 0.0999 0.0945 ...
    ##  $ tGravityAcc.std...X        : num  -0.985 -0.997 -1 -0.997 -0.998 ...
    ##  $ tGravityAcc.std...Y        : num  -0.982 -0.989 -0.993 -0.981 -0.988 ...
    ##  $ tGravityAcc.std...Z        : num  -0.878 -0.932 -0.993 -0.978 -0.979 ...
    ##  $ tBodyAccJerk.mean...X      : num  0.078 0.074 0.0736 0.0773 0.0734 ...
    ##  $ tBodyAccJerk.mean...Y      : num  0.005 0.00577 0.0031 0.02006 0.01912 ...
    ##  $ tBodyAccJerk.mean...Z      : num  -0.06783 0.02938 -0.00905 -0.00986 0.01678 ...
    ##  $ tBodyAccJerk.std...X       : num  -0.994 -0.996 -0.991 -0.993 -0.996 ...
    ##  $ tBodyAccJerk.std...Y       : num  -0.988 -0.981 -0.981 -0.988 -0.988 ...
    ##  $ tBodyAccJerk.std...Z       : num  -0.994 -0.992 -0.99 -0.993 -0.992 ...
    ##  $ tBodyGyro.mean...X         : num  -0.0061 -0.0161 -0.0317 -0.0434 -0.034 ...
    ##  $ tBodyGyro.mean...Y         : num  -0.0314 -0.0839 -0.1023 -0.0914 -0.0747 ...
    ##  $ tBodyGyro.mean...Z         : num  0.1077 0.1006 0.0961 0.0855 0.0774 ...
    ##  $ tBodyGyro.std...X          : num  -0.985 -0.983 -0.976 -0.991 -0.985 ...
    ##  $ tBodyGyro.std...Y          : num  -0.977 -0.989 -0.994 -0.992 -0.992 ...
    ##  $ tBodyGyro.std...Z          : num  -0.992 -0.989 -0.986 -0.988 -0.987 ...
    ##  $ tBodyGyroJerk.mean...X     : num  -0.0992 -0.1105 -0.1085 -0.0912 -0.0908 ...
    ##  $ tBodyGyroJerk.mean...Y     : num  -0.0555 -0.0448 -0.0424 -0.0363 -0.0376 ...
    ##  $ tBodyGyroJerk.mean...Z     : num  -0.062 -0.0592 -0.0558 -0.0605 -0.0583 ...
    ##  $ tBodyGyroJerk.std...X      : num  -0.992 -0.99 -0.988 -0.991 -0.991 ...
    ##  $ tBodyGyroJerk.std...Y      : num  -0.993 -0.997 -0.996 -0.997 -0.996 ...
    ##  $ tBodyGyroJerk.std...Z      : num  -0.992 -0.994 -0.992 -0.993 -0.995 ...
    ##  $ tBodyAccMag.mean..         : num  -0.959 -0.979 -0.984 -0.987 -0.993 ...
    ##  $ tBodyAccMag.std..          : num  -0.951 -0.976 -0.988 -0.986 -0.991 ...
    ##  $ tGravityAccMag.mean..      : num  -0.959 -0.979 -0.984 -0.987 -0.993 ...
    ##  $ tGravityAccMag.std..       : num  -0.951 -0.976 -0.988 -0.986 -0.991 ...
    ##  $ tBodyAccJerkMag.mean..     : num  -0.993 -0.991 -0.989 -0.993 -0.993 ...
    ##  $ tBodyAccJerkMag.std..      : num  -0.994 -0.992 -0.99 -0.993 -0.996 ...
    ##  $ tBodyGyroMag.mean..        : num  -0.969 -0.981 -0.976 -0.982 -0.985 ...
    ##  $ tBodyGyroMag.std..         : num  -0.964 -0.984 -0.986 -0.987 -0.989 ...
    ##  $ tBodyGyroJerkMag.mean..    : num  -0.994 -0.995 -0.993 -0.996 -0.996 ...
    ##  $ tBodyGyroJerkMag.std..     : num  -0.991 -0.996 -0.995 -0.995 -0.995 ...
    ##  $ fBodyAcc.mean...X          : num  -0.995 -0.997 -0.994 -0.995 -0.997 ...
    ##  $ fBodyAcc.mean...Y          : num  -0.983 -0.977 -0.973 -0.984 -0.982 ...
    ##  $ fBodyAcc.mean...Z          : num  -0.939 -0.974 -0.983 -0.991 -0.988 ...
    ##  $ fBodyAcc.std...X           : num  -0.995 -0.999 -0.996 -0.996 -0.999 ...
    ##  $ fBodyAcc.std...Y           : num  -0.983 -0.975 -0.966 -0.983 -0.98 ...
    ##  $ fBodyAcc.std...Z           : num  -0.906 -0.955 -0.977 -0.99 -0.992 ...
    ##  $ fBodyAccJerk.mean...X      : num  -0.992 -0.995 -0.991 -0.994 -0.996 ...
    ##  $ fBodyAccJerk.mean...Y      : num  -0.987 -0.981 -0.982 -0.989 -0.989 ...
    ##  $ fBodyAccJerk.mean...Z      : num  -0.99 -0.99 -0.988 -0.991 -0.991 ...
    ##  $ fBodyAccJerk.std...X       : num  -0.996 -0.997 -0.991 -0.991 -0.997 ...
    ##  $ fBodyAccJerk.std...Y       : num  -0.991 -0.982 -0.981 -0.987 -0.989 ...
    ##  $ fBodyAccJerk.std...Z       : num  -0.997 -0.993 -0.99 -0.994 -0.993 ...
    ##  $ fBodyGyro.mean...X         : num  -0.987 -0.977 -0.975 -0.987 -0.982 ...
    ##  $ fBodyGyro.mean...Y         : num  -0.982 -0.993 -0.994 -0.994 -0.993 ...
    ##  $ fBodyGyro.mean...Z         : num  -0.99 -0.99 -0.987 -0.987 -0.989 ...
    ##  $ fBodyGyro.std...X          : num  -0.985 -0.985 -0.977 -0.993 -0.986 ...
    ##  $ fBodyGyro.std...Y          : num  -0.974 -0.987 -0.993 -0.992 -0.992 ...
    ##  $ fBodyGyro.std...Z          : num  -0.994 -0.99 -0.987 -0.989 -0.988 ...
    ##  $ fBodyAccMag.mean..         : num  -0.952 -0.981 -0.988 -0.988 -0.994 ...
    ##  $ fBodyAccMag.std..          : num  -0.956 -0.976 -0.989 -0.987 -0.99 ...
    ##  $ fBodyBodyAccJerkMag.mean.. : num  -0.994 -0.99 -0.989 -0.993 -0.996 ...
    ##  $ fBodyBodyAccJerkMag.std..  : num  -0.994 -0.992 -0.991 -0.992 -0.994 ...
    ##  $ fBodyBodyGyroMag.mean..    : num  -0.98 -0.988 -0.989 -0.989 -0.991 ...
    ##  $ fBodyBodyGyroMag.std..     : num  -0.961 -0.983 -0.986 -0.988 -0.989 ...
    ##  $ fBodyBodyGyroJerkMag.mean..: num  -0.992 -0.996 -0.995 -0.995 -0.995 ...
    ##  $ fBodyBodyGyroJerkMag.std.. : num  -0.991 -0.996 -0.995 -0.995 -0.995 ...

We create a separate table computing the average of the variables by activity and by subject

``` r
Avg_table<-All_table%>%group_by(activity,subjects)%>%summarize_all(mean)
```

    ## # A tibble: 180 x 68
    ## # Groups:   activity [?]
    ##    activity subjects tBodyAcc.mean...X tBodyAcc.mean...Y tBodyAcc.mean...Z
    ##      <fctr>   <fctr>             <dbl>             <dbl>             <dbl>
    ##  1  WALKING        1         0.2773308       -0.01738382        -0.1111481
    ##  2  WALKING        2         0.2764266       -0.01859492        -0.1055004
    ##  3  WALKING        3         0.2755675       -0.01717678        -0.1126749
    ##  4  WALKING        4         0.2785820       -0.01483995        -0.1114031
    ##  5  WALKING        5         0.2778423       -0.01728503        -0.1077418
    ##  6  WALKING        6         0.2836589       -0.01689542        -0.1103032
    ##  7  WALKING        7         0.2755930       -0.01865367        -0.1109122
    ##  8  WALKING        8         0.2746863       -0.01866289        -0.1072521
    ##  9  WALKING        9         0.2785028       -0.01808920        -0.1108205
    ## 10  WALKING       10         0.2785741       -0.01702235        -0.1090575
    ## # ... with 170 more rows, and 63 more variables: tBodyAcc.std...X <dbl>,
    ## #   tBodyAcc.std...Y <dbl>, tBodyAcc.std...Z <dbl>,
    ## #   tGravityAcc.mean...X <dbl>, tGravityAcc.mean...Y <dbl>,
    ## #   tGravityAcc.mean...Z <dbl>, tGravityAcc.std...X <dbl>,
    ## #   tGravityAcc.std...Y <dbl>, tGravityAcc.std...Z <dbl>,
    ## #   tBodyAccJerk.mean...X <dbl>, tBodyAccJerk.mean...Y <dbl>,
    ## #   tBodyAccJerk.mean...Z <dbl>, tBodyAccJerk.std...X <dbl>,
    ## #   tBodyAccJerk.std...Y <dbl>, tBodyAccJerk.std...Z <dbl>,
    ## #   tBodyGyro.mean...X <dbl>, tBodyGyro.mean...Y <dbl>,
    ## #   tBodyGyro.mean...Z <dbl>, tBodyGyro.std...X <dbl>,
    ## #   tBodyGyro.std...Y <dbl>, tBodyGyro.std...Z <dbl>,
    ## #   tBodyGyroJerk.mean...X <dbl>, tBodyGyroJerk.mean...Y <dbl>,
    ## #   tBodyGyroJerk.mean...Z <dbl>, tBodyGyroJerk.std...X <dbl>,
    ## #   tBodyGyroJerk.std...Y <dbl>, tBodyGyroJerk.std...Z <dbl>,
    ## #   tBodyAccMag.mean.. <dbl>, tBodyAccMag.std.. <dbl>,
    ## #   tGravityAccMag.mean.. <dbl>, tGravityAccMag.std.. <dbl>,
    ## #   tBodyAccJerkMag.mean.. <dbl>, tBodyAccJerkMag.std.. <dbl>,
    ## #   tBodyGyroMag.mean.. <dbl>, tBodyGyroMag.std.. <dbl>,
    ## #   tBodyGyroJerkMag.mean.. <dbl>, tBodyGyroJerkMag.std.. <dbl>,
    ## #   fBodyAcc.mean...X <dbl>, fBodyAcc.mean...Y <dbl>,
    ## #   fBodyAcc.mean...Z <dbl>, fBodyAcc.std...X <dbl>,
    ## #   fBodyAcc.std...Y <dbl>, fBodyAcc.std...Z <dbl>,
    ## #   fBodyAccJerk.mean...X <dbl>, fBodyAccJerk.mean...Y <dbl>,
    ## #   fBodyAccJerk.mean...Z <dbl>, fBodyAccJerk.std...X <dbl>,
    ## #   fBodyAccJerk.std...Y <dbl>, fBodyAccJerk.std...Z <dbl>,
    ## #   fBodyGyro.mean...X <dbl>, fBodyGyro.mean...Y <dbl>,
    ## #   fBodyGyro.mean...Z <dbl>, fBodyGyro.std...X <dbl>,
    ## #   fBodyGyro.std...Y <dbl>, fBodyGyro.std...Z <dbl>,
    ## #   fBodyAccMag.mean.. <dbl>, fBodyAccMag.std.. <dbl>,
    ## #   fBodyBodyAccJerkMag.mean.. <dbl>, fBodyBodyAccJerkMag.std.. <dbl>,
    ## #   fBodyBodyGyroMag.mean.. <dbl>, fBodyBodyGyroMag.std.. <dbl>,
    ## #   fBodyBodyGyroJerkMag.mean.. <dbl>, fBodyBodyGyroJerkMag.std.. <dbl>

And then export the data in csv format.

``` r
write.csv(All_table,"All_means_and_sd.csv",row.names = FALSE)
write.csv(Avg_table,"Avg_by_activity.csv",row.names = FALSE)
```

Footnotes
---------

*1 - Smartlab - Non-Linear Complex Systems Laboratory DITEN - Università degli Studi di Genova, Genoa (I-16145), Italy.*

*2 - CETpD - Technical Research Centre for Dependency Care and Autonomous Living Universitat Politècnica de Catalunya (BarcelonaTech). Vilanova i la Geltrú (08800), Spain*
