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
In this phase, I have performed data wrangling including renaming column values and deriving new ones from existing columns. See more below.
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
