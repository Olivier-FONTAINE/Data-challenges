# Predict whether a Free Learning Plan user would convert to a paid subscriber or not.

## Introduction to the data challenge

365 Data Science is an online learning platform specializing in data science courses.
https://learn.365datascience.com/

![365 Data Science](images/365_data_science.jpg?raw=true "365 Data Science learning platform")

Students learn by watching video content, then evaluate their knowledge by taking quizzes, practice exams, course exams, and career track exams.

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

## Exploratory Data Analysis

-	The 2 main student countries are India and the US.
![Number of students per country](images/Nb_students_per_country.png?raw=true "Nb students per country")
-	There are 5 main subscription peaks corresponding to 365 data science special events like Winter Sales 2022. There is also a lesser peak for the platform gamification introduction.
![Number of subscriptions over time](images/Nb_subscriptions_over_time.png?raw=true "Nb subscriptions over time")
-	There are 2 registration peaks in June and mid-August. They are also related to 365 data science special events, respectively Data Science Summer Campaign 2022 and Most Wanted Campaign.
![Number of registrations over time](images/Nb_registrations_over_time.png?raw=true "Nb registrations over time")
-	There is a peak of video watching mid-august.
-	Almost all courses are well noted. We should not get much information from those ratings.
![Course ratings distribution](images/Course_ratings_distribution.png?raw=true "Course ratings distribution")
-	In the exam info table, there are a lot of missing values for the exam category. It could be interesting to know which exams are course/career track exam vs practice exam. We might infer those category by relating exam completion date with course watched in the days preceding the exam.
-	Students generally convert after 2 weeks after entering the platform (this assumption was also provided in the guided instructions).
![](images/Subscriptions_after_number_of_days_after_registration.png?raw=true "Subscriptions x days after registration")
