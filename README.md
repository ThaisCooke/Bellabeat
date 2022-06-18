# Bellabeat
Capstone Project

THIS IS THE CAPSTONE PROJECT FOR GOOGLE DATA ANALYTICS CERTIFICATE. I CHOSE THE SECOND OPTION, THE BELLABEAT ANALYSIS.

-- I used Excel Spreadsheets for cleaning, SQL for organizing the data and Tableau for Visualization. 

-- I used two csv files: sleepDay_merged and DailyActivity_merged (which contained Total Steps, Distance, Calories and Amount of Activity)

-- Excel Spreadsheets was used for removing duplicates, filtering and sorting the data.

-- For the Daily Steps data, there were some days that recorded steps as 0. This can be due to the user entering the data manually or forgetting to use the device.
Those were removed from the analysis. 





-- I did some Data Exploration in SQL. I wanted to find out how many users were on the Daily Activity table, which gave me the result of 33 distinct users:


SELECT 
COUNT(DISTINCT Id) AS Number_of_users
	FROM dailyactivity;
  
  
-- I did the same for Sleep Day table, which gave me the result of 24 distinct users:


SELECT 
COUNT(DISTINCT Id) AS Number_of_users
	FROM sleepDay;

-- (That was a problem, since it is not enough sample)



-- Adding Day of the week column on Daily Activity:

Alter Table dailyActivity
ADD day_of_week nvarchar(50)

Update dailyActivity
SET day_of_week = DATENAME(DW, ActivityDate)



-- Adding Day of the week column on Sleep Day:

Alter Table sleepDay
ADD day_of_week nvarchar(50)

Update sleepDay
SET day_of_week = DATENAME(DW, SleepDay)



-- For finding activity level per participant (percentage):
	SELECT 
  SUM(VeryActiveMinutes) / SUM(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes + SedentaryMinutes) * 100 AS VeryActivePerc,
  SUM(FairlyActiveMinutes) / SUM(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes + SedentaryMinutes) * 100 AS FairlyPerc,
  SUM(LightlyActiveMinutes) / SUM(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes + SedentaryMinutes) * 100 AS LightlyActivePerc,
  SUM(SedentaryMinutes) / SUM(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes + SedentaryMinutes) * 100 AS SedentaryPerc
FROM DailyActivity




-- Which day of the week participants are more active:

SELECT AVG(VeryActiveMinutes) AS VeryActiveMinutes, 
AVG(FairlyActiveMinutes) AS FairlyActiveMinutes, AVG(LightlyActiveMinutes) AS LightActiveMinutes, AVG(SedentaryMinutes) AS SedentaryMinutes, day_of_week
FROM dailyActivity
WHERE VeryActiveDistance+ModeratelyActiveDistance+LightActiveDistance <> 0 AND VeryActiveMinutes+FairlyActiveMinutes+LightlyActiveMinutes <> 0
GROUP BY day_of_week




-- Average number of steps on each day of the week:

Select AVG(TotalSteps) as total_steps,
AVG(TotalDistance) as total_dist,
AVG(Calories) as total_calories,
day_of_week
From dailyActivity
Group By  day_of_week




-- Relation Between Steps and Calories Burned:

  SELECT Id, AVG(TotalSteps) AS Total_Steps, AVG(Calories) AS Total_Calories
  FROM dailyActivity
  GROUP BY Id




-- Correlation between sleeping and number of calories burned using inner join:

  SELECT DailyActivity.Id, AVG (sleepDay.TotalMinutesAsleep) AS TotalMinutesAsleep, AVG (dailyActivity.calories) AS TotalCalories
	FROM DailyActivity
	INNER JOIN SleepDay 
	on dailyActivity.Id = sleepDay.Id 
	GROUP BY dailyActivity.Id




-- Average sleeping per day of the week:

Select AVG(TotalMinutesAsleep) as total_sleep,
day_of_week
From sleepDay
Group By  day_of_week


-- Total Time wasted awake in bed:

Select  Avg(TotalMinutesAsleep) as avg_sleep_time,
Avg(TotalTimeInBed) as avg_time_bed,
AVG(TotalTimeInBed - TotalMinutesAsleep) as wasted_bed_time, day_of_week
from sleepDay
Group by day_of_week

 
 
