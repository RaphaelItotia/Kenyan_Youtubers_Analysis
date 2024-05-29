# Top_Kenyan_Youtubers_by_29-05-2024

## Table of contents
-	[Objective](#objective)
- [Data Source](#data-source)
-	[Steps](#steps)
-	[Design](#design)
    -	[Dashboard Blueprint](#dashboard-blueprint)
    -	[Tools](#tools)
-	[Development](#development)
    -	[Data Exploration](#data-exploration)
    -	[Data Cleaning](#data-cleaning)
    -	[Transform the Data](#transform-the-data)
    -	[Create the SQL View](#create-the-sql-view)
-	[Testing](#testing)
-	[Visualization](#visualization)
    -	[Outcomes](#outcomes)
    -	[DAX Measures](#dax-measures)
-	[Analysis](#analysis)
    -	[Outcomes](#outcomes)
    -	[Validation](#validation)
    -	[Discovery](#discovery)
-	[Recommendations](#recommendations)
    -	[Potential ROI](#potential-roi)
    -	[Potential Courses of Actions](#potential-courses-of-actions)
-	[Conclusion](#conclusion)
### Objective
The Head of Marketing seeks to identify the optimal Kenyan YouTubers for conducting marketing campaigns over the marketing period.

To create a dashboard that provides insights, we need a dataset of top Kenyan YouTubers in 2024 that includes
- Number of subscribers
- Total views
- Total videos
- Engagement metrics

#### User story
As the Head of Marketing, I want to use a dashboard that analyzes YouTube channels’ data in Kenya.
This dashboard should allow me to identify the top performing channels based on metrics like subscriber base and average views.
With this information, I can make more data-driven decisions about which Youtubers are right to collaborate with, and therefore maximize Return On Investment (ROI).

### Data source
We need a dataset containing columns;
-	Channel name
-	Subscribers
-	Total views
-	Total uploads
  
The data is sourced from [Social Blade](https://socialblade.com/youtube/top/country/ke). 
### Steps
-	Design
-	Development
-	Testing
-	Analysis
#### Design
Dashboard components are determined by the following questions:
1.	Who are the top 10 YouTubers with the most subscribers?
2.	Which 3 channels have the most views?
3.	Which 3 channels have the highest average views per video?
4.	Which 3 channels have the highest views per subscriber ratio?
5.	Which 3 channels have the highest subscriber engagement rate per video uploaded?
6.	Which 3 channels have the most uploads?

##### Dashboard blueprint
Data visuals that may be appropriate include:
1. Table
2. Treemap
3. Scorecards
4. Horizontal bar chart
   
![]()

##### Tools
| Tool | Purpose |
| ---- | ------- |
|Excel | Exploring the data |
|SQL Server |	Cleaning, testing, and analyzing the data|
|Mokkup AI	|Designing the blueprint of the dashboard|
|Power BI|	Visualizing the data using interactive dashboards|
|GitHub	|Hosting the project documentation|

#### Development
##### Data exploration 
This is the step where you have an inspection of what is in the data, errors, inconsistencies, and bugs.
-	For our dataset,
    -	There are 4 columns that contain the data we need for this analysis.
    
##### Data cleaning
The objective is to enhance our dataset to guarantee its organization and readiness for analysis.

The cleaned dataset should meet the following criteria and constraints:
•	Each column should have data types that are suitable for its contents.
•	No column should contain null values, indicating complete data for all records.(Data quality)

Here is a tabular representation of the expected schema for the cleaned data:

|Column Name	|Data Type	|Nullable|
|-----------|-------|------|
|Channel_name|	VARCHAR|	NO|
|Total_subscribers|	INTEGER|	NO|
|Total_views	|INTEGER|	NO|
|Total_uploads|	INTEGER|	NO|

##### Transform the data
The dataset I used had not indicated the exact number of subscribers. So, I had to convert them as shown below
```sql
SELECT subs,
    CASE 
        WHEN subs LIKE '%K' THEN CAST(REPLACE(subs,'K','') AS FLOAT) * 1000
		WHEN subs LIKE '%M' THEN CAST(REPLACE(subs,'M','') AS FLOAT) * 1000000
		ELSE CAST(subs AS FLOAT)
    END AS converted_subs
FROM 
    dbo.My_extracted;
```
![sql view]()
##### Create the SQL view
```sql
CREATE view Kenyan_Youtubers AS 
SELECT 
	Channel,
	uploads AS Uploads,
	CASE 
        WHEN subs LIKE '%K' THEN CAST(REPLACE(subs,'K','') AS FLOAT) * 1000
		WHEN subs LIKE '%M' THEN CAST(REPLACE(subs,'M','') AS FLOAT) * 1000000
		ELSE CAST(subs AS FLOAT)
    END AS converted_subs,
	video_views AS Video_views
FROM dbo.My_extracted
```
##### Remove Duplicates
```sql
SELECT DISTINCT *
INTO Final_Kenyan_Youtubers
FROM dbo.Kenyan_Youtubers
```
##### Testing
###### Data type check
```sql
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'Final_Kenyan_Youtubers';
```

###### output
![]()

###### Duplicate count check
```sql
SELECT
    Channel,
    COUNT(*) AS duplicate_count
FROM
    Final_Kenyan_Youtubers


GROUP BY
    Channel


HAVING
    COUNT(*) > 1;
```
