

First I read the different pieces of information and store them in variables
This means that I need to read the x_test, y_test, subject_test files and teh same ones but with the train sufix
I put all of the 6 files in a variable called the same but with the sufix data_
I alos read the fetures tables that contains the 561 tyeps of movements that I will use later to indicate the 
the columns of the final data
Finally I read the activity lables files that have the 6 types of activities

the Last 2 files are put in variables with the same name as the file


###### Step 1
###### Merge the training and test sets for the 3 pieces of data to create one data set for each
This means that the data_ with the x and y test and train are put in a variable with sufix together.
In this way I manage to have 3 files that join together the test and train varible.
See below the code

data_x_together<-rbind(data_x_test, data_x_train)
data_subject_together<-rbind(data_subject_test, data_subject_train)
data_y_together<-rbind(data_y_test, data_y_train)

###### change the names of the columns to make them more descriptive
## change name of column of subject
data_subject_together<-rename(data_subject_together, c("V1"="subject_id"))
## change name of column of activity labels
data_y_together<-rename(data_y_together, c("V1"="labels"))
## change name of column of measures and 
## Step 2 - keep only mean and std measures
## I do this by taking any of the measures that contains the workd mean or std in it.
fields_mean_std<-c(grep ("mean", features$V2), grep ("std", features$V2))
features[fields_mean_std,2]
narrow <-data.frame()
narrow <- data_x_together[, c(fields_mean_std)]



## Step 4 put only descriptive variable names
colnames(narrow) <- features[fields_mean_std,2] 




### Step 3 use descriptive activity names to name the activities in the data set 
data_y_together[data_y_together==1]<-"WALKING           "
data_y_together[data_y_together==2]<-"WALKING_UPSTAIRS  "
data_y_together[data_y_together==3]<-"WALKING_DOWNSTAIRS"
data_y_together[data_y_together==4]<-"SITTING           "
data_y_together[data_y_together==5]<-"STANDING          "
data_y_together[data_y_together==6]<-"LAYING            "


### Combine all 
final<-cbind(narrow, data_subject_together, data_y_together)


### Step 5 agregate by activity and subject with average of each variable

agregational<-ddply(final, .(subject_id, labels), colwise(mean))




#### Delivery
write.table(agregational, file = "delivery_course", row.name=FALSE)