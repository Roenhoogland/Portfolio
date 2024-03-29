# Cyclistic Analysis
Capstone project. Part of the Google Data Analytics Certificate.
Albeit a fictive company, the analysis is conducted with real data.

## Table of Contents
- [Project Overview](#project-overview)
- [Business Task](#business-task)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning and Transformation](#data-cleaning-and-transformation)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)
- [Limitations](#limitations)



### Project Overview
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

#### Business Task
Analyze how annual members and casual riders use Cyclistic bikes differently

### Data Sources

Data has been provided by Motivate International Inc., an existing bike-sharing company based in the United States. 
For this project, monthly data from 7-2022 until 06-2023 was used.

- [Data source](https://divvy-tripdata.s3.amazonaws.com/index.html)
- [License](https://divvybikes.com/data-license-agreement)

### Tools
- Excel: data cleaning and transformation
- SQL (BigQuery): data analysis
- Tableau: visualization

### Data Cleaning and Transformation
I cleaned every monthly file separately as they were too big to combine into one dataset without Excel crashing. Due to the high amount of cleaning needed and the limitations of SQL when it comes to manipulating certain data, all of this was done in Excel.

Example of the data file:


![Schermafbeelding 2023-10-12 123908](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/3ed0d45b-bd38-448b-a9e9-de252b9bb5a0)


Data cleaning and transformation:
-	Removing entries with a negative ride time
-	Monthly datasets with slightly different names for the same station have been aligned accordingly
-	Fill in missing coordinates data using VLOOKUP
-	Entries with start or end station names could not be filled using VLOOKUP as different stations sometimes had the same coordinates
-	Entries with the same station name but slightly different coordinates have been unified under one coordinate set
-	Creation of new columns:
1. Ride length (min): ride length has to be calculated in minutes for SQL to run queries with it
2. Start time
3. Start hour of ride (so rides could be grouped according to start time)
4. Day of the week
5. Month

The monthly datasets were merged into one table in SQL

### Data Analysis
Click [here](https://github.com/Roenhoogland/Portfolio/blob/main/Projects/Cyclistic/SQL/Cyclistic.sql) for all code used

Example of code:
```sql
SELECT
  AVG(ride_length) AS ride_time,
  month,
  day_of_week,
  rideable_type,
  member_casual
FROM
  `deft-apparatus-394509.Cyclistic.cyclistic_year`
GROUP BY
  month,
  day_of_week,
  rideable_type,
  member_casual
ORDER BY
  month,
  day_of_week,
  rideable_type,
  member_casual; 
```

```sql
SELECT
  COUNT(ride_id) AS location_count,
  start_station_name,
  member_casual,
  start_lat,
  start_lng,
FROM
  `deft-apparatus-394509.Cyclistic.cyclistic_year`
WHERE
  start_station_name IS NOT NULL
GROUP BY
  start_station_name,
  start_lat,
  start_lng,
  member_casual
ORDER BY
  location_count DESC
```

```sql
SELECT
  TIME(EXTRACT(hour
    FROM
      AVG(start_time - '0:0:0')), EXTRACT(minute
    FROM
      AVG(start_time - '0:0:0')), EXTRACT(second
    FROM
      AVG(start_time - '0:0:0'))) AS avg_start_time,
  member_casual
FROM
  `deft-apparatus-394509.Cyclistic.cyclistic_year`
GROUP BY
  member_casual
ORDER BY
  member_casual
```


The year dataset was too large to be exported. Queries of analyses were run individually. The results of the analyses were saved and exported. These exports were used in Tableau to create visualizations

Note: the coordinate data in SQL is missing a decimal point. This was fixed in the exported results doc before creating the geographical visualizations in Tableau.

### Results
- Casual members ride the most on the weekends, while members ride more often during weekdays.
![Day (m_c)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/b95c515d-ab6b-4c75-9023-a6457b899b3b)



- Casual riders more often use the electric bike (57,85%) compared to members (52%). Additionally, casual riders (can) use the docked bike (6,31%) in contrast to members.
![Dashboard 1](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/070d1e19-6b86-44cf-a90b-d67a82fa48c7)



-	Casual riders most often ride in July (400k rides). In January, casual riders used the bikes about 10% of that (40k rides). Members peak in August (427k rides), while in their lowest month, they still account for 31,9% (136k rides). As such we can conclude that members use the bike more consistently. Of course, that is somewhat expected as members pay for an annual subscription.
![Month (m_c)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/62f60741-dda5-4067-bc5d-6e6a92731fa4)


-	The average ride length of casual riders (25 min) is around twice as long as members (12 min). In addition. casual riders typically ride for more extended periods around weekends seeing a 40% increase on Sunday (31 min) in contrast to weekdays from Tuesday (22 min) to Thursday. For members, this increase is only observed on weekends and is minimal - almost a 15% increase on Saturday compared to the weekday average.
![Ride time per day](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/41b91136-b278-44a9-ad6c-7fda69ba2295)




- In absolute terms, the seasonal increase in ride length is more for casual riders. However, in relative terms, the increase is similar (around 25%).
![Ride time per month](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/dc77abde-1d3c-48dc-a96a-fcea4387037e)



-	Compared to casual riders, members show a spike in bike usage around 7-8 a.m. This is observed in both June and December.
![Start Time June (1)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/1009de7c-8f64-409b-a234-713c53c96411)
![Start Time December](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/0c2c5024-1b01-4e2e-b129-8bf1fec4a6fa)



  
-	In general, members account for more rides than casual riders, so it’s expected to see more member rides on the geographical map. However, they seem to concentrate more around the Chicago Loop, Greektown, Magnificent Mile areas of downtown Chicago, such as Ogilvie Transportation Center and Chicago Union Station. In addition, members concentrate around universities, colleges, and medical centers (e.g. University of Chicago, University of Illinois at Chicago, Illinois Medical District).
![SS_M (1)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/c28d4ace-7c34-4120-8357-aaea0f9b333e)
- Casual riders concentrate relatively more along the lake shore and parks.
![SS_C (1)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/47b264b5-f025-4d49-a283-eb96b256a264)

- While rides around universities are predominantly led by members, there is also a significant amount of casual rider activity. As such, this could be identified as an opportunity.
![UofC conversion possibilities](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/96e6313b-e65f-4fb0-9ce5-7e50556e841a)


-	As expected, in most areas casual riders account for fewer rides than members. However, along the lake shore and around certain tourist areas/places casual riders consistently dominated.  Streeter Dr & Grand Ave accounts for the start of 52920 casual rides. The station that accounts for the most starts of members is Kingsbury St. & Kinzie St. with ‘only’ 25293 rides. Other examples around Millenium Park, Lincoln Park Zoo, and Shedd Aquarium
![Start station](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/9eb7883c-e24f-471f-9602-484f865b566b)



### Recommendations
- Casual riders and members are different to a certain extent, so we can't expect all casuals to become members. If we do want them to become members, we have to cater more to them. Casuals tend to go to tourist places and ride in the summer season. One recommendation would be, to introduce a seasonal pass for casuals to ride all summer long. Additionally, conduct a survey among casual riders to understand their needs better.
  
- There are still a lot of casual rides made in business areas, around universities, hospitals, etc. I assume (without demographic data of the users) that a lot of the people who made these rides will come to the location more often, especially universities. Thus, they could be stimulated to use the bike more. Promote cycling at universities and medical centers for example. Perhaps make deals with the (educational) institutions. 

### Limitations
- Due to privacy issues, demographic information is not included in the analysis. However, conducting another analysis that includes demographic information could be of value. For example, it would allow us to see where the customers live, and how many rides they have taken (regardless of them being a casual rider or a member).
- About 30% of the rides could not be included in the geographical data analysis because station names and coordinates were missing. As such, no exact can be given regarding the absolute amount of rides made from a certain station.
- The analysis was conducted with limited background knowledge. For example, no information was provided on why casual riders could use docked bikes while members could not. 
