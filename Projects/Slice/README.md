# Slice



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
Slice is a small take-away-only business. The owner sought to gain insights from the data generated by his business.

#### Business Task
Develop a dashboard offering insights into key performance indicators (KPIs), monthly sales, visualization of the weekly order distribution, and an analysis of orders within half-hour increments. 
Additionally, analyze and visualize the impact of rainfall on order patterns.

### Data Sources
- Sales data was taken from the Sitedish API. The data ran from 2023-01-01 to 2023-10-31 and did not include Mondays. Data not publicly available.
- Weather(rain) data for the same time period was taken from https://www.meteoblue.com/nl/weer/historyclimate/weatherarchive/hoorn_nederland_2753638

### Tools
- Excel: data cleaning and transformation
- SQL (BigQuery): data analysis and transformation
- Tableau: visualization

### Data Cleaning and Transformation
- Checked for and removed duplicates
- Removed sales data entries that missed significant information.
-	Split time from the data and rounded down to the nearest 30 minutes, forming half-hour time blocks.
- Created new column for date, month and weekday (1=sun - 7=sat)
- Trimmed the weather dataset to include only information aligning with business hours.
- In a new column (Rain), created a dichotomous variable that read 'Yes' (If rain on said date is > 0.01mm) or 'No' (If else). So, in cases where rain occurred during business hours, the corresponding entry for that date in indicated 'yes.'


### Data Analysis (SQL)
- Exploratory analyses
- Join ON date to link the sale table to the rain table
- Query to SELECT COUNT orders and GROUP BY time block, date, weekday, and month

Exported the Query results and created a '0' dummy variable for each time block that had no sales.



### Results
- The most orders are on Sunday and Friday (24% and 22% respectively), followed by Saturday and Thursday (18% and 15% respectively), trailed by Wednesday and Tuesday (11% and 10% respectively).
-  
![Slice_Dash](https://github.com/Roenhoogland/Portfolio/blob/main/assets/images/Slice_Dash.png)


- Casual riders more often use the electric bike (57,85%) compared to members (52%). Additionally, casual riders (can) use the docked bike (6,31%) in contrast to members.
![Dashboard 1](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/070d1e19-6b86-44cf-a90b-d67a82fa48c7)



-	Casual riders most often ride in July (400k rides). In January, casual riders only use the bikes about 10% of that (40k rides). Members peak in August (427k rides), while in their lowest month, they still account for 31,9% (136k rides). As such we can conclude that members use the bike more consistently. Of course, that is somewhat expected as members pay for an annual subscription.
![Month (m_c)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/62f60741-dda5-4067-bc5d-6e6a92731fa4)


-	The average ride length of casual riders (25 min) is around twice as long as members (12 min). In addition. Casual riders tend to ride longer around (Monday and Friday) and during weekends compared to weekdays (Tuesday till Thursday). For members, this increase is only observed on weekends and is minimal - almost a 15% increase on Saturday compared to the weekday average.
![Ride time per day](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/41b91136-b278-44a9-ad6c-7fda69ba2295)




-In absolute terms, the seasonal increase in ride length is more for casual riders. However, in relative terms, the increase is similar (around 25%).
![Ride time per month](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/dc77abde-1d3c-48dc-a96a-fcea4387037e)



-	Compared to casual riders, members show a spike in bike usage around 7-8 a.m. This is observed in both June and December.
![Start Time June (1)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/1009de7c-8f64-409b-a234-713c53c96411)
![Start Time December](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/0c2c5024-1b01-4e2e-b129-8bf1fec4a6fa)



  
-	In general, members account for more rides than casual riders, so it’s expected to see more member rides on the geographical map. However, they seem to concentrate more around the Chicago Loop, Greektown, Magnificent Mile areas of downtown Chicago, such as Ogilvie Transportation Center and Chicago Union Station. In addition, members concentrate around universities, colleges, and medical centers (e.g. University of Chicago, University of Illinois at Chicago, Illinois Medical District).
![SS_M (1)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/c28d4ace-7c34-4120-8357-aaea0f9b333e)
![SS_C (1)](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/47b264b5-f025-4d49-a283-eb96b256a264)
Casual riders concentrate relatively more along the lake shore and parks.

![UofC conversion possibilities](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/96e6313b-e65f-4fb0-9ce5-7e50556e841a)


-	As expected, in most areas casual riders account for fewer rides than members. However, along the lake shore and around certain tourist areas/places casual riders consistently dominated.  Streeter Dr & Grand Ave accounts for the start of 52920 casual rides. The station that accounts for the most starts of members is Kingsbury St. & Kinzie St. with ‘only’ 25293 rides. Other examples around Millenium Park, Lincoln Park Zoo, and Shedd Aquarium
![Start station](https://github.com/Roenhoogland/Data-Analytics/assets/145770693/9eb7883c-e24f-471f-9602-484f865b566b)



### Recommendations
- Casual riders and members are different to a certain extent, so we can't expect all casuals to become members. If we do want them to become members, we have to cater more to them. Casuals tend to go to tourist places and ride in the summer season. One recommendation would be, to introduce a seasonal pass for casuals to ride all summer long. Additionally, conduct a survey among casual riders to understand their needs better.
  
- There are still a lot of casual rides made in business areas, around universities, hospitals, etc. I assume (without demographic data of the users) that a lot of the people who made these rides will come to the location more often. Thus, they could be stimulated to use the bike more. Promote cycling at universities and medical centers for example. Perhaps make deals with the institutions. Based on the casual ride numbers around some institutions this has potential.

### Limitations
- The ordering patterns in connection with rainfall data may exhibit bias, given that precipitation is more frequent in spring, autumn, and winter, particularly in the latter when days are shorter. Consequently, it seems plausible that customers place orders earlier during these seasons compared to the longer days of summer. To address this, it would be prudent to examine the monthly distribution of orders.
- In the current analysis, rain is dichotomously categorized as 'Yes' or 'No' based on whether it occurred during business hours on a given day. However, if the rainfall is minimal and occurs only towards the end of business hours, it is less likely to impact sales at the beginning of the business day. 