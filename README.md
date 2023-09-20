# Bellabeat: Fitness Data Case Study
##### By Dilpreet Singh
## Background

Bellabeat is a high-tech manufacturer of health-focused products for women. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with
knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women. Our team has been asked to focus on a Bellabeat product and analyze smart device usage data to gain insight into how people are already using their smart devices. Then, using this information, stakeholders would like high-level recommendations for how these trends can inform Bellabeat marketing strategy.

## Ask

*Business Question:*

Analyze Fitbit data to gain insight and help guide marketing strategy for Bellabeat to grow as a global player. 

*Key Stakeholders:*


1.	Urška Sršen
2.	Sando Mur

*Deliverable:* 

Use the dataset to focus on a Bellabeat product and analyze smart device usage data to gain insight into how people are already using their smart devices. Clean the Data along with the relevant documentation. Then after analyzing the data, help the team with recommendations for how these trends can inform Bellabeat marketing strategy. In the end, use some appealing visualizations to present and support your findings to the stakeholders. 

## Prepare

The data contains Fitness Tracker Data collected by Mobius for 30 individuals who consented to this submission. For this project, I will be only using 5 of the total data tables for my analysis, which include `dailyActivity_merged`, `dailyIntensities_merged`, `hourlySteps_merged`, `sleepDay_merged`, `weightLogInfo_merged`.

Link to Data Source: [Click Here](https://www.kaggle.com/datasets/arashnic/fitbit)

## Process

For this dataset, I will be using spreadsheets to clean the data, SQL for extracting information using queries and analyzing the results within SQL using different functions of filtering and sorting. Later, I will be using Tableau to prepare meaningful visualizations which will support my analysis.

**Data Cleaning**

* Removed 3 duplicate rows in “sleepDay_merged” table.
* Changed the date and time format in “hourlySteps_merged” table.
* Checked for any empty cells throughout the 5 data tables.
* Used conditional formatting to look for any possible outlier throughout the data tables.

## Analyze

Now that we have assured the data integrity, we are finally at the analyze phase where we perform some functions to extract data and get answer to the stakeholder’s question: “Analyze Fitbit data to gain insight and help guide marketing strategy for Bellabeat to grow as a global player.” 

1. Used SQL Query to calculate the total number of participants for 3 of the data tables. (Other datasets were too big and not supported for this query by Bigquery because of the standard version).


   SELECT 

COUNT (DISTINCT `bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.Id) AS Participants_Activity,
COUNT (DISTINCT `bellabeat-data-project.Fitbit_Dataset.sleepDay_merged`.Id) AS Participants_Sleep,
COUNT (DISTINCT `bellabeat-data-project.Fitbit_Dataset.weightLogInfo_merged`.Id) AS Participants_Weight

FROM `bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`,`bellabeat-data-project.Fitbit_Dataset.sleepDay_merged`,`bellabeat-data-project.Fitbit_Dataset.weightLogInfo_merged`;


![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/7ca1231c-e778-4e3e-be4c-ec2386c97c3e)

This Check shows that not all the participants have used all the functions of the smart device offered by Bellabeat. Only 24 users used sleep tracker and only 8 used weight logs.
Things to note here are: 

* Not all users use sleep tracker and weight log.
* Weight log is used the least which asks for further analysis.

2. Used Avg() function in SQL to calculate the average distance, steps, sedentary mins, fairly active mins, lightly active mins, very active mins, sleep mins, time in bed, these calculations will then be very useful to answer many of stakeholder’s questions.


SELECT 

AVG (`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.TotalDistance) AS Average_Distance,
AVG (`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.TotalSteps) AS Average_Steps,
AVG (`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.SedentaryMinutes) AS Average_SedentaryMins,
AVG (`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.FairlyActiveMinutes) AS Average_FairlyActiveMins,
AVG (`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.VeryActiveMinutes) AS Average_VeryActiveMins,
AVG (`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.LightlyActiveMinutes) AS Average_LightlyActiveMins,
AVG (`bellabeat-data-project.Fitbit_Dataset.sleepDay_merged`.TotalMinutesAsleep) AS Average_SleepMins,
AVG (`bellabeat-data-project.Fitbit_Dataset.sleepDay_merged`.TotalTimeInBed) AS Average_TimeinBed,

FROM


`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`,`bellabeat-data-project.Fitbit_Dataset.sleepDay_merged`

![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/3a100b62-862f-4ebe-9b96-750df7ce713d)

In a study published in March 2020 in the Journal of the American Medical Association, researchers found that surpassing the common benchmark of 4,000 daily steps (considered low for adults) had substantial health benefits. They noted that achieving 8,000 steps daily was associated with a 51% lower risk of all-cause mortality, and 12,000 steps with a remarkable 65% lower risk, compared to those taking only 4,000 steps. These findings align with CDC recommendations, which suggest aiming for 10,000 daily steps. Looking at this dataset, making the average of 7,638 steps for many adults fall short for optimizing health and longevity.

1.	Used GROUP BY() and ORDER BY() functions to check the co relation between the days of the week and average very active minutes. 


SELECT 
    ActivityDate,
    AVG(`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.TotalDistance) AS Average_Distance,
    AVG(`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.VeryActiveMinutes) AS Average_VeryActiveMins,
    AVG(`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.TotalSteps) AS Average_Steps
 FROM
    `bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`
GROUP BY 
    ActivityDate
ORDER BY 
    Average_VeryActiveMins 
    DESC


![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/b98f7886-0231-4143-8f14-2e6aac46b3d0)

After looking at this result of my query, I noted the most active average days on calendar which shows that people were lease active on Sundays and Thursdays which accounts for more data to support this. But it still is an interesting face to note down. One of the possible reasons could be because people may like to sit and relax before Mondays, as they have to go to work and probably get more active while working.

  ![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/94d7c9f8-1784-4b63-8b58-aa678b711b92)

2.	I noticed in the preview tab of the dailyActivity_merged table that some of the days there were instances of 0 steps which doesn’t sound too normal because a person usually be making few steps in their routine even if they are at home. This could possibly indicate malfunctioning of the device, or the battery running out etc.


    SELECT 

COUNT(`bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.TotalSteps) AS Zero_Steps

FROM `bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`

WHERE `bellabeat-data-project.Fitbit_Dataset.dailyActivity_merged`.TotalSteps=0


![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/071d2af4-0bf6-44b2-88b7-48fd4cd94446)

3.	Now I wanted to check what time of the day the users are most active which could give our executives an idea of user behaviour possibly helping them to promote the product for other times of the day too. 
To do this, I will first have to split the date and time component in SQL for which I will use the “TIMESTAMP” function.


SELECT
  DATE(TIMESTAMP(ActivityHour)) AS date_component,
  TIME(TIMESTAMP(ActivityHour)) AS time_component,
  AVG(StepTotal) AS Steps
FROM `bellabeat-data-project.Fitbit_Dataset.hourlySteps_merged`
GROUP BY date_component, time_component
ORDER BY Steps DESC


![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/24dfa688-6f63-4e71-afde-1c7218b984f4)

The data reveals that participants showed their highest levels of activity during the late afternoon, specifically between 17:00 and 19:00. Afternoons also saw a notable level of activity, while nighttime registered as the least active period. This insight underscores the potential effectiveness of advertising during the 17:00-19:00 timeframe, as it aligns with when individuals tend to be more active, engaging with their devices and messages when they have leisure time available.

## Share

Visualizations are a great way of depicting our key findings and can serve as a one stop shop for our stakeholders to get answers to most of the possible questions about a dataset. 

![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/14bbd384-10e4-4481-988c-43872a8bc44b)

![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/8151401e-2ee6-462e-96cd-afa9e1d7344e)

![image](https://github.com/DpHundal/Fitness-Data-Project/assets/139656045/bd5d310c-051e-4afd-8f9b-4f2c3861b4fd)

## Act

Conclusion: 
* A subset of Fitbit users opts not to utilize the sleep tracking and weight logging features.
* Surprisingly, the weight logging functionality stands as the least favored among Fitbit's repertoire of functions.
* It is worth noting that not all users diligently monitored their daily step count.
* The participants exhibited their highest activity levels during the early evening hours, approximately between 17:00 and 19:00, while displaying moderate activity levels in the afternoons.

Top Three Recommendations:
1.	A comprehensive analysis, ideally through a survey, should be conducted to uncover the underlying reasons behind the unpopularity of the weight logging feature. This inquiry should delve into factors such as the feature's perceived relevance and ease of use.

2.	A further in-depth analysis, potentially facilitated by a survey, is warranted to understand why some participants failed to track their daily steps consistently. This investigation should encompass aspects such as battery life, the feature's relevance, the device's charging system, and overall device design.

3.	To encourage users to remain active throughout the day, especially outside the 17:00-19:00 window, Bellabeat could effectively employ push notifications. These messages could highlight the risks associated with prolonged sedentary periods and the health benefits of achieving a daily step count exceeding 8,000 steps. This proactive approach can serve to motivate users towards a healthier and more active lifestyle.





