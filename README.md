# Cyclistic Data Analysis (Capstone Case Study)

Data Analysis for a hypothetical Bike-share company, Cyclistic.
> Time of Analysis : December 2022

> Tools used : **MS Excel**, **Tableau**

> ## Note :
> R programming language also could've been used to conduct the following analysis. But my device was struggling with R Studio especially when I merged all 12 files into one. Hence I decided not to use R and went ahead with Excel & Tableau. Although, I've performed the analysis on a single month to show how we can do this with R also. It can be accessed [here](https://github.com/vnk-pathak/Cyclistic-Data-Analysis/tree/main/R%20Script).

----

### Background

I am a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes that the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, my team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve my recommendations, so they must be backed up with compelling data insights and visualizations.

----

## Business Task

**Analyse Rider's bike usage pattern to aid in designing marketing campaign for membership conversion.**

----

## Data

### Source

The data for this analysis has been sourced from [this link](https://divvy-tripdata.s3.amazonaws.com/index.html).
The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement).

### Cleaning and Manipulation

Provided data is categorised monthly. A total of 12 files were downloaded for each month from December 2021 to November 2022. <br>
All the files were in **.csv** format. <br>
**MS Excel 2010** was used for basic cleaning and formatting of the data.

The following steps were taken to clean & reformat the raw data :

- The CSV file was imported into MS Excel.
- Columns which were obsolete for this analysis were deleted. Following columns were deleted from the dataset-
  - start_station_id, end_station_id (*They are alphanumeric codes assigned to each docking station. Since the names of the stations are already present in the same file, they serve no purpose.*)
  - start_station_name, end_station_name
    > Station Names could have been useful to find busiest stations, top routes or any other metric along the lines of that. But, about 15% of the start station names and about 16% of end station names were missing which is a significant chunk for any dataset. This could very likely skew the result of any analysis we perform using this info as among the available entries with station names, busiest stations were hardly 1% of the total traffic. Also, there was no alternative way to actually retrieve the missing names. Hence, they too were dropped from the dataset.
  - start_lat, start_lng, end_lat, end_lng (*They are geographical co-ordinates for each docking station. They could've been used to visualize locations where the rides started or ended, but in absence of majority of station names, their relevance is not much effective for this analysis.*)
  
- The remaining columns were : **ride_id** (A unique alphanumeric code assigned to each ride), **rideable_type** (Type of bike used for that ride, Classic Bike/Docked Bike/Electric Bike), **started_at** (Date and Time when the ride starts), **ended_at** (Date and Time when the ride ends) and **member_casual** (Type of Rider, Casual Rider/Member).

- The data format of **started_at** and **ended_at** columns was 'General'. It was changed to 'dd-mm-yyyy hh:mm' using custom formatting.

- Following columns were added
  - **ride_duration** - Purpose of this column is to find the duration of each trip. It was calculated by subtracting the value of **started_at** column from the value of **ended_at** column.
  - **day_of_week** - Purpose of this column is to identify the day of the week when the ride starts. It was calculated using the following formula on **started_at** column - <br>
  **=WEEKDAY(D2, 2)**
    > It returns an integer where 1 denotes Monday and and 7 denotes Sunday.

This concludes the basic cleaning and manipulation of the data. The files were then saved for further analysis.

----

## Analysis

Basic analysis was conducted in **MS Excel** using Pivot Tables. <br>

A Pivot Table was created for each month to find out the number of trips taken by each type of rider along average ride duration, maximum ride duration and minimum ride duration along each day of the week. Following insights were gained by this-
- Members had a greater share of trips compared to Casual Riders.
- Average ride duration of Casual Riders was nearly double compared to Members.
- More rides are taken on Weekends compared to Weekdays, especially for Casual Riders.
- Maximum ride duration in each month was way more than a usual trip.
  - For Members, maximum ride duration in each month was around 25 hours.
  - For Casual Riders however, this value was over 180 hours for each month. It was around 604 hours in May 2022 and around 689 hours in October 2022.
  > Now, a full working day which starts at early morning and ends in late night would be of 1000 minutes (16 hours and 40 minutes) max. The number of trips which exceeded this limit was about 0.1% of the whole dataset. This, though a insignificant proportion, could skew the the results of the analysis because of their significantly large magnitude. Hence, they would be treated as Outliers.
- Minimum Ride Duration was Zero for some of the months, but it was negative for month of March and May to November.
  > A trip with negative ride duration is impossible. This could be a cause of some error in data collection or recording. Also, a trip with a ride duration of Zero seconds doesn't make any sense as it cannot be monetised. This could also be a cause of either some error in data collection or some mistakes from customer side where they accidently activated and ended a ride. The number of such trips was 0.009% of the whole data set. Since there was no other way to rectify these records, they are removed from the dataset.

All the tables were then combined into a single file for further analysis.

--

After the preliminary analysis, following initial hypothesis is formed -

**Hypothesis : Members mostly comprises of working people who are using bikes to commute to their workplace. Casual Riders are mostly comprised of students, casual strollers or visitors who use bikes for leisure riding or ocassional sightseeing.**

--

**Tableau** has been used for following analysis.

Following Calculative Fields were added using **Analysis > Create Calculated Field** option from the Worksheet Menu Bar.

1. **Ride Duration (Minutes)** | *(It was added because Tableau was not properly converting the values of **ride_duration** column in minutes.)*
   > DATEDIFF('minute', [started_at], [ended_at], 'monday')
   
2. **Day Of Week** | *(It was added to assign names to the days of the week since they are in numerical format.)*
   > IF [day_of_week] == 1 THEN 'Monday' ELSEIF [day_of_week] == 2 THEN 'Tuesday' ELSEIF [day_of_week] == 3 THEN 'Wednesday' ELSEIF [day_of_week] == 4 THEN 'Thursday' ELSEIF [day_of_week] == 5 THEN 'Friday' ELSEIF [day_of_week] == 6 THEN 'Saturday' ELSEIF [day_of_week] == 7 THEN 'Sunday' END

3. **Month & Year** | *(It was created to assign month name for each trip, it would be useful for month by month analysis of various metrics.)*
   > IF MONTH([started_at]) == 1 THEN 'January 2022' ELSEIF MONTH([started_at]) == 2 THEN 'February 2022' ELSEIF MONTH([started_at]) == 3 THEN 'March 2022' ELSEIF MONTH([started_at]) == 4 THEN 'April 2022' ELSEIF MONTH([started_at]) == 5 THEN 'May 2022' ELSEIF MONTH([started_at]) == 6 THEN 'June 2022' ELSEIF MONTH([started_at]) == 7 THEN 'July 2022' ELSEIF MONTH([started_at]) == 8 THEN 'August 2022' ELSEIF MONTH([started_at]) == 9 THEN 'September 2022' ELSEIF MONTH([started_at]) == 10 THEN 'October 2022' ELSEIF MONTH([started_at]) == 11 THEN 'November 2022' ELSEIF MONTH([started_at]) == 12 THEN 'December 2021' END

4. **Season** | *(It was created to see a broad view of various metrics across seasons.)*
   > IF [Month & Year] == 'December 2021' OR [Month & Year] == 'January 2022' OR [Month & Year] == 'February 2022' THEN 'Winter' ELSEIF [Month & Year] == 'March 2022' OR [Month & Year] == 'April 2022' OR [Month & Year] == 'May 2022' THEN 'Spring' ELSEIF [Month & Year] == 'June 2022' OR [Month & Year] == 'July 2022' OR [Month & Year] == 'August 2022' THEN 'Summer' ELSEIF [Month & Year] == 'September 2022' OR [Month & Year] == 'October 2022' OR [Month & Year] == 'November 2022' THEN 'Fall' END

Now, various charts are created to visualize number of rides taken by different types of riders, their average ride duration and change in these metrics along days of the week, months and seasons. These metrics were also analysed for different types of bikes to understand rider's preference. These visualizations would be helpful in better understanding ridership patterns and behaviour.

----

## Visualisation & Key Findings

### Visualizations

### 1. Member takes more rides than Casual Riders. About 60% of the rides are taken by Members.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208493496-750b368f-400f-4ebd-b24f-26390be4840a.png">
</kbd>

### 2. Average Ride Duration of Casual Riders was nearly 1.75 times to that of Members.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208458796-a4a467d4-2f21-418d-9b6f-7bc7feb27b22.png">
</kbd>

### 3. Members take more rides on Weekdays with a nearly consistent average ride duration across the week, whereas Casual Riders prefer Weekends for rides which is reflected by both number of rides taken and their average ride duration.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208497514-a7998789-3ae2-4a6f-8bbe-cfbc3747a06e.png">
</kbd>

### 4. Members mostly start their rides in morning around 8 AM and in the evening around 5 PM with a gradual increase and decrease in number of rides between these two peaks. Casual Riders however, mostly prefer the evening time to start their ride, with peak figure at 5 PM.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208458663-132ea9ea-ceee-4a3f-9022-bb4807d4f29b.png">
</kbd>

### 5. Riders, irrespective of their type, prefer warmer months for a bike ride. About 64% of the rides were taken in the months of March to August.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208508148-862f5f4b-f7fa-4cbd-905e-0dcd68c2cf54.png">
</kbd>

### 6. Casual Riders take longer rides in the warmer months having their average ride duration more than 20 minutes for the months of March to August. Members, however, have a nearly cosistent average ride duration throughout the year.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208508962-2ca52d56-7f0c-4ece-bdb6-896350ae2419.png">
</kbd>

### 7. Casual Riders prefer Electric Bikes over Classic Bikes. Members, however, use both types of bikes almost equally. Docked Bikes, though in small numbers, used only by Casual Riders.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208458952-8fb7ec21-188b-4862-bdcf-11d2d7763413.png">
</kbd>

### 8. Members have a near consistent average ride duration for both types of bikes. For Casual Riders, however, average ride duration of Classic Bikes was about 1.5 times to that of Electric Bikes. Docked Bikes have the highest average ride duration, nearly twice to that of the Classic Bikes.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208458988-1c2b7fe6-85be-442a-a36d-178b8120cfe9.png">
</kbd>

### 9. Majority of the Outliers (about 85%) are Casual Riders.Also, among the outliers, Average Ride Duration of Casual Riders was about 2.3 times to that of Members.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208515372-53351149-d01d-4334-9105-051e49a6420e.png">
</kbd>

### 10. Almost all but one Members and about 72.5% of Casual Riders have a ride duration of upto 1500 minutes or 25 hours. Only one Member has a ride duration of 1560 minutes. Maximum ride duration is 41387 minutes or 28.75 days.
<kbd>
  <img src="https://user-images.githubusercontent.com/91075582/208459089-0887dd37-1989-4326-95b6-7278be71dc15.png">
</kbd>

--

### Key Findings

1. Majority of rides are taken by Annual Members, but Casual Riders take longer trips *(As shown by [viz 1](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#1-member-takes-more-rides-than-casual-riders-about-60-of-the-rides-are-taken-by-members) and [viz 2](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#2-average-ride-duration-of-casual-riders-was-nearly-175-times-to-that-of-members))*.

2. Members are mostly working people who use bikes to commute to their workplace. This can be directly inferred from the following -
   - They have a nearly consistent average ride duration *(As shown by [viz 3](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#3-members-take-more-rides-on-weekdays-with-a-nearly-consistent-average-ride-duration-across-the-week-whereas-casual-riders-prefer-weekends-for-rides-which-is-reflected-by-both-number-of-rides-taken-and-their-average-ride-duration) and [viz 6](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#6-casual-riders-take-longer-rides-in-the-warmer-months-having-their-average-ride-duration-more-than-20-minutes-for-the-months-of-march-to-august-members-however-have-a-nearly-cosistent-average-ride-duration-throughout-the-year))*
   - They take more rides on weekdays *(As shown by [viz 3](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#3-members-take-more-rides-on-weekdays-with-a-nearly-consistent-average-ride-duration-across-the-week-whereas-casual-riders-prefer-weekends-for-rides-which-is-reflected-by-both-number-of-rides-taken-and-their-average-ride-duration))*
   - The peak times in a day when they usually start a ride are 8 AM (when they are most probably going for office) & 5 PM (when they are most probably returning for home) *(As shown by [viz 4](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#4-members-mostly-start-their-rides-in-morning-around-8-am-and-in-the-evening-around-5-pm-with-a-gradual-increase-and-decrease-in-number-of-rides-between-these-two-peaks-casual-riders-however-mostly-prefer-the-evening-time-to-start-their-ride-with-peak-figure-at-5-pm))*.
   - They are not using Docked Bikes *(As shown by [viz 7](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#7-casual-riders-prefer-electric-bikes-over-classic-bikes-members-however-use-both-types-of-bikes-almost-equally-docked-bikes-though-in-small-numbers-used-only-by-casual-riders))*, as docked bikes are required to be parked at docking stations which may not be convinient for them.
     > This requirement for the Docked bikes could very well be the reason for their high average ride duration *(As shown by [viz 8](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#8-members-have-a-near-consistent-average-ride-duration-for-both-types-of-bikes-for-casual-riders-however-average-ride-duration-of-classic-bikes-was-about-15-times-to-that-of-electric-bikes-docked-bikes-have-the-highest-average-ride-duration-nearly-twice-to-that-of-the-classic-bikes))*.

3. Casual Riders are mostly students or visitors who uses bikes for casual strolling. This can be directly inferred from the following -
   - They have higher average ride duration on weekends *(As shown by [viz 3](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#3-members-take-more-rides-on-weekdays-with-a-nearly-consistent-average-ride-duration-across-the-week-whereas-casual-riders-prefer-weekends-for-rides-which-is-reflected-by-both-number-of-rides-taken-and-their-average-ride-duration))*
   - They take more rides on weekends *(As shown by [viz 3](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#3-members-take-more-rides-on-weekdays-with-a-nearly-consistent-average-ride-duration-across-the-week-whereas-casual-riders-prefer-weekends-for-rides-which-is-reflected-by-both-number-of-rides-taken-and-their-average-ride-duration))*
   - They start most of their rides in the evening with peak time being 5 PM *(As shown by [viz 4](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#4-members-mostly-start-their-rides-in-morning-around-8-am-and-in-the-evening-around-5-pm-with-a-gradual-increase-and-decrease-in-number-of-rides-between-these-two-peaks-casual-riders-however-mostly-prefer-the-evening-time-to-start-their-ride-with-peak-figure-at-5-pm))*
   - They have higher average ride duration in the warmer months *(As shown by [viz 6](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#6-casual-riders-take-longer-rides-in-the-warmer-months-having-their-average-ride-duration-more-than-20-minutes-for-the-months-of-march-to-august-members-however-have-a-nearly-cosistent-average-ride-duration-throughout-the-year))*, this could be due to the Spring and Summer breaks that students get and also it is the peak time for tourists.
   - They prefer Electric Bikes over Classic Bikes *(As shown by [viz 7](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#7-casual-riders-prefer-electric-bikes-over-classic-bikes-members-however-use-both-types-of-bikes-almost-equally-docked-bikes-though-in-small-numbers-used-only-by-casual-riders))*, as it is more effortless in riding.
   - Some casual riders use Docked Bikes *(As shown by [viz 7](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#7-casual-riders-prefer-electric-bikes-over-classic-bikes-members-however-use-both-types-of-bikes-almost-equally-docked-bikes-though-in-small-numbers-used-only-by-casual-riders))*. They could be tourists, as docking stations are generally near some tourist hotspots.

4. Spring and Summer season attracts more riders, irrespective of their type *(As shown by [viz 5](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#5-riders-irrespective-of-their-type-prefer-warmer-months-for-a-bike-ride-about-64-of-the-rides-were-taken-in-the-months-of-march-to-august))* because people prefer more comfortable modes of travel as temperature drops.

5. Members who are Outliers, have an average ride duration of around 24 hours with maximum being 26 hours *(As shown by [viz 9](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#9-majority-of-the-outliers-about-85-are-casual-ridersalso-among-the-outliers-average-ride-duration-of-casual-riders-was-about-23-times-to-that-of-members) and [viz 10](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#10-almost-all-but-one-members-and-about-725-of-casual-riders-have-a-ride-duration-of-upto-1500-minutes-or-25-hours-only-one-member-has-a-ride-duration-of-1560-minutes-maximum-ride-duration-is-41387-minutes-or-2875-days))*. These are most likely people who took some other than usual route (maybe to a friend's house) or for a small adventure trip and decided to end the ride next day.
6. Casual Riders who are Outliers, have an average ride duration of 2.3 days with maximum being 28.7 days *(As shown by [viz 9](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#9-majority-of-the-outliers-about-85-are-casual-ridersalso-among-the-outliers-average-ride-duration-of-casual-riders-was-about-23-times-to-that-of-members) and [viz 10](https://github.com/vnk-pathak/Cyclistic-Data-Analysis#10-almost-all-but-one-members-and-about-725-of-casual-riders-have-a-ride-duration-of-upto-1500-minutes-or-25-hours-only-one-member-has-a-ride-duration-of-1560-minutes-maximum-ride-duration-is-41387-minutes-or-2875-days))*. These are most likely tourists who bought weekly or monthly passes and used a single bike throughout.


--

### The above visualizations & findings puts a strong evidence in favour of the initial hypothesis. Hence our Hypothesis is confirmed.

----

## Recommendations

1. Since the aim is to convert Casual Riders into Members, it is best to target Schools & Colleges as these places have those of our Casual Riders who are most likely to convert. Students can be targetted with campaigns which shows cost benefit of using bikes for daily commute compared to other modes of travel.
2. Casual Riders who use these bikes on a kind of regular basis and still not converting to annual members, is most likely due to a large one time payment to buy the annual membership. They could be offered a low interest or a no cost part payment plan to reduce the burden of one time payment.
3. Spring and Summer seasons have most of the ride traffic. This is the best time to heavily market the product and provide special discounts to Casual Riders who chose to get an annual membership.
----
