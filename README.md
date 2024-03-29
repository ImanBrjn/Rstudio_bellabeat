# bellabeat

A Goolge Data Analytics Case Study

## Introduction
This is my case study project for the [Google Data Analytics Professional Certificate published by coursera](https://www.coursera.org/professional-certificates/google-data-analytics).In this case study, I am a junior data analyst working on the marketing analyst team at [Bellabeat](https://bellabeat.com), a high-tech manufacturer of health-focused products for women. I performed a real-world task as a junior data analyst. Bellabeat is a successful small company, but it has the potential to become a larger player in the global smart device market. Urška Sršen, co-founder, and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. I have been asked to focus on one of Bellabeat's products and analyze smart device data to gain insight into how consumers are using their smart devices. The insights I discover will then help guide the marketing strategy for the company.


## About the Company
Bellabeat was founded by Urška Sršen and Sando Mur, a high-tech company that manufactures health-focused smart products. Sršen utilized her background as an artist to develop beautifully designed technology that informs and inspires women worldwide. By collecting data on activity, sleep, stress, and reproductive health, Bellabeat empowers women with knowledge about their health and habits. Since its establishment in 2013, Bellabeat has experienced rapid growth, positioning itself as a tech-driven wellness company for women. By 2016, Bellabeat had opened offices globally and introduced multiple products. These products became available through an increasing number of online retailers, alongside their own e-commerce channel on their website. While the company has invested in traditional advertising media like radio, out-of-home billboards, print, and television, it predominantly focuses on extensive digital marketing.

## Analysis Process
To address the key business questions, I adhered to the steps of the data analysis process: asking, preparing, processing, analyzing, sharing, and acting.

### 1- Ask
#### The Business Task
Sršen has requested an analysis of smart device usage data to gain insights into how consumers utilize non-Bellabeat smart devices. The goal is to apply these insights to select a specific Bellabeat product.

#### Key Stakeholders
**Urška Sršen**: Cofounder and Chief Creative Officer at Bellabeat.   
**Sando Mur**: Mathematician, Cofounder, and a key member of Bellabeat's executive team.   
**Bellabeat Marketing Analytics Team**: Data analysts responsible for collecting, analyzing, and reporting data to guide Bellabeat's marketing strategy.

### 2- Prepare
The data used in this case study can be found in the [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit), made available through [Mobius](https://www.kaggle.com/arashnic) on Kaggle. This dataset comprises personal fitness tracker data from thirty Fitbit users who provided minute-level output for physical activity, heart rate, and sleep monitoring. The information includes details about daily activity, steps, and heart rate, offering insights into users' habits. The dataset consists of 18 CSV files generated by respondents to a distributed survey via Amazon Mechanical Turk between 03.12.2016 and 05.12.2016. Individual reports can be parsed by export session ID (column A) or timestamp (column B). The variation between output represents the use of different types of Fitbit trackers and individual tracking behaviors/preferences.

## 2- Process

#### Tool
I used ***R Studio*** to conduct this case study, and I wrote this report using *R Markdown*. To perform the analysis, I first installed and loaded the required packages and libraries in R Studio.
```{r}
# Loading packages for data manipulation and visualization

library(tidyverse)
library(lubridate) 
library(dplyr)
library(ggplot2)
library(tidyr)
library(readr)
```
![Start](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/dc38d3c1-31fd-4dcc-bda0-5d952acb900e)   
Then, I loaded the necessary datasets from my local computer location into R Studio.
```{r}
# Loading data from my local storage
# Two dots indicates the local folder

activity <- read.csv("../Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
calories <- read.csv("../Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
intensities <- read.csv("../Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
sleep <- read.csv("../Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
weight <- read.csv("../Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
```
#### Data exploring
After loading my packages and data, I checked my tables to ensure they were properly imported.
```{r}
# Checking columns and number of rows of data setss

# activity
glimpse(activity)

# calories
glimpse(calories)

# intensities
glimpse(intensities)

# sleep
glimpse(sleep)

# weight
glimpse(weight)
```
![activity glimpse](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/a0232db9-6644-429a-a1a8-6d44219433e9)   
![calories glimps](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/51900b80-38f5-4c0a-8e6a-4f717964d63b)   
![intensities glimpse](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/4b581e36-317f-4a21-9302-fbe4e7ca1d53)   
![sleep glimpse](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/8f7a3904-17d3-482b-96c6-3495072cc5e6)   
![weight glimpse](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/92a16bea-9757-40e6-ac72-627c01ea3808)   
#### Cleaning data
By looking at the datasets, I noticed that some columns include both *date* and *time* in the same cells. I decided to separate these two into different columns for each dataset, maintaining the same date and time format.
```{r}
# Separating time and date for each table

# intensities
intensities$ActivityHour=as.POSIXct(intensities$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
intensities$time <- format(intensities$ActivityHour, format = "%H:%M:%S")
intensities$date <- format(intensities$ActivityHour, format = "%m/%d/%y")

# calories
calories$ActivityHour=as.POSIXct(calories$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
calories$time <- format(calories$ActivityHour, format = "%H:%M:%S")
calories$date <- format(calories$ActivityHour, format = "%m/%d/%y")

# activity
activity$ActivityDate=as.POSIXct(activity$ActivityDate, format="%m/%d/%Y", tz=Sys.timezone())
activity$date <- format(activity$ActivityDate, format = "%m/%d/%y")

# sleep
sleep$SleepDay=as.POSIXct(sleep$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
sleep$date <- format(sleep$SleepDay, format = "%m/%d/%y")

#weight
weight$Date=as.POSIXct(weight$Date, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
weight$time <- format(weight$Date, format = "%H:%M:%S")
weight$date <- format(weight$Date, format = "%m/%d/%y")
```

Next, I checked for any missing values.
```{r}
# Checking for missing values

# activity
any(is.na(activity))
# calories
any(is.na(calories))
# intensities
any(is.na(intensities))
# sleep
any(is.na(sleep))
# weight
any(is.na(weight))
```
![missing values](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/66902451-d727-4aeb-9279-f29669ac8f72)   

**TRUE** indicates that there are some missing values, and by checking the data sets, it is obvious that there are some *NA* in the *FAT* column. I don't need this column, so I decided to omit it.
```{r}
# Omit "fat" column

weight <- weight[, !(names(weight) %in% c("Fat"))]

# Check "weight" data sets again
any(is.na(weight))
```
![recheck](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/b6e181a2-1ee1-47f6-a4c3-e13eea101d82)    
Next, I checked the datasets for any duplicates
```{r}
# Checking for duplicates
sum(duplicated(activity))
sum(duplicated(calories))
sum(duplicated(intensities))
sum(duplicated(sleep))
sum(duplicated(weight))
```
![duplicates](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/27de4861-c8c9-4db4-ae1d-3a8f67be4123)   
The result shows that there are three duplicate rows in the *sleep* dataset. So, I removed the duplicates.
```{r}
# Removing duplicates
sleep <- sleep %>% distinct()

# Checking again
sum(duplicated(sleep))
```
![remove duplicates](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/8d2b4138-1845-4162-9774-dd8b9cebbbc3)   
I wanted to make a comparison between "activity" and "sleep". So, I decided to merge these two datasets into a dataset called "**merged_df**" using an **inner join** on columns *Id* and *date* (that I previously created after converting data to date-time format).
```{r}
# Merging activity and sleep 
merged_df <- merge(sleep, activity, by=c('Id', 'date'))
```
### 4- Analyze

Now that my data is stored appropriately and has been prepared for analysis, it's time to put it to work.

#### Statistical
I first checked the number of participants in each category.
```{r}
# Number of participants
n_distinct(activity$Id)  
n_distinct(calories$Id)   
n_distinct(intensities$Id)
n_distinct(sleep$Id)
n_distinct(weight$Id)
```
![participants](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/1103e549-a46a-4733-a930-ff8b322d79d7)    
The results show that there are 33 participants in the activity, calories, and intensities datasets, 24 in the sleep dataset, and only 8 in the weight dataset. Having only 8 participants in the "weight" dataset is not significant enough to make any recommendations or conclusions based on this data. More data would be needed to draw strong recommendations or conclusions.   
Now, let’s take a look at the summary statistics of the datasets:
```{r}
# Checking summery statistics

# activity
activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>%
  summary()

# explore number of active minutes per category
activity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()

# calories
calories %>%
  select(Calories) %>%
  summary()
# sleep
sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()
# weight
weight %>%
  select(WeightKg, BMI) %>%
  summary()
```
![summery](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/42e69e91-07ca-4cb1-9230-ffc9df5d6083)   
From the results, we can conclude that:
1- The average number of steps per day is 7638. According to [CDC research](https://www.cdc.gov/physicalactivity/basics/pa-health/index.htm), taking 8,000 steps per day was associated with a 51% lower risk for all-cause mortality. Taking 12,000 steps per day was associated with a 65% lower risk compared with taking 4,000 steps.
2- Participants are sedentary for about 16 hours, which is too much and needs to be reduced.
3- The majority of the participants are lightly active.
4- On average, participants sleep for almost 7 hours. However, they seem to struggle to fall asleep, as indicated by the mean of total time in bed.
#### Visualization
```{r}
#Total Steps vs. Calories

ggplot(data=activity, aes(x=TotalSteps, y=Calories)) + 
  geom_point() + geom_smooth() + labs(title="Total Steps vs. Calories")
```
![Total Steps vs  Calories](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/71924e1f-1698-4c08-871d-02d3316954aa)   
This scatter chart indicates a positive correlation between the total number of steps taken and calories burned, which is expected. The more active individuals are, the more calories they burn.
```{r}
# Total Minutes Asleep vs. Total Time in Bed

ggplot(data=sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + 
  geom_point()+ labs(title="Total Minutes Asleep vs. Total Time in Bed")
```
 ![Total Minutes Asleep vs  Total Time in Bed](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/33ce02ec-8549-4705-8507-65e88d9d15c4)   

There is a positive correlation between Total Minutes Asleep and Total Time in Bed, and it appears to be linear. To enhance the sleep quality of users, Bellabeat should consider incorporating a feature where users can customize their sleep schedule and utilize notifications to remind them to go to sleep.
```{r}
# Sleep Duration and Sedentary Time
ggplot(data = merged_df, mapping = aes(x = SedentaryMinutes, y = TotalMinutesAsleep)) + 
  geom_point() + labs(title= "Sleep Duration and Sedentary Time")
```
 ![Sleep Duration and Sedentary Time](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/75699236-1382-4fe3-bf34-7afe77f61abf)   
```{r}
cor(merged_df$TotalMinutesAsleep,merged_df$SedentaryMinutes)
```
![covariance](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/a3959234-9724-4156-9d43-74f1940d20e4)   
The correlation result between **Sleep Duration** and **Sedentary Time** in the graph above indicates a negative correlation between these two variables. This implies that the less active a participant is, the less sleep they tend to get.
```{r}
# Minutes Asleep vs. Sedentary Minutes

ggplot(data=merged_df, aes(x=TotalMinutesAsleep, y=SedentaryMinutes)) + 
  geom_point(color='black') + geom_smooth() +
  labs(title="Minutes Asleep vs. Sedentary Minutes")
```
![Minutes Asleep vs  Sedentary Minutes](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/5c1fee85-e305-4c1a-8789-cac58afd9ad1)   
The chart illustrates a negative relationship between Sedentary Minutes and Sleep Time. This suggests that users should reduce their sedentary time to improve their sleep.
```{r}
# Average Total Intensity vs. Time

# First, making a new data set called "intensities_grouped"
# Then group the users by average total intensity

intensities_grouped <- intensities %>%
  group_by(time) %>%
  drop_na() %>%
  summarise(avg_intensity = mean(TotalIntensity))

ggplot(data=intensities_grouped, aes(x=time, y=avg_intensity)) + geom_histogram(stat = "identity", fill='blue') +
  theme(axis.text.x = element_text(angle = 90)) +
  labs(title="Average Total Intensity vs. Time")
```
![Average Total Intensity vs  Time](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/b04428f0-db96-409a-bff4-bda0ca92688e)   
This bar graph shows that users start their activity at 5 am, with the peak of activity occurring between 17 and 19.
```{r}
# Steps count for each days of week

# Making a new data set from "merged_df" to separate day of the week

day_week <- mutate(merged_df, day = wday(SleepDay, label = TRUE))

# Making a new data set to group days of the week by average of total steps
avg_steps <- day_week %>% 
  group_by(day) %>% 
  summarise(avg_daily_steps = mean(TotalSteps))

ggplot(data = avg_steps, mapping = aes(x = day, y = avg_daily_steps)) +
  geom_col(fill = "darkblue") + labs(title = "Daily Step Count")
```
![Daily step counts](https://github.com/ImanBrjn/bellabeat_Rstudio/assets/140934258/f37a78e6-e4ac-45e7-8ea7-d8aca0b6f7a7)   
The bar graph indicates that users are most active on Saturdays and least active on Sundays. Since Sundays are a day off as well, it is expected that users have more time for their activities. Therefore, Bellabeat can encourage them to engage in activities on Sundays by sending notifications.
### 5- Share
For this step of the analysis, I created this report using **R Markdown**.

### 6- Act
After processing, analyzing, and sharing the insights, it's time to provide recommendations for Bellabeat. My recommendations are:    

1. Bellabeat should encourage and remind users to take at least 8000 steps per day.
2. Bellabeat could use motion sensors in their devices to alert users if they stay sedentary for too long.
3. Bellabeat could create schedules for users to get 8 hours of sleep every day. They should also consider incorporating a feature where users can customize their sleep schedule and utilize notifications to remind them to go to sleep.
4. As the analysis showed that if users become more sedentary, their sleep quality drops, the company could provide motivation to improve users' activity for better sleep quality.
5. Bellabeat can encourage users to engage in activities on Sundays by sending notifications.

## Conclusion
In this project, I collaborated with Bellabeat, a high-tech manufacturer of health-focused products for women. I utilized R-Studio for data preparation, cleaning, conducted analysis, and  data visualization. The goal was to focus on one of Bellabeat's products and analyze smart device data to gain insight into how consumers are using their smart devices. I presented recommendations to the team to help guide the marketing strategy for the company.   
   
**Thank you for taking the time to read my Capstone Project!**
