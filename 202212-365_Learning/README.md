# Predict whether a Free Learning Plan user would convert to a paid subscriber or not.

## Introduction to the data challenge

365 Data Science is an online learning platform specializing in data science courses.
https://learn.365datascience.com/

![365 Data Science](images/365_data_science.jpg?raw=true "365 Data Science learning platform")

👨‍🎓 👩‍🎓 Students learn by watching video content, then evaluate their knowledge by taking quizzes, practice exams, course exams, and career track exams.

🎯 The challenge is to build a machine learning model to predict whether a Free Plan user would convert to a paid subscriber or not.

## Data

The data is a collection of 11 .csv files
- 365_student_info.csv
- 365_student_purchases.csv
- 365_student_learning.csv
- 365_course_info.csv
- 365_course_ratings.csv
- 365_student_quizzes.csv
- 365_quiz_info.csv
- 365_student_exams.csv
- 365_exam_info.csv
- 365_student_engagement.csv
- 365_student_hub_questions.csv

## Exploratory Data Analysis (single datasets)

-	The 2 main student countries are India and the US.
![Number of students per country](images/Nb_students_per_country.png?raw=true "Nb students per country")
-	There are 5 main subscription peaks corresponding to 365 data science special events like Winter Sales 2022. There is also a lesser peak for the platform gamification introduction.
![Number of subscriptions over time](images/Nb_subscriptions_over_time.png?raw=true "Nb subscriptions over time")
-	There are 2 registration peaks in June and mid-August. They are also related to 365 data science special events, respectively Data Science Summer Campaign 2022 and Most Wanted Campaign.
![Number of registrations over time](images/Nb_registrations_over_time.png?raw=true "Nb registrations over time")
-	There is a peak of video watching mid-august.
![Number of days of video watched over time](images/Nb_video_watched_days_over_time.png?raw=true "Number of days of video watched over time")
-	Almost all courses are well noted. We should not get much information from those ratings.
![Course ratings distribution](images/Course_ratings_distribution.png?raw=true "Course ratings distribution")
-	In the exam info table, there are a lot of missing values for the exam category. It could be interesting to know which exams are course/career track exam vs practice exam. We might infer those category by relating exam completion date with course watched in the days preceding the exam.
-	Students generally convert after 2 weeks after entering the platform (this assumption was also provided in the guided instructions).
![Subscriptions x days after registration](images/Subscriptions_after_number_of_days_after_registration.png?raw=true "Subscriptions x days after registration")

## Data modeling & ML-ready dataset building

From the last insight, I decided to build the ML-ready dataset based on student registration and their behavior for the next 14 days. The target variable being the boolean “subscribed” during this 14 days period.

## Exploratory Data Analysis (ML-ready dataset)

💡 The target variable (subscribed) is very imbalanced:
- 6% of the students subscribed within the first 14 days after registration
- 94% did not

![Target variable distribution](images/Subscribed_distribution.png?raw=true "Target variable distribution")

Here are the relationships between the target variable and 2 others variables:
- Average exams score
- Number of days with video watching within the first 14 days after registration

![Target variable & Average exams score & Number of days with videos](images/Corr_target_var_and_avg_exam_res_and_watch_days.png?raw=true "Target variable & Average exams score & Number of days with videos")

💡 We can see that the more you watch videos, the more you convert to a paid subscriber. 
There seems to be also a link with the average exams score.

Here is the correlation matrix:

![Correlation matrix](images/Corr_analysis.png?raw=true "Correlation matrix")

💡"Average exams results" and "is engaged with exams" are very correlated features.
💡"Engagement_days" and "is engaged with exams" are very correlated features.
"Average exams results" and "Engagement_days" features are removed from the dataset.

Here are all pairplots for all features couples:

![All relationships](images/Relationships_between_variables.png?raw=true "All relationships")


## Choice of the evaluation metric

As I get eventually a very imbalanced classification (6% who subscribed/94% who did not subscribe), I have to choose the right evaluation metric. Accuracy should not be used as any naïve algorithm that would predict “100% student did not subscribe” would get around a 94% accuracy… 

I used the recall metric instead. I could have used other metrics for imbalanced dataset such as imbalanced accuracy, F1 or F2 score. Recall is the fraction of subscribed students that are successfully retrieved.
📝 Recall = # True positives / # positives = TP / (TP+FN).

The other tricks for imbalanced classification to preserve the same ratio of the target variable in different sets are:
-	stratified splits for train/test split
-	stratified K fold for validation

## Pre-processing

Features are not on the same scale:

![Features scales](images/Features_scale.jpg?raw=true "Features scales")

💡 We will apply features scaling in our ML pipeline in order to have features on the same scale for most of our ML algorithms. 

## Model building

I evaluated 4 classifiers: 
-	Logistic regression
-	KNN
-	Random Forest
-	LightGBM

They are trained based on the recall metric.

## Models evaluation

Here are the tests results:

![Evaluation results](images/Evaluation_results.jpg?raw=true "Evaluation results")

![Evaluation results](images/Evaluation_results-Recall.jpg?raw=true "Evaluation results")

Here is the confusion metrix for the best model based on LightGBM:

![Confusion matrix](images/Confusion_matrix.jpg?raw=true "Confusion matrix")

So, we have 199 True Positive, they are the students who are likely to convert. 365 Data Science needs to encourage them with offers.
And we have 228 False Negative, they are the sudents who did not receive any offers but they were likely to convert.
Recall = # True positives / # positives = TP / (TP+FN) = 199 / (199+228) = 0.46
And finally, we have the 60 False Negative, they are the sudents who did receive offers but they were not likely to convert.

Now, look at the Precision and Recall versus the decision threshold:

![Precision Recall](images/Precision_Recall.jpg?raw=true "Precision Recall")

The decision threshold should be adapted to foster a lesser number of False Negative in order to better target them.

## Final comments

What I could have done (if time permits):
-	During EDA, I haven’t performed any hypothesis testing.
-	During preprocessing, I have not run a proper features selection. 
-	I have not performed Oversampling/Undersampling techniques.

An experiment that 365 Data Science could do would be an A/B test: does gamification increase engagements and then subscriptions?
