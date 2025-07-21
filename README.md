# Uber Supply-Demand Gap Analysis

## Table of Contents

* [Project Overview](#project-overview)
* [Data Cleaning and Preparation](#data-cleaning-and-preparation)
* [Excel Dashboard Summary](#excel-dashboard-summary)
* [SQL Insights](#sql-insights)
* [Python EDA Insights](#python-eda-insights)
* [Data Sources](#data-sources)
* [Recommendations](#recommendations)

## Project Overview

This project investigates the Uber supply-demand gap using the data. Through a
combination of Excel dashboards, SQL queries, and Python-based EDA, we identify periods
of high demand and frequent trip failures. Our goal is to uncover patterns that explain user
dissatisfaction, driver availability, and canceled rides, and provide actionable insights.

## Data Cleaning and Preparation

The dataset was cleaned using Excel and Python Pandas.

Key steps:
- Converted inconsistent timestamp formats to datetime.
- Extracted date, hour, and weekday from request timestamps.
- Classified requests into time slots: Early Morning, Morning, Afternoon, Evening, Night.
- Calculated Trip Duration (only for completed rides).
- Created Status Category column to label trips as Success or Failure.
- Handled missing values in Drop timestamp by interpreting them as failed rides.

## Excel Dashboard Summary

### Chart 1: Status by Time Slot

This chart compares the count of each ride status across time slots. It highlights that most
trip failures (No Cars Available and Cancelled) occur in the Early Morning and Night. This
insight informs when driver supply needs to be increased.


<img width="585" height="316" alt="Status Vs Time Slot" src="https://github.com/user-attachments/assets/df067f94-8d19-41be-8468-92c6816155e7" />

### Chart 2: Pickup Point by Status

This chart shows how ride statuses vary across pickup points (Airport and City). City
pickups face more cancellations, while Airport pickups are often met with no availability —
especially at night.


<img width="583" height="317" alt="pickup" src="https://github.com/user-attachments/assets/9dbea080-4d65-40b1-9f04-dc86660f7dc8" />


### Chart 3: Hourly Request Distribution

Displays ride request volume across 24 hours. Clear spikes are observed between 5–9 AM
and 5–8 PM, suggesting peak periods where Uber should increase supply or apply surge
pricing.


<img width="631" height="332" alt="hourly" src="https://github.com/user-attachments/assets/5c375185-24e1-42b5-b417-7becc456df10" />

### Chart 4: Success Vs Failure

Shows the overall trip completion ratio. Only 39.6% of total requests are fulfilled,
emphasizing the urgent need to optimize driver availability and reduce cancellations.


<img width="583" height="319" alt="success" src="https://github.com/user-attachments/assets/1b033f69-f7c0-4cba-8de1-367e891676c4" />

## SQL Insights

SQL queries were used to extract high-level insights from structured Uber request data.

* _Total requests by status_

  
> SELECT Status, COUNT(*) AS Total_Requests

> FROM uber_requests

> GROUP BY Status;


**Output:**
- Trip Completed: 2831
- No Cars Available: 2650
- Cancelled: 1264


* _Requests by pickup point and status_


> SELECT Pickup_point, Status, COUNT(*) AS Count

> FROM uber_requests

> GROUP BY Pickup_point, Status;


**Output:**
- Airport - Cancelled: 198
- Airport - No Cars Available: 1713
- Airport - Trip Completed: 1327
- City - Cancelled: 1066
- City - No Cars Available: 937
- City - Trip Completed: 1504


* _Requests per hour_


> SELECT Request_Hour, COUNT(*) AS Request_Count

> FROM uber_requests

> GROUP BY Request_Hour

> ORDER BY Request_Hour;


**Output:**
- Hour 0: 99
- Hour 1: 85
- Hour 2: 99
- Hour 3: 92
- Hour 4: 203
- Hour 5: 445
- Hour 6: 398
- Hour 7: 406
- Hour 8: 423
- Hour 9: 431
- ...
- Hour 20: 492
- Hour 21: 449
- Hour 22: 304
- Hour 23: 194


* _Requests by time slot_


> SELECT Time_Slot, COUNT(*) AS Request_Count

> FROM uber_requests

> GROUP BY Time_Slot;


**Output:**
- Night: 1814
- Evening: 1560
- Early Morning: 1452
- Morning: 1268
- Afternoon: 651


* _Cancellations by pickup point_


> SELECT Pickup_point, COUNT(*) AS Cancelled_Requests

> FROM uber_requests

> WHERE Status = 'Cancelled'

> GROUP BY Pickup_point;


**Output:**
- Airport: 198 cancellations
- City: 1066 cancellations


* _No cars available by hour_


> SELECT Request_Hour, COUNT(*) AS No_Car_Requests

> FROM uber_requests

> WHERE Status = 'No Cars Available'

> GROUP BY Request_Hour;


**Output:**
- Hour 0: 56
- Hour 1: 56
- Hour 2: 57
- Hour 3: 56
- Hour 17: 232
- Hour 18: 322
- Hour 19: 283
- Hour 20: 290
- Hour 21: 265


* _Success vs Failure percentage_


> SELECT Status_Category, COUNT(*) * 100.0 / (SELECT COUNT(*) FROM uber_requests) AS
Percentage

> FROM uber_requests

> GROUP BY Status_Category;


**Output:**
- Success: 41.97%
- Failure: 58.03%


* _Average trip duration (only successful trips)_


> SELECT ROUND(AVG("Trip Duration (min)"), 2) AS Avg_Duration_Minutes

> FROM uber_requests

> WHERE Status = 'Trip Completed';


**Output:**
52.41 minutes


* _Daily trip request volume_


> SELECT Request_Date, COUNT(*) AS Daily_Requests

> FROM uber_requests

> GROUP BY Request_Date

> ORDER BY Request_Date;


**Output:**
- 2016-07-11: 1367 requests
- 2016-07-12: 1307 requests
- 2016-07-13: 1337 requests
- 2016-07-14: 1353 requests
- 2016-07-15: 1381 requests


* _Day-wise request patterns_


> SELECT Request_Day, COUNT(*) AS Request_Count

> FROM uber_requests

> GROUP BY Request_Day

> ORDER BY FIELD(Request_Day,
'Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday');


**Output:**
- Monday: 1367
- Tuesday: 1307
- Wednesday: 1337
- Thursday: 1353
- Friday: 1381
- Saturday: No data
- Sunday: No data

### SQL Insights Table

| Query | Explanation | Insights |
| :---:  |    :---:    |   :---:   |
| Total Requests by Status | Count of all trip outcomes | Majority of rides are unfullfiled: only ~39% success |
| Requests by Pickup Point and Status | Grouped by location (City/Airport) | City has more cancellations, Airport has more unavailability |
| Requests per Hour | Rides grouped by Request Hour | High demand between 5-9 AM and 6-8 PM |
| Requests per Time Slot | Time Slot vs Request Count | Early morning and Evening are busiest |
| Cancellations by Pickup Point | Where cancellations are the highest | City pickups dominate cancellations |
| No cars available by hour | Where Uber couldn't assign a ride | Mostly after 8PM at Night |
| Success vs Failure Percentage | Share of successful vs failed trips | Only 39.6% trips are successful |
| Average Trip Duration | For only completed rides | Average duration: ~52 minutes |
| Daily Trip Volume | Rides Per Day | Reveals consistent demand throught the week |
| Request Day Patterns | Weekday Analysis | Higher failures on weekdays during peak hours |


These insights help identify operational stress points and opportunities to optimize supply.

## Python EDA Insights

Python's Pandas was used for deep exploratory data analysis (EDA)
[Google Collab Repository](https://colab.research.google.com/drive/1mdz7vBz1Ex22Ep5g5iaJKORomkkXXp0E)

Used both Univariate Analysis and Bivariate Analysis.

* Univariate Analysis
  * Ride Status Count
  * Time Slot Frequency

* Bivariate Analysis
  * Pickup Point vs Ride Status
  * Hourly Request Trend

* Summary Chart

These visualizations validate Excel and SQL findings and provided data-driven support for our recommendations.

## Data Sources

Uber Services Data: The primary dataset used for this analysis is the "uber_raw_data.csv" file, containing detailed information about each service.

### Tools

* Excel - Data Cleaning + Creating reports
  * [Download Cleaned Data](https://drive.google.com/file/d/1tM11qrTBNGoyL2Y8IFMpijxlLFizS4XP/view)
* SQL Server - Data Analysis
* Pandas - Visualization 

## Recommendations

- Provide night shift incentives to reduce 'No Cars Available' errors.
- Penalize frequent driver cancellations in morning slots.
- Assign special airport fleet during peak hours.
- Expand driver base or enable automated rescheduling.
- Use surge pricing based on historical hourly demand.
