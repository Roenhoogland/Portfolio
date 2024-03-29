# Slice



## Table of Contents
- [Project Overview](#project-overview)
- [Business Task](#business-task)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning and Transformation](#data-cleaning-and-transformation)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Limitations](#limitations)



### Project Overview
Slice is a small take-away-only business. The owner sought to gain insights from the data generated by the business. And more specifically, re-evaluate business hours based on a half-hour order distribution.

#### Business Task
Develop a dashboard offering insights into key performance indicators (KPIs), monthly sales, visualization of the weekly order distribution, and an analysis of orders within half-hour increments. 
Additionally, analyze and visualize the impact of rainfall on order patterns.

### Data Sources
- Sales data was retrieved from the Sitedish API. The data ran from 2023-01-01 to 2023-10-31 and did not include Mondays (business closed). Data not publicly available.
- Weather(rain) data for the same period was retrieved from https://www.meteoblue.com/nl/weer/historyclimate/weatherarchive/hoorn_nederland_2753638

### Tools
- Excel: data cleaning and transformation
- SQL (BigQuery): data analysis and transformation
- Tableau: visualization

### Data Cleaning and Transformation
- Checked for and removed duplicates
- Removed sales data entries that missed significant information.
-	Split time from the data and rounded down to the nearest 30 minutes, forming half-hour time blocks.
- Created new column for date, month, and weekday (1=sun - 7=sat)
- Trimmed the weather dataset to include only information aligning with business hours.
- In a new column (Rain), create a dichotomous variable that reads 'Yes' (If rain on said date is > 0.01mm) or 'No' (If else). So, in cases where rain occurred during business hours, the corresponding entry for that date indicated 'yes.'


### Data Analysis
SQL
- Uploaded Weather and Sales tables
- Exploratory analyses
- Join ON date Query to SELECT COUNT orders and GROUP BY time block, date, weekday, month, and rain from the tables

Excel
- Exported the Query results and created a '0' dummy variable for each time block with no sales.



### Results
- Gross income between Jan was Oct was roughly €127.000, from 3969 orders.
- Gross income was the highest in May (€16.084) and the lowest in August (€10.022)
- Most orders came on Sundays and Fridays (24% and 22% respectively), followed by Saturdays and Thursdays (18% and 15% respectively), trailed by Wednesdays and Tuesdays (11% and 10% respectively).
- On average, most orders came in between 17:30-18:00 for both rainy and dry days (3.34 and 3.18 respectively) and the least between 16:00-16:30, with an average of 0.85 on rainy days and 0,67 on dry days.
- On days with rain, the average number of orders was 15.7, representing a 7.5% increase compared to dry days with an average of 14.6 orders.
- On rainy days, there is a slight uptick in orders during the early business hours, particularly within the 16:30-17:00 and 17:00-17:30 time frames, with an average increase of around 0.35 orders.

Detailed graphs illustrating the impact of rain for each month were shared with the business owner. however, they are not included here to prevent the page from being cluttered with ten similar graphs. The overall trend of orders clustering in the early hours of business persisted.

![slicedashboard (1)](https://github.com/Roenhoogland/Portfolio/assets/145770693/74138a70-788e-44c9-91d4-d5e0724bdc79)
Click [here](https://public.tableau.com/views/SliceDash/Dashboard1?:language=en-GB&:display_count=n&:origin=viz_share_link) for a link to the interactive dashboard
- The majority of orders, totaling 91.74%, were for delivery, with Takeaway constituting the remaining 8.26%.
![sliceordertype](https://github.com/Roenhoogland/Portfolio/assets/145770693/cec66a74-054b-46ee-8d38-42136eb80346)
- Most orders were made through the company's official website, comprising 71.96% of the total. The Thuisbezorgd food order platform accounted for 23.53% of orders. A small portion, 3.4%, were placed in person at the physical location, while UberEats accounted for 1.11% of the orders.
![sliceplatform](https://github.com/Roenhoogland/Portfolio/assets/145770693/f9bf4301-c6bb-4127-81d5-737bb7865338)



### Limitations
- The ordering patterns in connection with rainfall data may exhibit bias, given that precipitation is more frequent in spring, autumn, and winter, particularly when days are shorter. Consequently, it seems plausible that customers place orders earlier during these seasons compared to the longer days of summer. To address this, examining the monthly distribution of orders would be prudent.
- In the current analysis, rain is dichotomously categorized as 'Yes' or 'No' based on whether it occurred during business hours on a given day. However, if the rainfall is minimal and occurs only towards the end of business hours, it is less likely to impact sales at the beginning of the business day. 
