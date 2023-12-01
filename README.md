# Google Data Analytics Certificate Capstone Project

### Summary
I am a data analyst working in the marketing team at Cyclistic, a fictional bike-share company in Chicago. The director of marketing believes the company's future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, my team will design a new marketing strategy to conver casual riders into annual members. But first, Cyclistic executives must approve my recoomendations, so they must be backed up with compelling data insights and professional data visualizations.

### Stakeholders
* **Cyclistic** - A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share inclusive to people with disabiilties and riders who can't use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.
* **Lily Moreno** - The director of marketing my manager. Lily is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social medial, and other channels.
* **Marketing analytics team** - A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy.
* **Executive team** - The detail-oriented executive team will decide whether to approve the recommended marketing program.

### Key terms
* **Casual riders** - Customers who purchase single-ride or full-day passes are referred to as casual riders.
* **Cyclistic members** - Customers who purchase annual memberships are Cyclistic members.

### Data analysis process
* **Ask** - Clearly define the problem or question you are trying to solve with data. This involves understanding the context and objectives of the analysis, as well as identifying the stakeholders and their needs.
* **Prepare** - - Gather and prepare the data. This may involve collecting data from various sources, cleaning and transforming data, and ensuring that it is of sufficient quality for analysis.
* **Process** - - Explore and summarize data. This may involve using descriptive statistics, data visualization, and other techniques to identify patterns, trends, and relationships in the data.
* **Analyze** - The analysis phase involves using the insights gained from the processing phase to answer the original question or solve the problem. This may involve building predictive models, developing hypotheses, and conducting statistical tests.
* **Share** - The sharing pahse involves communicating the results of the analysis to stakeholders. This may involve creating reports, presentations, and dashboards to effectively convey the findings of the analysis.
* **Act** - The final phase involves taking action based on the insights gained from the analysis. This may involve making decisions, implementing changes, or developing new strategies.

### Goal
Lily has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analytics team needs to better understand how annual members and casual riders differe, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Lily and the team are interested in analytics the historical bike trip to identify trends.

### Ask
* How do annual members and casual riders use bikes differently?
* Why would causal riders buy annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?

### Prepare
* **Data source** - Cyclistic's historical data can be downloaded [here](https://divvy-tripdata.s3.amazonaws.com/index.html), made available by Motivate International inc under this [license](https://divvybikes.com/data-license-agreement) and by Google. I have chosen to use data from April 2020 through March 2021. This comes in a CSV format that can be processed in Excel, Google Sheets, or programming softwares such as R. While processing the data, I noticed that there are nulls or missing values in multiple attributes that has been removed during analysis and data visualization. Note that some of the files might be too large to process in Sheets or Excel. This is public data that one can use to explore bike rides from Cyclistic.

### Process
In this phase, I have performed data wrangling including renaming column values, deriving new ones from existing columns, and more. See below.
```
apr_2020 <- rename(apr_2020,
                     trip_id = ride_id,
                     bikeid = rideable_type,
                     start_time = started_at,
                     end_time = ended_at,
                     from_station_name = start_station_name,
                     from_station_id = start_station_id,
                     to_station_name = end_station_name,
                     to_station_id = end_station_id,
                     usertype = member_casual)

may_2020 <- rename(may_2020,
                     trip_id = ride_id,
                     bikeid = rideable_type,
                     start_time = started_at,
                     end_time = ended_at,
                     from_station_name = start_station_name,
                     from_station_id = start_station_id,
                     to_station_name = end_station_name,
                     to_station_id = end_station_id,
                     usertype = member_casual)

jun_2020 <- rename(jun_2020,
                     trip_id = ride_id,
                     bikeid = rideable_type,
                     start_time = started_at,
                     end_time = ended_at,
                     from_station_name = start_station_name,
                     from_station_id = start_station_id,
                     to_station_name = end_station_name,
                     to_station_id = end_station_id,
                     usertype = member_casual)

I've omitted the rest of the code but the same transformation was done in each data frame before they were bind together.
```

```
# inspect the data frames
str(apr_2020)
str(may_2020)
str(jun_2020)
str(jul_2020)
str(aug_2020)
str(sep_2020)
str(oct_2020)
str(nov_2020)
str(dec_2020)
str(jan_2021)
str(feb_2021)
str(mar_2021)
```

```
apr_2020 <- mutate(apr_2020, 
                   trip_id = as.character(trip_id),
                   bikeid = as.character(bikeid))

may_2020 <- mutate(may_2020, 
                   trip_id = as.character(trip_id),
                   bikeid = as.character(bikeid))

jun_2020 <- mutate(jun_2020, 
                   trip_id = as.character(trip_id),
                   bikeid = as.character(bikeid))

I've omitted the rest of the code but the same transformation was done in each data frame before they were bind together.
```

```
# combine all rows into one big data frame
all_trips <- bind_rows(apr_2020, may_2020, jun_2020, jul_2020, 
                       aug_2020, sep_2020, oct_2020, nov_2020, 
                       dec_2020, jan_2021, feb_2021, mar_2021)
```

```
colnames(all_trips) # list of column names
nrow(all_trips) # how many rows are in the data frame
dim(all_trips) # dimensions of the data frame
head(all_trips) # see the first six rows
tail(all_trips)
str(all_trips) # inspect the columns and data types
summary(all_trips) # statistical summary of data
```

```
all_trips$date <- as.Date(all_trips$start_time) # the default format is yyyy-mm-dd
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")

all_trips$ride_length <- difftime(all_trips$end_time, all_trips$start_time)
str(all_trips$ride_length)
is.factor(all_trips$ride_length)
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))
is.numeric(all_trips$ride_length)
```

```
# remove bad data including bikes that were taken out of docks for quality check
# or ride length was negative
all_trips_v2 <- all_trips[!(all_trips$from_station_name == "HQ QR" | all_trips$ride_length < 0),]
head(all_trips_v2)
```

### Analysis
In this phase, I performed calculations to identify trends and relationships and conducted descriptive analysis. Earlier, I mentioned that there were some nulls or empty values in multiple columns and those are removed whenever appropriate. R is my tool of choice because it's more efficient to manipulate and analyze data there compared to spreadsheets like Excel in this case.
```
table(all_trips_v2$usertype)
```
| casual | member |
| ------ | ------ |
| 1380623 | 1976445|

```
nrow(all_trips_v2)
3479196
```

```
all_trips_v2 %>% 
  summarise(mean = mean(all_trips_v2$ride_length, na.rm = TRUE),
            median = median(all_trips_v2$ride_length, na.rm = TRUE),
            max = max(all_trips_v2$ride_length, na.rm = TRUE),
            min = min(all_trips_v2$ride_length, na.rm = TRUE))

OR

summary(all_trips_v2$ride_length)
```
In seconds
| Min | 1st Quartile | Median | Mean | 3rd Quartile | Max | NAs |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | 483 | 884 | 1704 | 1616 | 3523202 | 122128 | 

```
aggregate(all_trips_v2$ride_length ~ all_trips_v2$usertype, FUN = mean)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$usertype, FUN = median)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$usertype, FUN = max)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$usertype, FUN = min)
```

| User type | Ride length in seconds | Statistic |
| --- | --- | --- |
| casual | 2749.376 | mean |
| member | 973.974 | mean | 
| casual | 1293 | median |
| member | 695 | median | 
| casual | 3341033 | max |
| member | 3523202 | max | 
| casual | 0 | min |
| member | 0 | min | 

```
aggregate(all_trips_v2$ride_length ~ all_trips_v2$usertype + all_trips_v2$day_of_week, FUN = mean)
```

| User type | Day | Ride length in seconds |
| --- | --- | --- |
| casual |	Sunday |	3094.3241	|
| member |	Sunday |	1103.4337	|
| casual |	Monday |	2756.5101	|
| member |	Monday |	927.1667	|
| casual |	Tuesday |	2480.4720	|
| member |	Tuesday |	914.0390	|
| casual |	Wednesday |	2471.3060 |	
| member |	Wednesday |	923.9081	|
| casual |	Thursday |	2632.8978	|
| member |	Thursday |	918.3857	|
| casual |	Friday |	2617.0818	|
| member |	Friday |	955.0886	|
| casual |	Saturday |	2860.8466 |	
| member |	Saturday |	1076.2081 |

```
# analyrize ridership data by type and weekday
all_trips_v2 %>%
  mutate(weekday = wday(start_time, label = TRUE)) %>% 
  group_by(usertype, weekday) %>% 
  summarise(number_of_rides = n(),
            average = mean(ride_length)) %>% 
  arrange(usertype, weekday)
```

| User type | Day | Number of rides | Average | 
| --- | --- | --- | --- |
| casual | Sun	| 254988	| 3094.3241	| 
| casual	| Mon	| 145701	| 2756.5101	| 
| casual	| Tue	| 139826	| 2480.4720	| 
| casual	| Wed	| 152370	| 2471.3060	| 
| casual	| Thu	| 160385	| 2632.8978	| 
| casual	| Fri	| 201552	| 2617.0818	| 
| casual	| Sat	| 325801	| 2860.8466	| 
| member	| Sun	| 255384	| 1103.4337	| 
| member	| Mon	| 257181	| 927.1667	| 
| member	| Tue	| 273786	| 914.0390 | 
| member	| Wed	| 294308	| 923.9081	| 
| member	| Thu	| 289687	| 918.3857 | 	
| member	| Fri	| 295085	| 955.0886 | 	
| member	| Sat	| 311014	| 1076.2081 | 	
| NA	| NA	| 122128	| NA |

```
# data visualization
all_trips_v2 %>% 
  mutate(weekday = wday(start_time, label = TRUE)) %>% 
  group_by(usertype, weekday) %>% 
  summarise(number_of_rides = n(),
            average_duration = mean(ride_length)) %>% 
  arrange(usertype, weekday) %>%  
  ggplot(aes(x = weekday, y = number_of_rides, fill = usertype)) +
  geom_col(position = "dodge") +
  ggtitle("Number of Rides by Weekday")

all_trips_v2 %>% 
  mutate(weekday = wday(start_time, label = TRUE)) %>% 
  group_by(usertype, weekday) %>% 
  summarise(number_of_rides = n(),
            average_duration = mean(ride_length)) %>% 
  arrange(usertype, weekday) %>%  
  ggplot(aes(x = weekday, y = average_duration, fill = usertype)) +
  geom_col(position = "dodge") +
  ggtitle("Average Ride Duration in Seconds by Weekday")
```

![Viz 1](https://github.com/gpadolina/google_data_analytics_certificate_capstone_project/blob/main/files/Number%20of%20Rides%20by%20Weekday.png)
![Viz 2](https://github.com/gpadolina/google_data_analytics_certificate_capstone_project/blob/main/files/Avg%20Ride%20Duration%20in%20Seconds%20by%20Weekday.png)

### Share
In this phase, I chose Tableau to create some data visualization. See [here](https://github.com/gpadolina/google_data_analytics_certificate_capstone_project/blob/main/files/Cyclistic%20Bike%20Rides%20using%20Tableau%20Desktop.pdf) if the pdf version doesnt load on this page.
![Data Viz](https://github.com/gpadolina/google_data_analytics_certificate_capstone_project/blob/main/files/Cyclistic%20Bike%20Rides%20using%20Tableau%20Desktop.pdf)

### Conclusion/Act
* Overall, there are more annual members than casual riders.
