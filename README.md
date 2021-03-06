# Cyclistic-Case-Study
Google Data Analytics Certificate Final Case Study\
Cyclistic Case Study Report\
Angel Gama-Diaz

### Introduction
As part of Coursera’s ‘Google Data Analytics Certificate’ program, completing a Case Study was the final portion of the program. This Case Study was used to highlight what I learned from the program and showcase my evolution in Data Analysis.

Case Study Background
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

Setting The Scene\
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations

Three questions will guide the future marketing program: 1. How do annual members and casual riders use Cyclistic bikes differently? 2. Why would casual riders buy Cyclistic annual memberships? 3. How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned you the first question to answer: How do annual members and casual riders use Cyclistic bikes differently?

Business Task
To answer the question: How do annual members and casual riders use Cyclistic bike differently? I will be using the publicly available dataset from a real bike sharing company called Divvy. (Due to the fictional nature of Cyclistic no real data is available)

Data set here: https://divvy-tripdata.s3.amazonaws.com/index.html (The data has been made available by Motivate International Inc. under the following license: https://www.divvybikes.com/data-license-agreement)

I will be using one year of data using the Q2_2019, Q3_2019, Q4_2019, and Q1_2020 data. I will be using the program RStudio to perform my data analysis including cleaning of data, analyzation, and basic visualizations.

An assessment of the integrity of the data reveals two items of note. 1) Due to data-privacy issues, riders’ personally identifiable information is hidden. 2) This means I won’t be able to connect pass purchases to credit card numbers to determine if casual riders are visiting the service area or if they have purchased multiple single passes.

### Data Analysis Phase
To begin I will load previously installed packages that will help with cleaning and consolidating data, change and read date attributes, and visualize the data.

```
library(tidyverse)
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
## ✓ readr   2.0.1     ✓ forcats 0.5.1
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
library(lubridate)
## 
## Attaching package: 'lubridate'
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
library(ggplot2)
```

Next I will load in the four data sets that I will be using

```
q2_2019 <- read_csv("Divvy_Trips_2019_Q2")
## Rows: 1108163 Columns: 12
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): 03 - Rental Start Station Name, 02 - Rental End Station Name, User...
## dbl  (5): 01 - Rental Details Rental ID, 01 - Rental Details Bike ID, 03 - R...
## dttm (2): 01 - Rental Details Local Start Time, 01 - Rental Details Local En...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
q3_2019 <- read_csv("Divvy_Trips_2019_Q3.csv")
## Rows: 1640718 Columns: 12
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): from_station_name, to_station_name, usertype, gender
## dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
## dttm (2): start_time, end_time
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
q4_2019 <- read_csv("Divvy_Trips_2019_Q4.csv")
## Rows: 704054 Columns: 12
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): from_station_name, to_station_name, usertype, gender
## dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
## dttm (2): start_time, end_time
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
q1_2020 <- read_csv("Divvy_Trips_2020_Q1.csv")
## Rows: 426887 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
## dttm (2): started_at, ended_at
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Now that the data has been loaded into the Script/Console I will do a quick glimpse of the data to see what I am working with.

```
colnames(q2_2019)
##  [1] "01 - Rental Details Rental ID"                   
##  [2] "01 - Rental Details Local Start Time"            
##  [3] "01 - Rental Details Local End Time"              
##  [4] "01 - Rental Details Bike ID"                     
##  [5] "01 - Rental Details Duration In Seconds Uncapped"
##  [6] "03 - Rental Start Station ID"                    
##  [7] "03 - Rental Start Station Name"                  
##  [8] "02 - Rental End Station ID"                      
##  [9] "02 - Rental End Station Name"                    
## [10] "User Type"                                       
## [11] "Member Gender"                                   
## [12] "05 - Member Details Member Birthday Year"
colnames(q3_2019)
##  [1] "trip_id"           "start_time"        "end_time"         
##  [4] "bikeid"            "tripduration"      "from_station_id"  
##  [7] "from_station_name" "to_station_id"     "to_station_name"  
## [10] "usertype"          "gender"            "birthyear"
colnames(q4_2019)
##  [1] "trip_id"           "start_time"        "end_time"         
##  [4] "bikeid"            "tripduration"      "from_station_id"  
##  [7] "from_station_name" "to_station_id"     "to_station_name"  
## [10] "usertype"          "gender"            "birthyear"
colnames(q1_2020)
##  [1] "ride_id"            "rideable_type"      "started_at"        
##  [4] "ended_at"           "start_station_name" "start_station_id"  
##  [7] "end_station_name"   "end_station_id"     "start_lat"         
## [10] "start_lng"          "end_lat"            "end_lng"           
## [13] "member_casual"
```

Right away I notice there are an uneven amount of columns and that there are different column names for the same type of data. I will rename columns to match one another using the newest data set as a guide in order to combine the data sets and eliminate unused columns.

```
(q2_2019 <- rename(q2_2019,
                  ride_id="01 - Rental Details Rental ID",
                  rideable_type = "01 - Rental Details Bike ID",
                  started_at = "01 - Rental Details Local Start Time",
                  ended_at = "01 - Rental Details Local End Time",
                  start_station_name = "03 - Rental Start Station Name",
                  start_station_id = "03 - Rental Start Station ID",
                  end_station_name = "02 - Rental End Station Name",
                  end_station_id = "02 - Rental End Station ID",
                  member_casual = "User Type"))
## # A tibble: 1,108,163 × 12
##     ride_id started_at          ended_at            rideable_type
##       <dbl> <dttm>              <dttm>                      <dbl>
##  1 22178529 2019-04-01 00:02:22 2019-04-01 00:09:48          6251
##  2 22178530 2019-04-01 00:03:02 2019-04-01 00:20:30          6226
##  3 22178531 2019-04-01 00:11:07 2019-04-01 00:15:19          5649
##  4 22178532 2019-04-01 00:13:01 2019-04-01 00:18:58          4151
##  5 22178533 2019-04-01 00:19:26 2019-04-01 00:36:13          3270
##  6 22178534 2019-04-01 00:19:39 2019-04-01 00:23:56          3123
##  7 22178535 2019-04-01 00:26:33 2019-04-01 00:35:41          6418
##  8 22178536 2019-04-01 00:29:48 2019-04-01 00:36:11          4513
##  9 22178537 2019-04-01 00:32:07 2019-04-01 01:07:44          3280
## 10 22178538 2019-04-01 00:32:19 2019-04-01 01:07:39          5534
## # … with 1,108,153 more rows, and 8 more variables:
## #   01 - Rental Details Duration In Seconds Uncapped <dbl>,
## #   start_station_id <dbl>, start_station_name <chr>, end_station_id <dbl>,
## #   end_station_name <chr>, member_casual <chr>, Member Gender <chr>,
## #   05 - Member Details Member Birthday Year <dbl>
(q3_2019 <- rename(q3_2019,
                   ride_id = trip_id,
                   rideable_type = bikeid,
                   started_at = start_time,
                   ended_at = end_time,
                   start_station_name = from_station_name,
                   start_station_id = from_station_id,
                   end_station_name = to_station_name,
                   end_station_id = to_station_id,
                   member_casual = usertype))
## # A tibble: 1,640,718 × 12
##     ride_id started_at          ended_at            rideable_type tripduration
##       <dbl> <dttm>              <dttm>                      <dbl>        <dbl>
##  1 23479388 2019-07-01 00:00:27 2019-07-01 00:20:41          3591         1214
##  2 23479389 2019-07-01 00:01:16 2019-07-01 00:18:44          5353         1048
##  3 23479390 2019-07-01 00:01:48 2019-07-01 00:27:42          6180         1554
##  4 23479391 2019-07-01 00:02:07 2019-07-01 00:27:10          5540         1503
##  5 23479392 2019-07-01 00:02:13 2019-07-01 00:22:26          6014         1213
##  6 23479393 2019-07-01 00:02:21 2019-07-01 00:07:31          4941          310
##  7 23479394 2019-07-01 00:02:24 2019-07-01 00:23:12          3770         1248
##  8 23479395 2019-07-01 00:02:26 2019-07-01 00:28:16          5442         1550
##  9 23479396 2019-07-01 00:02:34 2019-07-01 00:28:57          2957         1583
## 10 23479397 2019-07-01 00:02:45 2019-07-01 00:29:14          6091         1589
## # … with 1,640,708 more rows, and 7 more variables: start_station_id <dbl>,
## #   start_station_name <chr>, end_station_id <dbl>, end_station_name <chr>,
## #   member_casual <chr>, gender <chr>, birthyear <dbl>
(q4_2019 <- rename(q4_2019,
                   ride_id = trip_id,
                   rideable_type = bikeid,
                   started_at = start_time,
                   ended_at = end_time,
                   start_station_name = from_station_name,
                   start_station_id = from_station_id,
                   end_station_name = to_station_name,
                   end_station_id = to_station_id,
                   member_casual = usertype))
## # A tibble: 704,054 × 12
##     ride_id started_at          ended_at            rideable_type tripduration
##       <dbl> <dttm>              <dttm>                      <dbl>        <dbl>
##  1 25223640 2019-10-01 00:01:39 2019-10-01 00:17:20          2215          940
##  2 25223641 2019-10-01 00:02:16 2019-10-01 00:06:34          6328          258
##  3 25223642 2019-10-01 00:04:32 2019-10-01 00:18:43          3003          850
##  4 25223643 2019-10-01 00:04:32 2019-10-01 00:43:43          3275         2350
##  5 25223644 2019-10-01 00:04:34 2019-10-01 00:35:42          5294         1867
##  6 25223645 2019-10-01 00:04:38 2019-10-01 00:10:51          1891          373
##  7 25223646 2019-10-01 00:04:52 2019-10-01 00:22:45          1061         1072
##  8 25223647 2019-10-01 00:04:57 2019-10-01 00:29:16          1274         1458
##  9 25223648 2019-10-01 00:05:20 2019-10-01 00:29:18          6011         1437
## 10 25223649 2019-10-01 00:05:20 2019-10-01 02:23:46          2957         8306
## # … with 704,044 more rows, and 7 more variables: start_station_id <dbl>,
## #   start_station_name <chr>, end_station_id <dbl>, end_station_name <chr>,
## #   member_casual <chr>, gender <chr>, birthyear <dbl>
```

Now that the columns have been renamed I will check them all for further inconsistencies.

```
str(q2_2019)
## spec_tbl_df [1,108,163 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id                                         : num [1:1108163] 22178529 22178530 22178531 22178532 22178533 ...
##  $ started_at                                      : POSIXct[1:1108163], format: "2019-04-01 00:02:22" "2019-04-01 00:03:02" ...
##  $ ended_at                                        : POSIXct[1:1108163], format: "2019-04-01 00:09:48" "2019-04-01 00:20:30" ...
##  $ rideable_type                                   : num [1:1108163] 6251 6226 5649 4151 3270 ...
##  $ 01 - Rental Details Duration In Seconds Uncapped: num [1:1108163] 446 1048 252 357 1007 ...
##  $ start_station_id                                : num [1:1108163] 81 317 283 26 202 420 503 260 211 211 ...
##  $ start_station_name                              : chr [1:1108163] "Daley Center Plaza" "Wood St & Taylor St" "LaSalle St & Jackson Blvd" "McClurg Ct & Illinois St" ...
##  $ end_station_id                                  : num [1:1108163] 56 59 174 133 129 426 500 499 211 211 ...
##  $ end_station_name                                : chr [1:1108163] "Desplaines St & Kinzie St" "Wabash Ave & Roosevelt Rd" "Canal St & Madison St" "Kingsbury St & Kinzie St" ...
##  $ member_casual                                   : chr [1:1108163] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
##  $ Member Gender                                   : chr [1:1108163] "Male" "Female" "Male" "Male" ...
##  $ 05 - Member Details Member Birthday Year        : num [1:1108163] 1975 1984 1990 1993 1992 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   `01 - Rental Details Rental ID` = col_double(),
##   ..   `01 - Rental Details Local Start Time` = col_datetime(format = ""),
##   ..   `01 - Rental Details Local End Time` = col_datetime(format = ""),
##   ..   `01 - Rental Details Bike ID` = col_double(),
##   ..   `01 - Rental Details Duration In Seconds Uncapped` = col_number(),
##   ..   `03 - Rental Start Station ID` = col_double(),
##   ..   `03 - Rental Start Station Name` = col_character(),
##   ..   `02 - Rental End Station ID` = col_double(),
##   ..   `02 - Rental End Station Name` = col_character(),
##   ..   `User Type` = col_character(),
##   ..   `Member Gender` = col_character(),
##   ..   `05 - Member Details Member Birthday Year` = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>

str(q3_2019)
## spec_tbl_df [1,640,718 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id           : num [1:1640718] 23479388 23479389 23479390 23479391 23479392 ...
##  $ started_at        : POSIXct[1:1640718], format: "2019-07-01 00:00:27" "2019-07-01 00:01:16" ...
##  $ ended_at          : POSIXct[1:1640718], format: "2019-07-01 00:20:41" "2019-07-01 00:18:44" ...
##  $ rideable_type     : num [1:1640718] 3591 5353 6180 5540 6014 ...
##  $ tripduration      : num [1:1640718] 1214 1048 1554 1503 1213 ...
##  $ start_station_id  : num [1:1640718] 117 381 313 313 168 300 168 313 43 43 ...
##  $ start_station_name: chr [1:1640718] "Wilton Ave & Belmont Ave" "Western Ave & Monroe St" "Lakeview Ave & Fullerton Pkwy" "Lakeview Ave & Fullerton Pkwy" ...
##  $ end_station_id    : num [1:1640718] 497 203 144 144 62 232 62 144 195 195 ...
##  $ end_station_name  : chr [1:1640718] "Kimball Ave & Belmont Ave" "Western Ave & 21st St" "Larrabee St & Webster Ave" "Larrabee St & Webster Ave" ...
##  $ member_casual     : chr [1:1640718] "Subscriber" "Customer" "Customer" "Customer" ...
##  $ gender            : chr [1:1640718] "Male" NA NA NA ...
##  $ birthyear         : num [1:1640718] 1992 NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   trip_id = col_double(),
##   ..   start_time = col_datetime(format = ""),
##   ..   end_time = col_datetime(format = ""),
##   ..   bikeid = col_double(),
##   ..   tripduration = col_number(),
##   ..   from_station_id = col_double(),
##   ..   from_station_name = col_character(),
##   ..   to_station_id = col_double(),
##   ..   to_station_name = col_character(),
##   ..   usertype = col_character(),
##   ..   gender = col_character(),
##   ..   birthyear = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>

str(q4_2019)
## spec_tbl_df [704,054 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id           : num [1:704054] 25223640 25223641 25223642 25223643 25223644 ...
##  $ started_at        : POSIXct[1:704054], format: "2019-10-01 00:01:39" "2019-10-01 00:02:16" ...
##  $ ended_at          : POSIXct[1:704054], format: "2019-10-01 00:17:20" "2019-10-01 00:06:34" ...
##  $ rideable_type     : num [1:704054] 2215 6328 3003 3275 5294 ...
##  $ tripduration      : num [1:704054] 940 258 850 2350 1867 ...
##  $ start_station_id  : num [1:704054] 20 19 84 313 210 156 84 156 156 336 ...
##  $ start_station_name: chr [1:704054] "Sheffield Ave & Kingsbury St" "Throop (Loomis) St & Taylor St" "Milwaukee Ave & Grand Ave" "Lakeview Ave & Fullerton Pkwy" ...
##  $ end_station_id    : num [1:704054] 309 241 199 290 382 226 142 463 463 336 ...
##  $ end_station_name  : chr [1:704054] "Leavitt St & Armitage Ave" "Morgan St & Polk St" "Wabash Ave & Grand Ave" "Kedzie Ave & Palmer Ct" ...
##  $ member_casual     : chr [1:704054] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
##  $ gender            : chr [1:704054] "Male" "Male" "Female" "Male" ...
##  $ birthyear         : num [1:704054] 1987 1998 1991 1990 1987 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   trip_id = col_double(),
##   ..   start_time = col_datetime(format = ""),
##   ..   end_time = col_datetime(format = ""),
##   ..   bikeid = col_double(),
##   ..   tripduration = col_number(),
##   ..   from_station_id = col_double(),
##   ..   from_station_name = col_character(),
##   ..   to_station_id = col_double(),
##   ..   to_station_name = col_character(),
##   ..   usertype = col_character(),
##   ..   gender = col_character(),
##   ..   birthyear = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>

str(q1_2020)
## spec_tbl_df [426,887 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id           : chr [1:426887] "EACB19130B0CDA4A" "8FED874C809DC021" "789F3C21E472CA96" "C9A388DAC6ABF313" ...
##  $ rideable_type     : chr [1:426887] "docked_bike" "docked_bike" "docked_bike" "docked_bike" ...
##  $ started_at        : POSIXct[1:426887], format: "2020-01-21 20:06:59" "2020-01-30 14:22:39" ...
##  $ ended_at          : POSIXct[1:426887], format: "2020-01-21 20:14:30" "2020-01-30 14:26:22" ...
##  $ start_station_name: chr [1:426887] "Western Ave & Leland Ave" "Clark St & Montrose Ave" "Broadway & Belmont Ave" "Clark St & Randolph St" ...
##  $ start_station_id  : num [1:426887] 239 234 296 51 66 212 96 96 212 38 ...
##  $ end_station_name  : chr [1:426887] "Clark St & Leland Ave" "Southport Ave & Irving Park Rd" "Wilton Ave & Belmont Ave" "Fairbanks Ct & Grand Ave" ...
##  $ end_station_id    : num [1:426887] 326 318 117 24 212 96 212 212 96 100 ...
##  $ start_lat         : num [1:426887] 42 42 41.9 41.9 41.9 ...
##  $ start_lng         : num [1:426887] -87.7 -87.7 -87.6 -87.6 -87.6 ...
##  $ end_lat           : num [1:426887] 42 42 41.9 41.9 41.9 ...
##  $ end_lng           : num [1:426887] -87.7 -87.7 -87.7 -87.6 -87.6 ...
##  $ member_casual     : chr [1:426887] "member" "member" "member" "member" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   ride_id = col_character(),
##   ..   rideable_type = col_character(),
##   ..   started_at = col_datetime(format = ""),
##   ..   ended_at = col_datetime(format = ""),
##   ..   start_station_name = col_character(),
##   ..   start_station_id = col_double(),
##   ..   end_station_name = col_character(),
##   ..   end_station_id = col_double(),
##   ..   start_lat = col_double(),
##   ..   start_lng = col_double(),
##   ..   end_lat = col_double(),
##   ..   end_lng = col_double(),
##   ..   member_casual = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

Checking this time I notice that columns ‘ride_id’ and ‘rideable_type’ need to be changed to character in order to stack. To do this I run the following:

```
q2_2019 <- mutate(q2_2019, ride_id = as.character(ride_id),
                  rideable_type = as.character(rideable_type))
q3_2019 <- mutate(q3_2019, ride_id = as.character(ride_id),
                  rideable_type = as.character(rideable_type))
q4_2019 <- mutate(q4_2019, ride_id = as.character(ride_id),
                  rideable_type = as.character(rideable_type))
```

Now I check the column names again to make sure the data matches and will be able to merge into one data set.

```
colnames(q2_2019)
##  [1] "ride_id"                                         
##  [2] "started_at"                                      
##  [3] "ended_at"                                        
##  [4] "rideable_type"                                   
##  [5] "01 - Rental Details Duration In Seconds Uncapped"
##  [6] "start_station_id"                                
##  [7] "start_station_name"                              
##  [8] "end_station_id"                                  
##  [9] "end_station_name"                                
## [10] "member_casual"                                   
## [11] "Member Gender"                                   
## [12] "05 - Member Details Member Birthday Year"

colnames(q3_2019)
##  [1] "ride_id"            "started_at"         "ended_at"          
##  [4] "rideable_type"      "tripduration"       "start_station_id"  
##  [7] "start_station_name" "end_station_id"     "end_station_name"  
## [10] "member_casual"      "gender"             "birthyear"

colnames(q4_2019)
##  [1] "ride_id"            "started_at"         "ended_at"          
##  [4] "rideable_type"      "tripduration"       "start_station_id"  
##  [7] "start_station_name" "end_station_id"     "end_station_name"  
## [10] "member_casual"      "gender"             "birthyear"

colnames(q1_2020)
##  [1] "ride_id"            "rideable_type"      "started_at"        
##  [4] "ended_at"           "start_station_name" "start_station_id"  
##  [7] "end_station_name"   "end_station_id"     "start_lat"         
## [10] "start_lng"          "end_lat"            "end_lng"           
## [13] "member_casual"
```

Now, I notice ‘end_station_id’ and "start_station_id’ need to go through the same cleanup.

```
q2_2019 <- mutate(q2_2019, end_station_id = as.character(end_station_id),
                  start_station_id = as.character(start_station_id))
q3_2019 <- mutate(q3_2019, end_station_id = as.character(end_station_id),
                  start_station_id = as.character(start_station_id))
q4_2019 <- mutate(q4_2019, end_station_id = as.character(end_station_id),
                  start_station_id = as.character(start_station_id))
q1_2020 <- mutate(q1_2020, end_station_id = as.character(end_station_id),
                  start_station_id = as.character(start_station_id))
```

One more check to see if everything is lining up as I want it to.

```
str(q2_2019)
## spec_tbl_df [1,108,163 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id                                         : chr [1:1108163] "22178529" "22178530" "22178531" "22178532" ...
##  $ started_at                                      : POSIXct[1:1108163], format: "2019-04-01 00:02:22" "2019-04-01 00:03:02" ...
##  $ ended_at                                        : POSIXct[1:1108163], format: "2019-04-01 00:09:48" "2019-04-01 00:20:30" ...
##  $ rideable_type                                   : chr [1:1108163] "6251" "6226" "5649" "4151" ...
##  $ 01 - Rental Details Duration In Seconds Uncapped: num [1:1108163] 446 1048 252 357 1007 ...
##  $ start_station_id                                : chr [1:1108163] "81" "317" "283" "26" ...
##  $ start_station_name                              : chr [1:1108163] "Daley Center Plaza" "Wood St & Taylor St" "LaSalle St & Jackson Blvd" "McClurg Ct & Illinois St" ...
##  $ end_station_id                                  : chr [1:1108163] "56" "59" "174" "133" ...
##  $ end_station_name                                : chr [1:1108163] "Desplaines St & Kinzie St" "Wabash Ave & Roosevelt Rd" "Canal St & Madison St" "Kingsbury St & Kinzie St" ...
##  $ member_casual                                   : chr [1:1108163] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
##  $ Member Gender                                   : chr [1:1108163] "Male" "Female" "Male" "Male" ...
##  $ 05 - Member Details Member Birthday Year        : num [1:1108163] 1975 1984 1990 1993 1992 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   `01 - Rental Details Rental ID` = col_double(),
##   ..   `01 - Rental Details Local Start Time` = col_datetime(format = ""),
##   ..   `01 - Rental Details Local End Time` = col_datetime(format = ""),
##   ..   `01 - Rental Details Bike ID` = col_double(),
##   ..   `01 - Rental Details Duration In Seconds Uncapped` = col_number(),
##   ..   `03 - Rental Start Station ID` = col_double(),
##   ..   `03 - Rental Start Station Name` = col_character(),
##   ..   `02 - Rental End Station ID` = col_double(),
##   ..   `02 - Rental End Station Name` = col_character(),
##   ..   `User Type` = col_character(),
##   ..   `Member Gender` = col_character(),
##   ..   `05 - Member Details Member Birthday Year` = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>

str(q3_2019)
## spec_tbl_df [1,640,718 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id           : chr [1:1640718] "23479388" "23479389" "23479390" "23479391" ...
##  $ started_at        : POSIXct[1:1640718], format: "2019-07-01 00:00:27" "2019-07-01 00:01:16" ...
##  $ ended_at          : POSIXct[1:1640718], format: "2019-07-01 00:20:41" "2019-07-01 00:18:44" ...
##  $ rideable_type     : chr [1:1640718] "3591" "5353" "6180" "5540" ...
##  $ tripduration      : num [1:1640718] 1214 1048 1554 1503 1213 ...
##  $ start_station_id  : chr [1:1640718] "117" "381" "313" "313" ...
##  $ start_station_name: chr [1:1640718] "Wilton Ave & Belmont Ave" "Western Ave & Monroe St" "Lakeview Ave & Fullerton Pkwy" "Lakeview Ave & Fullerton Pkwy" ...
##  $ end_station_id    : chr [1:1640718] "497" "203" "144" "144" ...
##  $ end_station_name  : chr [1:1640718] "Kimball Ave & Belmont Ave" "Western Ave & 21st St" "Larrabee St & Webster Ave" "Larrabee St & Webster Ave" ...
##  $ member_casual     : chr [1:1640718] "Subscriber" "Customer" "Customer" "Customer" ...
##  $ gender            : chr [1:1640718] "Male" NA NA NA ...
##  $ birthyear         : num [1:1640718] 1992 NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   trip_id = col_double(),
##   ..   start_time = col_datetime(format = ""),
##   ..   end_time = col_datetime(format = ""),
##   ..   bikeid = col_double(),
##   ..   tripduration = col_number(),
##   ..   from_station_id = col_double(),
##   ..   from_station_name = col_character(),
##   ..   to_station_id = col_double(),
##   ..   to_station_name = col_character(),
##   ..   usertype = col_character(),
##   ..   gender = col_character(),
##   ..   birthyear = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>

str(q4_2019)
## spec_tbl_df [704,054 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id           : chr [1:704054] "25223640" "25223641" "25223642" "25223643" ...
##  $ started_at        : POSIXct[1:704054], format: "2019-10-01 00:01:39" "2019-10-01 00:02:16" ...
##  $ ended_at          : POSIXct[1:704054], format: "2019-10-01 00:17:20" "2019-10-01 00:06:34" ...
##  $ rideable_type     : chr [1:704054] "2215" "6328" "3003" "3275" ...
##  $ tripduration      : num [1:704054] 940 258 850 2350 1867 ...
##  $ start_station_id  : chr [1:704054] "20" "19" "84" "313" ...
##  $ start_station_name: chr [1:704054] "Sheffield Ave & Kingsbury St" "Throop (Loomis) St & Taylor St" "Milwaukee Ave & Grand Ave" "Lakeview Ave & Fullerton Pkwy" ...
##  $ end_station_id    : chr [1:704054] "309" "241" "199" "290" ...
##  $ end_station_name  : chr [1:704054] "Leavitt St & Armitage Ave" "Morgan St & Polk St" "Wabash Ave & Grand Ave" "Kedzie Ave & Palmer Ct" ...
##  $ member_casual     : chr [1:704054] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
##  $ gender            : chr [1:704054] "Male" "Male" "Female" "Male" ...
##  $ birthyear         : num [1:704054] 1987 1998 1991 1990 1987 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   trip_id = col_double(),
##   ..   start_time = col_datetime(format = ""),
##   ..   end_time = col_datetime(format = ""),
##   ..   bikeid = col_double(),
##   ..   tripduration = col_number(),
##   ..   from_station_id = col_double(),
##   ..   from_station_name = col_character(),
##   ..   to_station_id = col_double(),
##   ..   to_station_name = col_character(),
##   ..   usertype = col_character(),
##   ..   gender = col_character(),
##   ..   birthyear = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>

str(q1_2020)
## spec_tbl_df [426,887 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ ride_id           : chr [1:426887] "EACB19130B0CDA4A" "8FED874C809DC021" "789F3C21E472CA96" "C9A388DAC6ABF313" ...
##  $ rideable_type     : chr [1:426887] "docked_bike" "docked_bike" "docked_bike" "docked_bike" ...
##  $ started_at        : POSIXct[1:426887], format: "2020-01-21 20:06:59" "2020-01-30 14:22:39" ...
##  $ ended_at          : POSIXct[1:426887], format: "2020-01-21 20:14:30" "2020-01-30 14:26:22" ...
##  $ start_station_name: chr [1:426887] "Western Ave & Leland Ave" "Clark St & Montrose Ave" "Broadway & Belmont Ave" "Clark St & Randolph St" ...
##  $ start_station_id  : chr [1:426887] "239" "234" "296" "51" ...
##  $ end_station_name  : chr [1:426887] "Clark St & Leland Ave" "Southport Ave & Irving Park Rd" "Wilton Ave & Belmont Ave" "Fairbanks Ct & Grand Ave" ...
##  $ end_station_id    : chr [1:426887] "326" "318" "117" "24" ...
##  $ start_lat         : num [1:426887] 42 42 41.9 41.9 41.9 ...
##  $ start_lng         : num [1:426887] -87.7 -87.7 -87.6 -87.6 -87.6 ...
##  $ end_lat           : num [1:426887] 42 42 41.9 41.9 41.9 ...
##  $ end_lng           : num [1:426887] -87.7 -87.7 -87.7 -87.6 -87.6 ...
##  $ member_casual     : chr [1:426887] "member" "member" "member" "member" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   ride_id = col_character(),
##   ..   rideable_type = col_character(),
##   ..   started_at = col_datetime(format = ""),
##   ..   ended_at = col_datetime(format = ""),
##   ..   start_station_name = col_character(),
##   ..   start_station_id = col_double(),
##   ..   end_station_name = col_character(),
##   ..   end_station_id = col_double(),
##   ..   start_lat = col_double(),
##   ..   start_lng = col_double(),
##   ..   end_lat = col_double(),
##   ..   end_lng = col_double(),
##   ..   member_casual = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

Seeing that indeed the columns will stack and therefore the data will be consolidated; it is time to merge the four data sets into one.

```
all_trips <- bind_rows(q2_2019,q3_2019,q4_2019,q1_2020)
Excellent, the four data sets are now combined into one set named ‘all_trips’ It is now time to remove the unused columns.

all_trips <- all_trips %>%
  select(-c(start_lat,start_lng,end_lat,end_lng,birthyear,gender,
            "01 - Rental Details Duration In Seconds Uncapped",
            "05 - Member Details Member Birthday Year",
            "Member Gender", "tripduration"))
```

Four columns of data were removed. Now I will check the remaining columns, generate a table of the new data set, and generate a summary in order to learn more about the data.

```
colnames(all_trips)
## [1] "ride_id"            "started_at"         "ended_at"          
## [4] "rideable_type"      "start_station_id"   "start_station_name"
## [7] "end_station_id"     "end_station_name"   "member_casual"
View(all_trips)
summary(all_trips)
##    ride_id            started_at                     ended_at                  
##  Length:3879822     Min.   :2019-04-01 00:02:22   Min.   :2019-04-01 00:09:48  
##  Class :character   1st Qu.:2019-06-23 07:49:09   1st Qu.:2019-06-23 08:20:27  
##  Mode  :character   Median :2019-08-14 17:43:38   Median :2019-08-14 18:02:04  
##                     Mean   :2019-08-26 00:49:59   Mean   :2019-08-26 01:14:37  
##                     3rd Qu.:2019-10-12 12:10:21   3rd Qu.:2019-10-12 12:36:16  
##                     Max.   :2020-03-31 23:51:34   Max.   :2020-05-19 20:10:34  
##  rideable_type      start_station_id   start_station_name end_station_id    
##  Length:3879822     Length:3879822     Length:3879822     Length:3879822    
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##  end_station_name   member_casual     
##  Length:3879822     Length:3879822    
##  Class :character   Class :character  
##  Mode  :character   Mode  :character  
##                                       
##                                       
## 
```

I am down to nine columns of data, and looking through the table that was generated, spot some changes that need to be implemented.

The first change is to combined the four different names used for riders into two usable names. I will check the data before and after to make sure the merger worked.

```
table(all_trips$member_casual)
## 
##     casual   Customer     member Subscriber 
##      48480     857474     378407    2595461
all_trips <- all_trips %>%
  mutate(member_casual=recode(member_casual,
                              "Subscriber" = "member",
                              "Customer" = "casual"))
table(all_trips$member_casual)
## 
##  casual  member 
##  905954 2973868
```

Now that I verified that the changes worked, it is time for the next change. I aim to split up the Dates into a more usable format by having separate columns for Month, Day, and Year of each ride.

```
all_trips$date <- as.Date(all_trips$started_at)
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date),"%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")
```

Now I can use the different columns to combine or analyze the data if needed.

Next I will make a ride length calculation and read it.

```
all_trips$ride_length <-difftime(all_trips$ended_at,all_trips$started_at)

str(all_trips)
## tibble [3,879,822 × 15] (S3: tbl_df/tbl/data.frame)
##  $ ride_id           : chr [1:3879822] "22178529" "22178530" "22178531" "22178532" ...
##  $ started_at        : POSIXct[1:3879822], format: "2019-04-01 00:02:22" "2019-04-01 00:03:02" ...
##  $ ended_at          : POSIXct[1:3879822], format: "2019-04-01 00:09:48" "2019-04-01 00:20:30" ...
##  $ rideable_type     : chr [1:3879822] "6251" "6226" "5649" "4151" ...
##  $ start_station_id  : chr [1:3879822] "81" "317" "283" "26" ...
##  $ start_station_name: chr [1:3879822] "Daley Center Plaza" "Wood St & Taylor St" "LaSalle St & Jackson Blvd" "McClurg Ct & Illinois St" ...
##  $ end_station_id    : chr [1:3879822] "56" "59" "174" "133" ...
##  $ end_station_name  : chr [1:3879822] "Desplaines St & Kinzie St" "Wabash Ave & Roosevelt Rd" "Canal St & Madison St" "Kingsbury St & Kinzie St" ...
##  $ member_casual     : chr [1:3879822] "member" "member" "member" "member" ...
##  $ date              : Date[1:3879822], format: "2019-04-01" "2019-04-01" ...
##  $ month             : chr [1:3879822] "04" "04" "04" "04" ...
##  $ day               : chr [1:3879822] "01" "01" "01" "01" ...
##  $ year              : chr [1:3879822] "2019" "2019" "2019" "2019" ...
##  $ day_of_week       : chr [1:3879822] "Monday" "Monday" "Monday" "Monday" ...
##  $ ride_length       : 'difftime' num [1:3879822] 446 1048 252 357 ...
##   ..- attr(*, "units")= chr "secs"
```

I can see that the new ‘ride_length’ is in Factor format and needs to be in Numeric format to be able to run calculations on the data.

```
is.factor(all_trips$ride_length)
## [1] FALSE
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))
is.numeric(all_trips$ride_length)
## [1] TRUE
```

At this point I generate a table again in order to play around with the data and see what else needs to be cleaned, changed, or added.

```
View(all_trips)
```

Everything is looking much better and now it is time to clean the “bad” data. There are negative ride_length entries as well as entries that appear to be taken to be serviced. A new data set will be created due to removing a portion of the data.

```
all_trips_v2 <- all_trips[!(all_trips$ride_length<0),]

summary(all_trips_v2$ride_length)
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0     411     711    1478    1288 9387024
```

The summary of the new data set ‘all_trips_2’ shows me the min, median, mean, and max points of data.

At this point it is time to start comparing data to draw conclusions.

```
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = mean)
##   all_trips_v2$member_casual all_trips_v2$ride_length
## 1                     casual                3538.4516
## 2                     member                 850.0659
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = median)
##   all_trips_v2$member_casual all_trips_v2$ride_length
## 1                     casual                     1540
## 2                     member                      589
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = max)
##   all_trips_v2$member_casual all_trips_v2$ride_length
## 1                     casual                  9387024
## 2                     member                  9056634
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = min)
##   all_trips_v2$member_casual all_trips_v2$ride_length
## 1                     casual                        0
## 2                     member                        1
```

Looking at the aggregate data of ‘ride_length’ I can start to see that there is a large difference between the average ride length of causal users vs members.

Now I will check the average ride time per day of the week for both types of members.

```
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual +
          all_trips_v2$day_of_week, FUN = mean)
##    all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
## 1                      casual                   Friday                3758.2210
## 2                      member                   Friday                 824.5305
## 3                      casual                   Monday                3335.6446
## 4                      member                   Monday                 842.5726
## 5                      casual                 Saturday                3331.9138
## 6                      member                 Saturday                 968.9337
## 7                      casual                   Sunday                3581.4054
## 8                      member                   Sunday                 919.9746
## 9                      casual                 Thursday                3660.2933
## 10                     member                 Thursday                 823.9278
## 11                     casual                  Tuesday                3569.7986
## 12                     member                  Tuesday                 826.1427
## 13                     casual                Wednesday                3691.0203
## 14                     member                Wednesday                 823.9980
```

Noticing that the dates are out of order, I run the following to adjust this:

```
all_trips_v2$day_of_week <- ordered(all_trips_v2$day_of_week, levels=c(
  "Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"))
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual +
            all_trips_v2$day_of_week, FUN = mean)
##    all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
## 1                      casual                   Sunday                3581.4054
## 2                      member                   Sunday                 919.9746
## 3                      casual                   Monday                3335.6446
## 4                      member                   Monday                 842.5726
## 5                      casual                  Tuesday                3569.7986
## 6                      member                  Tuesday                 826.1427
## 7                      casual                Wednesday                3691.0203
## 8                      member                Wednesday                 823.9980
## 9                      casual                 Thursday                3660.2933
## 10                     member                 Thursday                 823.9278
## 11                     casual                   Friday                3758.2210
## 12                     member                   Friday                 824.5305
## 13                     casual                 Saturday                3331.9138
## 14                     member                 Saturday                 968.9337
```

To get a good look at ridership data by type and weekday I run this:

```
all_trips_v2 %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday)
## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.
## # A tibble: 14 × 4
## # Groups:   member_casual [2]
##    member_casual weekday number_of_rides average_duration
##    <chr>         <ord>             <int>            <dbl>
##  1 casual        Sun              181293            3581.
##  2 casual        Mon              104432            3336.
##  3 casual        Tue               91184            3570.
##  4 casual        Wed               93150            3691.
##  5 casual        Thu              103316            3660.
##  6 casual        Fri              122913            3758.
##  7 casual        Sat              209543            3332.
##  8 member        Sun              267965             920.
##  9 member        Mon              472196             843.
## 10 member        Tue              508445             826.
## 11 member        Wed              500330             824.
## 12 member        Thu              484177             824.
## 13 member        Fri              452790             825.
## 14 member        Sat              287958             969.
```

This results in a tibble (data frame) showing member amount of rides and duration per weekday.

At this point it is time to revisit the original Business Task and see if there is progress towards the question that needs to be answered.

- How do annual members and casual riders use Cyclistic bikes differently?

To answer this question I will run two data visualizations that can shed some key differences in riders.

The first viz will be ‘Number of riders per weekday’

```
all_trips_v2 %>%
  mutate(weekday=wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = (n()/10000), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x=weekday, y=number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge") +
  labs(title = "Number of riders per weekday", 
       x = "Weekday", y = "Number of riders (10k)", fill = "Member Type")
## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.
```

![image](https://user-images.githubusercontent.com/91291459/135568200-1ab24b3c-6fb8-4c9e-94de-b194f743b5f1.png)


After some trial and error I was able to make the necessary changes to provide a clean visual. I am grouping by ‘member_casual’ or member type and weekday, using a formula of ‘number_of_rides’ (divided by ten thousand in order to fit the data better into the visual) and using the mean of ‘ride_length’ as ‘average_duration’. Next, it is arranged by member type and weekday. The actual type of visual and its elements are next, setting the X and Y axis using the ‘weekday’, and ‘number_of_rides’ data. The fill or what the column will show is the member type. A column/bar chart was chosen due to the data that was being compared, one type versus one type. Lastly, I labeled the visual to be able to read it at a glance.

The second visual will be ‘Average duration of ride per weekday’

```
all_trips_v2 %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), 
            average_duration = mean(ride_length)/60) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x=weekday, y=average_duration, fill = member_casual)) +
  geom_col(position = "dodge") +
  labs(title = "Average duration of ride per weekday", x = "Weekday",
       y = "Average duration in minutes", fill = "Member Type")
## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.
```

![image](https://user-images.githubusercontent.com/91291459/135568314-e88e6f40-71a6-4734-ad2e-e129a0164ddb.png)
  
Almost identical set up to the first visual. The key changes are using ‘average_duration’ as the Y axis, changing the labels, and dividing the ‘ride_length’ mean by 60 as the duration was in seconds thus dividing by 60 yields minutes.

### Conclusions
The purpose of this Case Study was to showcase my ability to follow the data analysis process as detailed in Courseras ‘Google Data Analytics Certificate’ program.

The ASK portion was the question that I intended to answer: How do annual members and casual riders use Cyclistic bikes differently?

The PREPARE portion was to check where the data would come from. What issues might arise. How I would go about analyzing the data.

The PROCESS phase was get a first feel for it and choose which program I would use to analyze the data. Start cleaning the data, finding trends, errors, changes that needed to be made and more.

The ANALYZE phase was to take the new data set I had created by merging four different sets, cleaning it, and creating two visualizations to explain my analysis from the data.

The SHARE phase, involves showing the visualizations that were made and creating this R Markdown to document my journey through this Case Study.

Finally the ACT portion is to submit this Case Study for the program, and share this Case Study in my portfolio as a testament to the start of my journey in Data Analysis.

Thank You
