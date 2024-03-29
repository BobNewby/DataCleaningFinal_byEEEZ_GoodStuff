# DataCleaningFinal

## Obtaining and reading data

These scripts where executed for obtaining and saving the data into variables.

```{r}
zipFile <- "data.zip"
urlData <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(urlData, zipFile)
unzip(zipFile)
unlink(zipFile)

install.packages("data.table")
install.packages("dplyr")
library(data.table)
library(dplyr)

features <- read.table("UCI HAR Dataset/features.txt")
activities <- read.table("UCI HAR Dataset/activity_labels.txt")

trainingSubjects <- read.table("UCI HAR Dataset/train/subject_train.txt")
trainingActivities <- read.table("UCI HAR Dataset/train/y_train.txt")
trainingFeatures <- read.table("UCI HAR Dataset/train/X_train.txt")

testSubjects <- read.table("UCI HAR Dataset/test/subject_test.txt")
testActivities <- read.table("UCI HAR Dataset/test/y_test.txt")
testFeatures <- read.table("UCI HAR Dataset/test/X_test.txt")
```

# 1. Merge

The merged data from test and training.

```{r}
subjects <- rbind(trainingSubjects, testSubjects)
activities <- rbind(trainingActivities, testActivities)
features <- rbind(trainingFeatures, testFeatures)

featureNames <- read.table("UCI HAR Dataset/features.txt")
activityLabels <- read.table("UCI HAR Dataset/activity_labels.txt")
colnames(features) <- t(featureNames[2])

colnames(activities) <- "Activity"
colnames(subjects) <- "Subject"
mergedData <- cbind(features, activities, subjects)
```

# 2. Mean and standard deviation
There was rearched all the patterns from the expressions for standard deviation and mean

```{r}
meanSTDcols <- grep(".*Mean.*|.*Std.*", names(mergedData), ignore.case = T)
requiredColumns <- c(meanSTDcols, 562, 563)
extractedData <- mergedData[, requiredColumns]
```

# 3. Descriptive activity names
All the names were converted into factor.

```{r}
extractedData$Activity <- as.character(extractedData$Activity)

for (i in 1:6){
  extractedData$Activity[extractedData$Activity == i] <- as.character(activityLabels[i, 2])
}

extractedData$Activity <- as.factor(extractedData$Activity)
```

# 4. Labels
Prefixes and suffixes were replaced by descriptive words.

```{r}
names(extractedData)<-gsub("Acc", "Accelerometer", names(extractedData))
names(extractedData)<-gsub("Gyro", "Gyroscope", names(extractedData))
names(extractedData)<-gsub("BodyBody", "Body", names(extractedData))
names(extractedData)<-gsub("Mag", "Magnitude", names(extractedData))
names(extractedData)<-gsub("^t", "Time", names(extractedData))
names(extractedData)<-gsub("^f", "Frequency", names(extractedData))
names(extractedData)<-gsub("tBody", "TimeBody", names(extractedData))
names(extractedData)<-gsub("-mean()", "Mean", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("-std()", "Standard deviation", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("-freq()", "Frequency", names(extractedData), ignore.case = TRUE)
names(extractedData)<-gsub("angle", "Angle", names(extractedData))
names(extractedData)<-gsub("gravity", "Gravity", names(extractedData))
```

# 5. Tidy data set
Finally, the tiny dataset was obtained and saved into a flat text file.

```{r}
extractedData$Subject <- as.factor(extractedData$Subject)
extractedData <- data.table(extractedData)
tidyData <- aggregate(. ~Subject + Activity, extractedData, mean)
tidyData <- tidyData[order(tidyData$Subject,tidyData$Activity),]
write.table(tidyData, file = "Tidy.txt", row.names = FALSE)
```