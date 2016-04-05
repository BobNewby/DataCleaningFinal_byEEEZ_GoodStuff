# Scope

The current document describes all the variable meanings for this repository.

# Data

The data was downloaded from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

# Inputs

X_train.txt: features used for training.
y_train.txt: activities corresponding to features in X_train.txt.
subject_train.txt: information about the subjects participating in the recollection of data.
X_test.txt: features used for testing.
y_test.txt: activities corresponding to features in X_test.txt.
subject_test.txt: information about the subjects participating in the recollection of data.
activity_labels.txt: metadata of the activities.
features.txt: names of the features in the datasets.

# Transformations

Following are the transformations that were performed on the input dataset:

X_train.txt was transformed to trainingFeatures.
y_train.txt was transformed to trainingActivities.
subject_train.txt was transformed to trainingSubjects.
X_test.txt was transformed to testFeatures.
y_test.txt was transformed to testActivities.
subject_test.txt was transformed to testSubjects.
features.txt was transformed to testFeatures.
activity_labels.txt was transformed to activityLabels.

Subjects in training and test are merged into "subjects".
Activities in training and test are merged into "activities".
Features of test and training are merged into "features".
The name of the features are set in features from "featureNames".
"features", "activities" and "subjects" are merged into "mergedData".

Prefixes and abbreviation from extractedData were transformed into descriptive names
"newdata.txt" contains the tidy data merged.

# Output Data Set

"newdata" is the output file as result of all of the above operations. The name of the coulmns are descriptive and the data is tidy.