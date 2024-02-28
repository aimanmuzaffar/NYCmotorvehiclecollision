PROJECT NAME: ## NYC Motor Vehicle Collisions Crashes

HOW TO INSTALL AND RUN THE PROJECT: Jupyter notebook, AWS

DEPENDENCIES: - [Python](https://www.python.org/)

- [Jupyter Notebook](https://jupyter.org/) (version 6.5.4)


### Ask1 - Search for a dataset

**Identify and describe our dataset** 

- For our project, we intend to analyze New York Motor Vehicle Collisions. The dataset provides comprehensive details based on police reports for motor vehicle collisions in NYC.
- Each row in the dataset represents a crash event, including information such as the crash date, time, location, contributing factors, vehicle types, and outcomes.
- Dataset Size: 436.6 MB, Last Update: 11/28/2023, 2,045,723 records at the transactional level, and 29 variables.


**Dataset Source**

The Home of the U.S. Government's Open Database, New York City Police Department (NYPD). https://catalog.data.gov/dataset/motor-vehicle-collisions-crashes

**Why is important and what appeals to you about this dataset?**

The chosen dataset holds substantial potential for yielding valuable analytical insights. By delving into the patterns of motor vehicle collisions, we anticipate gaining a comprehensive understanding of the contributing factors and outcomes associated with these incidents.
      
Analyzing temporal patterns, spatial distributions, and identifying contributing factors such as vehicle types can uncover trends and correlations that contribute to enhancing traffic safety. Insights derived from this dataset can inform evidence-based decision-making, enabling us to propose targeted interventions and preventive measures to mitigate the occurrence and severity of motor vehicle collisions. This aligns with the broader goal of leveraging analytics for actionable insights.

**Acquire data**


```python
!pwd
```

    /home/ubuntu/notebooks/FinalProjectGroup2



```python
#Count the number of ,lines (rows) in the raw data; the result should be 2045723.
!wc -l Motor_Vehicle_Collisions_-_Crashes.csv
```

    2045723 Motor_Vehicle_Collisions_-_Crashes.csv


The reason we didn't use the wget command to acquire the dataset is that the dataset is frequently updated on the website. To ensure consistency with a specific version of the dataset, we directly uploaded the dataset file, which was downloaded from https://catalog.data.gov/dataset/motor-vehicle-collisions-crashes on November 28.

( !wget 'https://data.cityofnewyork.us/api/views/h9gi-nx95/rows.csv?accessType=DOWNLOAD' -O Motor_Vehicle_Collisions_-_Crashes.csv )


```python
# Rename the file. 
!mv Motor_Vehicle_Collisions_-_Crashes.csv MVC.csv
```


```python
#Check data
!csvclean -n MVC.csv
```

    No errors.



```python
#Check the column names
!csvcut -n MVC.csv
```

      1: CRASH DATE
      2: CRASH TIME
      3: BOROUGH
      4: ZIP CODE
      5: LATITUDE
      6: LONGITUDE
      7: LOCATION
      8: ON STREET NAME
      9: CROSS STREET NAME
     10: OFF STREET NAME
     11: NUMBER OF PERSONS INJURED
     12: NUMBER OF PERSONS KILLED
     13: NUMBER OF PEDESTRIANS INJURED
     14: NUMBER OF PEDESTRIANS KILLED
     15: NUMBER OF CYCLIST INJURED
     16: NUMBER OF CYCLIST KILLED
     17: NUMBER OF MOTORIST INJURED
     18: NUMBER OF MOTORIST KILLED
     19: CONTRIBUTING FACTOR VEHICLE 1
     20: CONTRIBUTING FACTOR VEHICLE 2
     21: CONTRIBUTING FACTOR VEHICLE 3
     22: CONTRIBUTING FACTOR VEHICLE 4
     23: CONTRIBUTING FACTOR VEHICLE 5
     24: COLLISION_ID
     25: VEHICLE TYPE CODE 1
     26: VEHICLE TYPE CODE 2
     27: VEHICLE TYPE CODE 3
     28: VEHICLE TYPE CODE 4
     29: VEHICLE TYPE CODE 5



```python
!csvgrep -c1 -r "201[6789]|202[012]" MVC.csv > MVC_filtered.csv
```

**Key Decision we took to modify/remove data**

- While the original dataset we downloaded spans a broad timeframe, from 1998 to 2022, the source website (https://catalog.data.gov/dataset/motor-vehicle-collisions-crashes) indicates a significant change in data collection methods, transitioning from manual entry to electronic entry. Therefore, to address data entry inconsistencies, we have opted to narrow our analysis to the years 2016 to 2020. This allows us to focus on a specific period, providing a more concentrated and relevant dataset for our analysis.


```python
#Count the number of lines (rows) in the filtered file; the result should be 1231230.
!wc -l MVC_filtered.csv
```

    1231230 MVC_filtered.csv


**Perform initial exploration**

Using 'csvstat' to generate statistical information, this includes variable data types, and it helps identify columns that may contain null values.
**Note: This code should take at least 10min to run.** 


```python
!csvstat MVC_filtered.csv
```

    /home/ubuntu/.local/lib/python3.8/site-packages/agate/table/from_csv.py:74: RuntimeWarning: Error sniffing CSV dialect: Could not determine delimiter
      1. "CRASH DATE"
    
    	Type of data:          Date
    	Contains null values:  False
    	Unique values:         2557
    	Smallest value:        2016-01-01
    	Largest value:         2022-12-31
    	Most common values:    2018-11-15 (1065x)
    	                       2017-12-15 (999x)
    	                       2017-05-19 (974x)
    	                       2017-05-18 (911x)
    	                       2017-01-07 (896x)
    
      2. "CRASH TIME"
    
    	Type of data:          TimeDelta
    	Contains null values:  False
    	Unique values:         1440
    	Smallest value:        0:00:00
    	Largest value:         0:23:59
    	Sum:                   11548 days, 9:08:55
    	Most common values:    0:00:00 (18717x)
    	                       0:16:00 (17339x)
    	                       0:17:00 (16956x)
    	                       0:15:00 (16631x)
    	                       0:14:00 (15825x)
    
      3. "BOROUGH"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         6
    	Longest value:         13 characters
    	Most common values:    None (434027x)
    	                       BROOKLYN (258181x)
    	                       QUEENS (219818x)
    	                       MANHATTAN (160127x)
    	                       BRONX (128533x)
    
      4. "ZIP CODE"
    
    	Type of data:          Number
    	Contains null values:  True (excluded from calculations)
    	Unique values:         231
    	Smallest value:        10000
    	Largest value:         11697
    	Sum:                   8659006718
    	Mean:                  10864.664
    	Median:                11208
    	StDev:                 543.846
    	Most common values:    None (434241x)
    	                       11207 (16072x)
    	                       11236 (11813x)
    	                       11234 (10996x)
    	                       11212 (10782x)
    
      5. "LATITUDE"
    
    	Type of data:          Number
    	Contains null values:  True (excluded from calculations)
    	Unique values:         96603
    	Smallest value:        0
    	Largest value:         43.344
    	Sum:                   45487344.793
    	Mean:                  40.579
    	Median:                40.72
    	StDev:                 2.435
    	Most common values:    None (110274x)
    	                       0 (4018x)
    	                       40.862 (807x)
    	                       40.696 (714x)
    	                       40.805 (691x)
    
      6. "LONGITUDE"
    
    	Type of data:          Number
    	Contains null values:  True (excluded from calculations)
    	Unique values:         68754
    	Smallest value:        -201.36
    	Largest value:         0
    	Sum:                   -82574466.816
    	Mean:                  -73.664
    	Median:                -73.923
    	StDev:                 4.592
    	Most common values:    None (110274x)
    	                       0 (4018x)
    	                       -73.913 (717x)
    	                       -73.985 (697x)
    	                       -73.891 (694x)
    
      7. "LOCATION"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         235212
    	Longest value:         25 characters
    	Most common values:    None (110274x)
    	                       (0.0, 0.0) (4018x)
    	                       (40.861862, -73.91282) (685x)
    	                       (40.608757, -74.038086) (669x)
    	                       (40.696033, -73.98453) (646x)
    
      8. "ON STREET NAME"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         16227
    	Longest value:         32 characters
    	Most common values:    None (300891x)
    	                       BELT PARKWAY                     (13539x)
    	                       LONG ISLAND EXPRESSWAY           (9924x)
    	                       BROOKLYN QUEENS EXPRESSWAY       (9740x)
    	                       BROADWAY                         (8835x)
    
      9. "CROSS STREET NAME"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         16151
    	Longest value:         32 characters
    	Most common values:    None (617059x)
    	                       3 AVENUE (6166x)
    	                       BROADWAY (5725x)
    	                       2 AVENUE (4540x)
    	                       5 AVENUE (3421x)
    
     10. "OFF STREET NAME"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         187411
    	Longest value:         40 characters
    	Most common values:    None (937655x)
    	                       772       EDGEWATER ROAD                 (402x)
    	                       110-00    ROCKAWAY BOULEVARD             (261x)
    	                       2800      VICTORY BOULEVARD              (236x)
    	                       2655      RICHMOND AVENUE                (169x)
    
     11. "NUMBER OF PERSONS INJURED"
    
    	Type of data:          Number
    	Contains null values:  True (excluded from calculations)
    	Unique values:         26
    	Smallest value:        0
    	Largest value:         40
    	Sum:                   392626
    	Mean:                  0.319
    	Median:                0
    	StDev:                 0.702
    	Most common values:    0 (940894x)
    	                       1 (225125x)
    	                       2 (42999x)
    	                       3 (13754x)
    	                       4 (5079x)
    
     12. "NUMBER OF PERSONS KILLED"
    
    	Type of data:          Number
    	Contains null values:  True (excluded from calculations)
    	Unique values:         7
    	Smallest value:        0
    	Largest value:         8
    	Sum:                   1829
    	Mean:                  0.001
    	Median:                0
    	StDev:                 0.041
    	Most common values:    0 (1229439x)
    	                       1 (1707x)
    	                       2 (41x)
    	                       None (31x)
    	                       3 (8x)
    
     13. "NUMBER OF PEDESTRIANS INJURED"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         9
    	Smallest value:        0
    	Largest value:         27
    	Sum:                   67099
    	Mean:                  0.054
    	Median:                0
    	StDev:                 0.239
    	Most common values:    0 (1166856x)
    	                       1 (62029x)
    	                       2 (2086x)
    	                       3 (196x)
    	                       4 (36x)
    
     14. "NUMBER OF PEDESTRIANS KILLED"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         4
    	Smallest value:        0
    	Largest value:         6
    	Sum:                   894
    	Mean:                  0.001
    	Median:                0
    	StDev:                 0.028
    	Most common values:    0 (1230347x)
    	                       1 (874x)
    	                       2 (7x)
    	                       6 (1x)
    
     15. "NUMBER OF CYCLIST INJURED"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         4
    	Smallest value:        0
    	Largest value:         3
    	Sum:                   35138
    	Mean:                  0.029
    	Median:                0
    	StDev:                 0.169
    	Most common values:    0 (1196567x)
    	                       1 (34202x)
    	                       2 (444x)
    	                       3 (16x)
    
     16. "NUMBER OF CYCLIST KILLED"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         3
    	Smallest value:        0
    	Largest value:         2
    	Sum:                   153
    	Mean:                  0
    	Median:                0
    	StDev:                 0.011
    	Most common values:    0 (1231077x)
    	                       1 (151x)
    	                       2 (1x)
    
     17. "NUMBER OF MOTORIST INJURED"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         24
    	Smallest value:        0
    	Largest value:         40
    	Sum:                   285599
    	Mean:                  0.232
    	Median:                0
    	StDev:                 0.665
    	Most common values:    0 (1042706x)
    	                       1 (127588x)
    	                       2 (39325x)
    	                       3 (13348x)
    	                       4 (4988x)
    
     18. "NUMBER OF MOTORIST KILLED"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         5
    	Smallest value:        0
    	Largest value:         4
    	Sum:                   748
    	Mean:                  0.001
    	Median:                0
    	StDev:                 0.027
    	Most common values:    0 (1230533x)
    	                       1 (654x)
    	                       2 (33x)
    	                       3 (8x)
    	                       4 (1x)
    
     19. "CONTRIBUTING FACTOR VEHICLE 1"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         62
    	Longest value:         53 characters
    	Most common values:    Driver Inattention/Distraction (296909x)
    	                       Unspecified (294446x)
    	                       Following Too Closely (103115x)
    	                       Failure to Yield Right-of-Way (83760x)
    	                       Backing Unsafely (52827x)
    
     20. "CONTRIBUTING FACTOR VEHICLE 2"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         61
    	Longest value:         53 characters
    	Most common values:    Unspecified (850298x)
    	                       None (207535x)
    	                       Driver Inattention/Distraction (64504x)
    	                       Following Too Closely (17594x)
    	                       Other Vehicular (15867x)
    
     21. "CONTRIBUTING FACTOR VEHICLE 3"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         48
    	Longest value:         53 characters
    	Most common values:    None (1141064x)
    	                       Unspecified (84219x)
    	                       Following Too Closely (1759x)
    	                       Other Vehicular (1545x)
    	                       Driver Inattention/Distraction (1259x)
    
     22. "CONTRIBUTING FACTOR VEHICLE 4"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         31
    	Longest value:         30 characters
    	Most common values:    None (1210336x)
    	                       Unspecified (19760x)
    	                       Other Vehicular (380x)
    	                       Following Too Closely (342x)
    	                       Driver Inattention/Distraction (176x)
    
     23. "CONTRIBUTING FACTOR VEHICLE 5"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         22
    	Longest value:         30 characters
    	Most common values:    None (1225432x)
    	                       Unspecified (5473x)
    	                       Other Vehicular (121x)
    	                       Following Too Closely (83x)
    	                       Driver Inattention/Distraction (42x)
    
     24. "COLLISION_ID"
    
    	Type of data:          Number
    	Contains null values:  False
    	Unique values:         1231229
    	Smallest value:        3363355
    	Largest value:         4673556
    	Sum:                   4900183019116
    	Mean:                  3979911.957
    	Median:                3979988
    	StDev:                 355675.025
    	Most common values:    4455765 (1x)
    	                       4513547 (1x)
    	                       4541903 (1x)
    	                       4456314 (1x)
    	                       4486609 (1x)
    
     25. "VEHICLE TYPE CODE 1"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         1463
    	Longest value:         38 characters
    	Most common values:    Sedan (523350x)
    	                       Station Wagon/Sport Utility Vehicle (413130x)
    	                       Taxi (48425x)
    	                       4 dr sedan (40113x)
    	                       Pick-up Truck (32039x)
    
     26. "VEHICLE TYPE CODE 2"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         1632
    	Longest value:         38 characters
    	Most common values:    Sedan (372776x)
    	                       Station Wagon/Sport Utility Vehicle (303056x)
    	                       None (286086x)
    	                       Taxi (35959x)
    	                       4 dr sedan (30046x)
    
     27. "VEHICLE TYPE CODE 3"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         231
    	Longest value:         35 characters
    	Most common values:    None (1146204x)
    	                       Sedan (39675x)
    	                       Station Wagon/Sport Utility Vehicle (32047x)
    	                       4 dr sedan (2558x)
    	                       Taxi (2065x)
    
     28. "VEHICLE TYPE CODE 4"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         90
    	Longest value:         35 characters
    	Most common values:    None (1211440x)
    	                       Sedan (9468x)
    	                       Station Wagon/Sport Utility Vehicle (7713x)
    	                       4 dr sedan (566x)
    	                       Pick-up Truck (426x)
    
     29. "VEHICLE TYPE CODE 5"
    
    	Type of data:          Text
    	Contains null values:  True (excluded from calculations)
    	Unique values:         58
    	Longest value:         35 characters
    	Most common values:    None (1225695x)
    	                       Sedan (2639x)
    	                       Station Wagon/Sport Utility Vehicle (2168x)
    	                       Pick-up Truck (137x)
    	                       4 dr sedan (123x)
    
    Row count: 1231229


After we acquire data and perform initial exploration by using "csvstat" command.
Based on the provided statistical information, we are reasonably sure the dataset appears suitable for dimensional modeling and analytical analysis for the following reasons:

**"CRASH DATE" and "CRASH TIME":**
"CRASH DATE" contains date values ranging from 2016-01-01 to 2022-12-31, and "CRASH TIME" contains time values. Both columns have no null values and provide a detailed temporal aspect for analysis.

**"BOROUGH" and "ZIP CODE":**
These columns contain categorical and numerical location information, respectively. While "BOROUGH" has null values, they are excluded from calculations. Both columns offer geographic context for analysis.

**"LATITUDE" and "LONGITUDE":**
These numerical columns provide precise location coordinates. Although they have null values, they are excluded from calculations. They can be used for spatial analysis.

**"NUMBER OF PERSONS INJURED" and "NUMBER OF PERSONS KILLED":**
These numerical columns contain information on injuries and fatalities in accidents, allowing for safety analysis.

**"CONTRIBUTING FACTOR VEHICLE 1" to "CONTRIBUTING FACTOR VEHICLE 5":**
These categorical columns provide information on contributing factors in accidents, allowing for analysis of the causes of incidents.

**"VEHICLE TYPE CODE 1" to "VEHICLE TYPE CODE 5":**
These categorical columns provide information on the types of vehicles involved in accidents, enabling analysis based on vehicle types.

**"COLLISION_ID":**
This numerical column serves as a unique identifier for each collision, allowing for easy referencing in analysis.

Overall, the dataset offers a diverse set of features, both numerical and categorical, with a substantial number of entries (1231230 rows), making it suitable for comprehensive dimensional modeling and analytical exploration.

**Analytical questions we can answer with this data**

We are a consultation firm providing an overview of the New York City traffic incident trends from 2016-2022. Our goal is to offer suggestions to car insurance companies, the New York City Police (NYPD) Traffic Enforcement, and the New York Department of Motor Vehicles (DMV).

Analysis 1 for Car Insurance:
- Which top 10 vehicle types have the most crashes?
- In which boroughs do the most crashes occur?

Analysis 2 for NYPD Traffic Enforcement:
- Which top 10 zip code areas have the highest number of persons injured due to crashes?
- What combination of day of the week, hour of the day, and zip code experienced the top 30 highest numbers of persons injured in crashes?

Analysis 3 for New York Department of Motor Vehicles (DMV):
- Identify the top 10 contributing factors to the highest number of fatalities.

**Concerns with the data and changes we expect to overcome**

**Concerns**
- Data Completeness: There might be potential missing values, particularly in crucial fields such as boroughs, zip codes, contributing factors, and vehicle types.
- Data Volume/Computational Issues: Extracting data from 2016-2022 results in around 1,200,000 records, potentially leading to slow processing times for codes.

**Expected Changes:** 
We anticipate addressing these concerns by dropping null values in critical fields and eliminating unnecessary columns.


### Ask2 - Data Wrangling & Data Cleaning 

The steps for Ask 2 are as follows:

1. Creation of Fact Table:

    We initiate the process by creating a fact table named "Crashes" and utilize the COPY command to import all data into our primary fact table.

2. Column Selection and Cleanup:

    Next, we streamline our dataset by eliminating unnecessary columns in our data analysis, such as latitude, longitude, location, on_street_name, cross_street_name, off_street_name, number_of_pedestrians_injured, number_of_pedestrians_killed, number_of_cyclist_injured, number_of_cyclist_killed, number_of_motorist_injured, and number_of_motorist_killed.

3. Handling Null Values:

    We proceed by removing null values in the 'borough,' 'zip_code,' and 'contributing_factor_vehicle_1' columns. Given that these columns are crucial for our analytics, any rows with null values in these columns are dropped.

4. Star Schema Design:

    Our modeling approach involves creating a star schema. We identify the fact table as "Crashes" and establish four dimensional tables: Geography, Contributing factor, Vehicle type, and Crash time.

5. Dimensional Table Processing:

    We delve into working on the four dimensional tables (Geography, Contributing factor, Vehicle type, and Crash time). Simultaneously, we modify the fact table and establish links between them.

6. Final Fact Table:
    Upon completing the above steps, we arrive at our final fact table, which integrates the cleaned and processed data from both the fact and dimensional tables.


```python
%load_ext sql
```


```python
!dropdb -U student Final_Project
```


```python
!createdb -U student Final_Project
```


```python
%sql postgresql://student@/Final_Project
```

The above code cells %load_text sql allows for the %%sql magic command to used through out ther notebook. To have the next tasks completed you will need to run this command line. The following codes asks that you drop and create the database Final_Project. These two command will not need to be executed once they have been executed before. The final code line "%sql postgresql://student@/Final_Project creates the link between the database you've created and the notebook you are currently working in. 


```sql
%%sql

DROP TABLE IF EXISTS Crashes;

CREATE TABLE Crashes (    
    CRASH_DATE DATE NOT NULL,
    CRASH_TIME TIME(20) NOT NULL,
    BOROUGH CHAR(20),
    ZIP_CODE NUMERIC(10),
    LATITUDE NUMERIC(20),
    LONGITUDE NUMERIC(20),
    LOCATION CHAR(30),
    ON_STREET_NAME CHAR(50),
    CROSS_STREET_NAME CHAR(50),
    OFF_STREET_NAME CHAR(50),
    NUMBER_OF_PERSONS_INJURED NUMERIC(10),
    NUMBER_OF_PERSONS_KILLED NUMERIC(10),
    NUMBER_OF_PEDESTRIANS_INJURED NUMERIC(10),
    NUMBER_OF_PEDESTRIANS_KILLED NUMERIC(10),
    NUMBER_OF_CYCLIST_INJURED NUMERIC(10),
    NUMBER_OF_CYCLIST_KILLED NUMERIC(10),
    NUMBER_OF_MOTORIST_INJURED NUMERIC(10),
    NUMBER_OF_MOTORIST_KILLED NUMERIC(10),
    CONTRIBUTING_FACTOR_VEHICLE_1 CHAR(100),
    CONTRIBUTING_FACTOR_VEHICLE_2 CHAR(100),
    CONTRIBUTING_FACTOR_VEHICLE_3 CHAR(100),
    CONTRIBUTING_FACTOR_VEHICLE_4 CHAR(100),
    CONTRIBUTING_FACTOR_VEHICLE_5 CHAR(100),
    COLLISION_ID NUMERIC(10) NOT NULL,
    VEHICLE_TYPE_CODE_1 CHAR(100),
    VEHICLE_TYPE_CODE_2 CHAR(100),
    VEHICLE_TYPE_CODE_3 CHAR(100),
    VEHICLE_TYPE_CODE_4 CHAR(100),
    VEHICLE_TYPE_CODE_5 CHAR(100)
)
```

     * postgresql://student@/Final_Project
    Done.
    Done.





    []



The simplest way to insert the data into one table instead of each dimensional table. This also allowed for us to easily clean the data, by having one base table then start removing null values shown later in the notebook. 


```sql
%%sql
SELECT * from Crashes
Limit 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
            <th>contributing_factor_key</th>
            <th>vehicle_type_key</th>
            <th>crash_time_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>167</td>
            <td>38</td>
            <td>1108</td>
            <td>125468</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>217</td>
            <td>38</td>
            <td>1108</td>
            <td>102912</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>12:54:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487052</td>
            <td>Sedan                                                                                               </td>
            <td>174</td>
            <td>38</td>
            <td>1108</td>
            <td>204230</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>8</td>
            <td>16</td>
            <td>1108</td>
            <td>260466</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>69</td>
            <td>34</td>
            <td>1108</td>
            <td>320759</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:15:00</td>
            <td>BROOKLYN            </td>
            <td>11211</td>
            <td>1</td>
            <td>0</td>
            <td>Passing or Lane Usage Improper                                                                      </td>
            <td>4486556</td>
            <td>Bus                                                                                                 </td>
            <td>145</td>
            <td>21</td>
            <td>1007</td>
            <td>352811</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>201</td>
            <td>15</td>
            <td>1108</td>
            <td>456487</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
            <td>59</td>
            <td>56</td>
            <td>1108</td>
            <td>493185</td>
        </tr>
        <tr>
            <td>2022-03-21</td>
            <td>12:05:00</td>
            <td>MANHATTAN           </td>
            <td>10018</td>
            <td>1</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4514237</td>
            <td>Motorcycle                                                                                          </td>
            <td>120</td>
            <td>32</td>
            <td>582</td>
            <td>210587</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>05:13:00</td>
            <td>QUEENS              </td>
            <td>11418</td>
            <td>0</td>
            <td>0</td>
            <td>Failure to Yield Right-of-Way                                                                       </td>
            <td>4513470</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>139</td>
            <td>61</td>
            <td>1140</td>
            <td>59664</td>
        </tr>
    </tbody>
</table>



The table above shows the top 10 rows of the dataset from the table CRASHES. This helped us to understand which columns and rows were sum columns, such as all the injured and fatalities columns were summed in the total injured and total fatalities, and that of the contributing factors, and vehicle types. (The limit of 10 was to shorten the notebook for easier reading, but can be changed to the entire dataset of 793560, as shown below).


```sql
%%sql
SELECT * from Crashes
```

     * postgresql://student@/Final_Project
    793560 rows affected.


As seen the total number of rows have now lessened to 793560 from 1231230. This strictly accounts for all the rows and columns related to the Crashes table.


```python
!pwd
```

    /home/ubuntu/notebooks/FinalProjectGroup2


**Importing Bulk Data**


```sql
%%sql
COPY Crashes FROM '/home/ubuntu/notebooks/FinalProjectGroup2/MVC_filtered.csv'
CSV
HEADER;
```

     * postgresql://student@/Final_Project
    1231229 rows affected.





    []



This code is vital to the dimensional tables, with this command the dataset MVC_filtered.csv is inserted into our base table "CRASHES". Without inserting data into the table, the codes and commands entered will read header or column headers, but will not read the data in the rows preceeding. The commands and codes truly run successfully only after inserting the data.  


```sql
%%sql
SELECT * from Crashes
LIMIT 5
```

     * postgresql://student@/Final_Project
    793560 rows affected.


Drop unnecessary columns


```sql
%%sql
ALTER TABLE Crashes
DROP COLUMN latitude,
DROP COLUMN longitude,
DROP COLUMN location,
DROP COLUMN on_street_name,
DROP COLUMN cross_street_name,
DROP COLUMN off_street_name;
```

     * postgresql://student@/Final_Project
    Done.





    []




```sql
%%sql
ALTER TABLE Crashes
DROP COLUMN number_of_pedestrians_injured,
DROP COLUMN number_of_pedestrians_killed,
DROP COLUMN number_of_cyclist_injured,
DROP COLUMN number_of_cyclist_killed, 
DROP COLUMN number_of_motorist_injured,
DROP COLUMN number_of_motorist_killed;
```

     * postgresql://student@/Final_Project
    Done.





    []




```sql
%%sql
ALTER TABLE Crashes
DROP COLUMN contributing_factor_vehicle_2,
DROP COLUMN contributing_factor_vehicle_3,
DROP COLUMN contributing_factor_vehicle_4,
DROP COLUMN contributing_factor_vehicle_5, 
DROP COLUMN vehicle_type_code_2,
DROP COLUMN vehicle_type_code_3,
DROP COLUMN vehicle_type_code_4,
DROP COLUMN vehicle_type_code_5;
```

     * postgresql://student@/Final_Project
    Done.





    []



The above three cells drop command tells the table to remove the following columns: latitude, longitude, location, on_street_name, cross_street_name, off_street_name, number_of_pedestrians_injured, number_of_pedestrians_killed, number_of_cyclist_injured, number_of_cyclist_killed, number_of_motorist_injured, number_of_motorist_killed. However, we specifically did so for the following reasons. Regarding the location values is null and the zip code/ Borough provides us enough information geographically to set a dimensional table "Geography". The number of pedestrians, cyclist, motorist, killed and injured were consolidated into two columns total injured and total killed respectively. For contributing factor 2,3,4,and 5, were either duplicates of contributing factor 1 or held no additional information, the same can be said about the vehicle type.  

**The biggest challenge we encountered was updating the contributing_factor_key and vehicle_type_key in the Crashes table.**

- Reason 1:
When attempting to match all five values for contributing_factor_vehicle_1 and vehicle_type_1 simultaneously, the execution time was 30 minutes.

- Reason 2:
Contributing factors 2, 3, 4, and 5, as well as vehicle types, either duplicated contributing factor 1 or provided no additional information; the same can be said about the vehicle types.

Consequently, we decided to retain only the first contributing factor and vehicle type for matching in the table. Simultaneously, we shifted our focus to the vehicle causing the crash rather than the affected vehicle.


```sql
%%sql
SELECT * from Crashes
LIMIT 5
```

     * postgresql://student@/Final_Project
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>02:39:00</td>
            <td>None</td>
            <td>None</td>
            <td>2</td>
            <td>0</td>
            <td>Aggressive Driving/Road Rage                                                                        </td>
            <td>4455765</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>11:45:00</td>
            <td>None</td>
            <td>None</td>
            <td>1</td>
            <td>0</td>
            <td>Pavement Slippery                                                                                   </td>
            <td>4513547</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2022-06-29</td>
            <td>06:55:00</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4541903</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:13:00</td>
            <td>BROOKLYN            </td>
            <td>11233</td>
            <td>0</td>
            <td>0</td>
            <td>None</td>
            <td>4486609</td>
            <td>None</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * from Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>02:39:00</td>
            <td>None</td>
            <td>None</td>
            <td>2</td>
            <td>0</td>
            <td>Aggressive Driving/Road Rage                                                                        </td>
            <td>4455765</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>11:45:00</td>
            <td>None</td>
            <td>None</td>
            <td>1</td>
            <td>0</td>
            <td>Pavement Slippery                                                                                   </td>
            <td>4513547</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2022-06-29</td>
            <td>06:55:00</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4541903</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:13:00</td>
            <td>BROOKLYN            </td>
            <td>11233</td>
            <td>0</td>
            <td>0</td>
            <td>None</td>
            <td>4486609</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-04-14</td>
            <td>12:47:00</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4407458</td>
            <td>Dump                                                                                                </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:05:00</td>
            <td>None</td>
            <td>None</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486555</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
        </tr>
    </tbody>
</table>



Delete the null values in the 'borough,' 'zip_code,' and 'contributing_factor_vehicle_1' columns. Since these columns are our main focus for analytics, any rows with null values in these columns will be dropped.


```sql
%%sql
SELECT count(*)
From Crashes 
where borough is NULL;
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
DELETE FROM Crashes
WHERE borough IS NULL;
```

     * postgresql://student@/Final_Project
    434027 rows affected.





    []




```sql
%%sql
SELECT count(*)
From Crashes 
where zip_code is NULL;
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
DELETE FROM Crashes
WHERE zip_code IS NULL;
```

     * postgresql://student@/Final_Project
    214 rows affected.





    []




```sql
%%sql
SELECT count(*)
From Crashes 
where contributing_factor_vehicle_1 is NULL;
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
DELETE FROM Crashes
WHERE contributing_factor_vehicle_1 IS NULL;
```

     * postgresql://student@/Final_Project
    3428 rows affected.





    []



The above cells is the cleaning of the data aspect. This allows the team to work with only relevant data that can improve the query and answer questions from insurance companies along with traffic agencies. 

Count the final number of data for analysis after dropping the null values.


```sql
%%sql
SELECT count(*)
From Crashes
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>793560</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * from Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:58:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486604</td>
            <td>Tanker                                                                                              </td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>20:03:00</td>
            <td>BROOKLYN            </td>
            <td>11226</td>
            <td>4</td>
            <td>0</td>
            <td>Steering Failure                                                                                    </td>
            <td>4486991</td>
            <td>Sedan                                                                                               </td>
        </tr>
        <tr>
            <td>2021-12-11</td>
            <td>19:43:00</td>
            <td>BRONX               </td>
            <td>10463</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487040</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
        </tr>
        <tr>
            <td>2021-12-11</td>
            <td>04:45:00</td>
            <td>MANHATTAN           </td>
            <td>10001</td>
            <td>0</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4486905</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
        </tr>
    </tbody>
</table>



The above shows the top 10 rows of data which allows us to review if the nulls have been removed, and if we were to run a query what to expect as output.


```python
!pwd
```

    /home/ubuntu/notebooks/FinalProjectGroup2


**We start with the Schema from this raw data with all the columns**


```python
from IPython.display import Image

image_url = 'https://i.ibb.co/z2SJWcJ/Whats-App-Image-2023-12-09-at-11-40-33-AM.jpg'

Image(url=image_url)
```




<img src="https://i.ibb.co/z2SJWcJ/Whats-App-Image-2023-12-09-at-11-40-33-AM.jpg"/>



**We eventually build a star-schema like this:**


```python
image_url = 'https://i.ibb.co/ZhkWn2k/Dimensions.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/ZhkWn2k/Dimensions.jpg"/>



**Work on Geography dimension, modify fact table and link them together**


```sql
%%sql
DROP TABLE IF EXISTS Geography;

CREATE TABLE Geography ( 
    key SERIAL PRIMARY KEY, 
    borough CHAR(20),
    zip_code NUMERIC(10)
);
```

     * postgresql://student@/Final_Project
    Done.
    Done.





    []



The first step to creating a dimensional table, we must use the create table command and create the primary key with the columns with their data value/limit. 


```sql
%%sql
INSERT INTO Geography (borough,zip_code) 
SELECT DISTINCT borough,zip_code
FROM Crashes;
```

     * postgresql://student@/Final_Project
    234 rows affected.





    []



Now to ensure that we have data in the table, we'll insert the borough and zip code columns from our base table "Crashes".


```sql
%%sql
SELECT * From Geography
Order By key
limit 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>borough</th>
            <th>zip_code</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>BRONX               </td>
            <td>10465</td>
        </tr>
        <tr>
            <td>2</td>
            <td>BROOKLYN            </td>
            <td>11226</td>
        </tr>
        <tr>
            <td>3</td>
            <td>QUEENS              </td>
            <td>11416</td>
        </tr>
        <tr>
            <td>4</td>
            <td>MANHATTAN           </td>
            <td>10169</td>
        </tr>
        <tr>
            <td>5</td>
            <td>BRONX               </td>
            <td>10464</td>
        </tr>
        <tr>
            <td>6</td>
            <td>STATEN ISLAND       </td>
            <td>10305</td>
        </tr>
        <tr>
            <td>7</td>
            <td>MANHATTAN           </td>
            <td>10032</td>
        </tr>
        <tr>
            <td>8</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
        </tr>
        <tr>
            <td>9</td>
            <td>BROOKLYN            </td>
            <td>11215</td>
        </tr>
        <tr>
            <td>10</td>
            <td>MANHATTAN           </td>
            <td>10281</td>
        </tr>
    </tbody>
</table>



Just checking if the Geography table functions correctly.


```sql
%%sql
SELECT count(*) From Geography
Where borough = 'MANHATTAN';
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>88</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT count(*) From Geography
Where borough = 'QUEENS'
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>67</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
ALTER TABLE Crashes
ADD COLUMN geography_key INTEGER, 
ADD CONSTRAINT fk_geography
FOREIGN KEY (geography_key) REFERENCES Geography (key);
```

     * postgresql://student@/Final_Project
    Done.





    []



After effectively verfying that the table has data, we can add the contraint key.


```sql
%%sql
SELECT * from Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:58:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486604</td>
            <td>Tanker                                                                                              </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>20:03:00</td>
            <td>BROOKLYN            </td>
            <td>11226</td>
            <td>4</td>
            <td>0</td>
            <td>Steering Failure                                                                                    </td>
            <td>4486991</td>
            <td>Sedan                                                                                               </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-11</td>
            <td>19:43:00</td>
            <td>BRONX               </td>
            <td>10463</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487040</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-11</td>
            <td>04:45:00</td>
            <td>MANHATTAN           </td>
            <td>10001</td>
            <td>0</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4486905</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>None</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * from Geography
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>borough</th>
            <th>zip_code</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>BRONX               </td>
            <td>10465</td>
        </tr>
        <tr>
            <td>2</td>
            <td>BROOKLYN            </td>
            <td>11226</td>
        </tr>
        <tr>
            <td>3</td>
            <td>QUEENS              </td>
            <td>11416</td>
        </tr>
        <tr>
            <td>4</td>
            <td>MANHATTAN           </td>
            <td>10169</td>
        </tr>
        <tr>
            <td>5</td>
            <td>BRONX               </td>
            <td>10464</td>
        </tr>
        <tr>
            <td>6</td>
            <td>STATEN ISLAND       </td>
            <td>10305</td>
        </tr>
        <tr>
            <td>7</td>
            <td>MANHATTAN           </td>
            <td>10032</td>
        </tr>
        <tr>
            <td>8</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
        </tr>
        <tr>
            <td>9</td>
            <td>BROOKLYN            </td>
            <td>11215</td>
        </tr>
        <tr>
            <td>10</td>
            <td>MANHATTAN           </td>
            <td>10281</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
UPDATE Crashes
SET geography_key = Geography.key
FROM Geography
WHERE Crashes.borough = Geography.borough
    AND Crashes.zip_code = Geography.zip_code;
```

     * postgresql://student@/Final_Project
    793560 rows affected.





    []



With the constraint key created we need to update the 'Crashes' table by having the 'Crashes' table columns: 'borough' and 'zip_code' match that of 'Geographys' columns: 'borough' and 'zip_code' values. 


```sql
%%sql
SELECT * FROM Crashes 
LIMIT 5;
```

     * postgresql://student@/Final_Project
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>167</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>217</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>201</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>8</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>69</td>
        </tr>
    </tbody>
</table>



Check there are no null values in the 'geography_key' column.


```sql
%%sql 
SELECT count(*) From Crashes
Where geography_key is NULL
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>



We check that there are no null values in the 'Crashes' table in the 'geography_key'.


```sql
%%sql
SELECT count(DISTINCT geography_key)
FROM Crashes
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>234</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT count(key)
FROM Geography
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>234</td>
        </tr>
    </tbody>
</table>



Check the distinct 'geography_key' count in both the fact table (Crashes) and the dimensional table (Geography). They have the same count, which is 234. Thus, we ensure that the fact table contains the right number of records.

**Work on Contributing_factor dimension table, modify fact table and link them together**


```sql
%%sql
DROP TABLE IF EXISTS Contributing_factor CASCADE;

CREATE TABLE Contributing_factor ( 
    key SERIAL PRIMARY KEY, 
    contributing_factor_vehicle_1 CHAR(100)
);
```

     * postgresql://student@/Final_Project
    Done.
    Done.





    []



Next is to create the second dimensional table "Contributing_factor", this dimensional table will have one primary key. 

CASCADE is often used with foreign keys and update delete commands. Meaning that the changes to the referenced table will automatically be applied to the referencing table. In other words this creates a parent-child relationship. 


```sql
%%sql
INSERT INTO Contributing_factor (contributing_factor_vehicle_1) 
SELECT DISTINCT contributing_factor_vehicle_1
FROM Crashes;
```

     * postgresql://student@/Final_Project
    61 rows affected.





    []



Again, we will need to insert the contributing factors from the parent table "Crashes" to the child table 'Contributing_factors'. We'll then want to validate whether or not the insertion of data was done correctly. 


```sql
%%sql
SELECT * From Contributing_factor
Order By Key
limit 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>contributing_factor_vehicle_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Animals Action                                                                                      </td>
        </tr>
        <tr>
            <td>2</td>
            <td>Steering Failure                                                                                    </td>
        </tr>
        <tr>
            <td>3</td>
            <td>Driver Inattention/Distraction                                                                      </td>
        </tr>
        <tr>
            <td>4</td>
            <td>Accelerator Defective                                                                               </td>
        </tr>
        <tr>
            <td>5</td>
            <td>Tire Failure/Inadequate                                                                             </td>
        </tr>
        <tr>
            <td>6</td>
            <td>Brakes Defective                                                                                    </td>
        </tr>
        <tr>
            <td>7</td>
            <td>Traffic Control Device Improper/Non-Working                                                         </td>
        </tr>
        <tr>
            <td>8</td>
            <td>Pedestrian/Bicyclist/Other Pedestrian Error/Confusion                                               </td>
        </tr>
        <tr>
            <td>9</td>
            <td>Illness                                                                                             </td>
        </tr>
        <tr>
            <td>10</td>
            <td>Failure to Keep Right                                                                               </td>
        </tr>
    </tbody>
</table>



Next, step is to create the constraint key. 


```sql
%%sql
ALTER TABLE Crashes
ADD COLUMN contributing_factor_key INTEGER, 
ADD CONSTRAINT fk_contributing_factor
FOREIGN KEY (contributing_factor_key) REFERENCES Contributing_factor (key);
```

     * postgresql://student@/Final_Project
    Done.





    []




```sql
%%sql
SELECT * from Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
            <th>contributing_factor_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>167</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>217</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>201</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>8</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>69</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
            <td>59</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>12:54:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487052</td>
            <td>Sedan                                                                                               </td>
            <td>174</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:15:00</td>
            <td>BROOKLYN            </td>
            <td>11211</td>
            <td>1</td>
            <td>0</td>
            <td>Passing or Lane Usage Improper                                                                      </td>
            <td>4486556</td>
            <td>Bus                                                                                                 </td>
            <td>145</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:02:00</td>
            <td>QUEENS              </td>
            <td>11373</td>
            <td>1</td>
            <td>0</td>
            <td>Lost Consciousness                                                                                  </td>
            <td>4513794</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>220</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:05:00</td>
            <td>QUEENS              </td>
            <td>11411</td>
            <td>1</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4514267</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>98</td>
            <td>None</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * from Contributing_factor
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>contributing_factor_vehicle_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Animals Action                                                                                      </td>
        </tr>
        <tr>
            <td>2</td>
            <td>Steering Failure                                                                                    </td>
        </tr>
        <tr>
            <td>3</td>
            <td>Driver Inattention/Distraction                                                                      </td>
        </tr>
        <tr>
            <td>4</td>
            <td>Accelerator Defective                                                                               </td>
        </tr>
        <tr>
            <td>5</td>
            <td>Tire Failure/Inadequate                                                                             </td>
        </tr>
        <tr>
            <td>6</td>
            <td>Brakes Defective                                                                                    </td>
        </tr>
        <tr>
            <td>7</td>
            <td>Traffic Control Device Improper/Non-Working                                                         </td>
        </tr>
        <tr>
            <td>8</td>
            <td>Pedestrian/Bicyclist/Other Pedestrian Error/Confusion                                               </td>
        </tr>
        <tr>
            <td>9</td>
            <td>Illness                                                                                             </td>
        </tr>
        <tr>
            <td>10</td>
            <td>Failure to Keep Right                                                                               </td>
        </tr>
    </tbody>
</table>




```sql
%%sql
UPDATE Crashes
SET contributing_factor_key = Contributing_factor.key
FROM Contributing_factor
WHERE 
    (Crashes.contributing_factor_vehicle_1 = Contributing_factor.contributing_factor_vehicle_1 OR
     (Crashes.contributing_factor_vehicle_1 IS NULL AND Contributing_factor.contributing_factor_vehicle_1 IS NULL))
```

     * postgresql://student@/Final_Project
    793560 rows affected.





    []



This will insert the contraint key 'Contributing_factor.key' into the 'Crashes' table. This allows the exact values to be copied over. 


```sql
%%sql
SELECT count(*)
FROM Crashes
WHERE contributing_factor_key IS NULL
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>



Check there are no null values in the 'contributing_factor_key' column.


```sql
%%sql
SELECT count(DISTINCT contributing_factor_key)
FROM Crashes
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>61</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT count(key)
FROM Contributing_factor
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>61</td>
        </tr>
    </tbody>
</table>



Check the distinct 'contributing_factor_key' count in both the fact table (Crashes) and the dimensional table (Contributing_factor). They have the same count, which is 61. Thus, we ensure that the fact table contains the right number of records.

**Work on Vehicle_type dimension table, modify fact table and link them together**


```sql
%%sql
DROP TABLE IF EXISTS Vehicle_type CASCADE;

CREATE TABLE Vehicle_type ( 
    key SERIAL PRIMARY KEY, 
    vehicle_type_code_1 CHAR(100)
);
```

     * postgresql://student@/Final_Project
    Done.
    Done.





    []



Now, we create the third dimensional table 'Vehicle_type': this dimensional table will have one primary key. 

CASCADE is often used with foreign keys and to update delete commands. Meaning if the changes to the referenced table will automatically be applied to the referencing table. In other words this creates a parent-child relationship.


```sql
%%sql
INSERT INTO Vehicle_type (vehicle_type_code_1) 
SELECT DISTINCT vehicle_type_code_1
FROM Crashes;
```

     * postgresql://student@/Final_Project
    1145 rows affected.





    []



Again, we will need to insert the vehicle type from the parent table 'Crashes' to the child table 'Vehicle_type'. We'll then want to validate whether or not the insertion of data was done correctly.


```sql
%%sql
SELECT * From Vehicle_type
Order By Key
limit 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>vehicle_type_code_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>SANTI                                                                                               </td>
        </tr>
        <tr>
            <td>2</td>
            <td>Tow t                                                                                               </td>
        </tr>
        <tr>
            <td>3</td>
            <td>commercial                                                                                          </td>
        </tr>
        <tr>
            <td>4</td>
            <td>HORSE CARR                                                                                          </td>
        </tr>
        <tr>
            <td>5</td>
            <td>FDNY Ambul                                                                                          </td>
        </tr>
        <tr>
            <td>6</td>
            <td>dumps                                                                                               </td>
        </tr>
        <tr>
            <td>7</td>
            <td>Sprinter W                                                                                          </td>
        </tr>
        <tr>
            <td>8</td>
            <td>jeep                                                                                                </td>
        </tr>
        <tr>
            <td>9</td>
            <td>Power shov                                                                                          </td>
        </tr>
        <tr>
            <td>10</td>
            <td>CONTR                                                                                               </td>
        </tr>
    </tbody>
</table>




```sql
%%sql
ALTER TABLE Crashes
ADD COLUMN vehicle_type_key INTEGER, 
ADD CONSTRAINT fk_vehicle_type
FOREIGN KEY (vehicle_type_key) REFERENCES Vehicle_type (key);
```

     * postgresql://student@/Final_Project
    Done.





    []




```sql
%%sql
SELECT * from Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
            <th>contributing_factor_key</th>
            <th>vehicle_type_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>167</td>
            <td>38</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>217</td>
            <td>38</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>201</td>
            <td>15</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>8</td>
            <td>16</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>69</td>
            <td>34</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
            <td>59</td>
            <td>56</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>12:54:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487052</td>
            <td>Sedan                                                                                               </td>
            <td>174</td>
            <td>38</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:15:00</td>
            <td>BROOKLYN            </td>
            <td>11211</td>
            <td>1</td>
            <td>0</td>
            <td>Passing or Lane Usage Improper                                                                      </td>
            <td>4486556</td>
            <td>Bus                                                                                                 </td>
            <td>145</td>
            <td>21</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:02:00</td>
            <td>QUEENS              </td>
            <td>11373</td>
            <td>1</td>
            <td>0</td>
            <td>Lost Consciousness                                                                                  </td>
            <td>4513794</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>220</td>
            <td>46</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:05:00</td>
            <td>QUEENS              </td>
            <td>11411</td>
            <td>1</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4514267</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>98</td>
            <td>32</td>
            <td>None</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * from Vehicle_type
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>vehicle_type_code_1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>SANTI                                                                                               </td>
        </tr>
        <tr>
            <td>2</td>
            <td>Tow t                                                                                               </td>
        </tr>
        <tr>
            <td>3</td>
            <td>commercial                                                                                          </td>
        </tr>
        <tr>
            <td>4</td>
            <td>HORSE CARR                                                                                          </td>
        </tr>
        <tr>
            <td>5</td>
            <td>FDNY Ambul                                                                                          </td>
        </tr>
        <tr>
            <td>6</td>
            <td>dumps                                                                                               </td>
        </tr>
        <tr>
            <td>7</td>
            <td>Sprinter W                                                                                          </td>
        </tr>
        <tr>
            <td>8</td>
            <td>jeep                                                                                                </td>
        </tr>
        <tr>
            <td>9</td>
            <td>Power shov                                                                                          </td>
        </tr>
        <tr>
            <td>10</td>
            <td>CONTR                                                                                               </td>
        </tr>
    </tbody>
</table>



Note: This code will take around 3-5 min to run.


```sql
%%sql 
UPDATE Crashes
SET vehicle_type_key = Vehicle_type.key
FROM Vehicle_type
WHERE 
    (Crashes.vehicle_type_code_1 = Vehicle_type.vehicle_type_code_1 OR
     (Crashes.vehicle_type_code_1 IS NULL AND Vehicle_type.vehicle_type_code_1 IS NULL));
```

     * postgresql://student@/Final_Project
    793560 rows affected.





    []



This will insert the contraint key 'Vehicle_type.key' into the 'Crashes' table. This allows the exact vales to be copied over.


```sql
%%sql
SELECT * FROM Crashes 
LIMIT 10;
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
            <th>contributing_factor_key</th>
            <th>vehicle_type_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>167</td>
            <td>38</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>217</td>
            <td>38</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>201</td>
            <td>15</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>8</td>
            <td>16</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>69</td>
            <td>34</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
            <td>59</td>
            <td>56</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>12:54:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487052</td>
            <td>Sedan                                                                                               </td>
            <td>174</td>
            <td>38</td>
            <td>1108</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:15:00</td>
            <td>BROOKLYN            </td>
            <td>11211</td>
            <td>1</td>
            <td>0</td>
            <td>Passing or Lane Usage Improper                                                                      </td>
            <td>4486556</td>
            <td>Bus                                                                                                 </td>
            <td>145</td>
            <td>21</td>
            <td>1007</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:02:00</td>
            <td>QUEENS              </td>
            <td>11373</td>
            <td>1</td>
            <td>0</td>
            <td>Lost Consciousness                                                                                  </td>
            <td>4513794</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>220</td>
            <td>46</td>
            <td>1140</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:05:00</td>
            <td>QUEENS              </td>
            <td>11411</td>
            <td>1</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4514267</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>98</td>
            <td>32</td>
            <td>1140</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT count(*) FROM Crashes 
Where vehicle_type_key is null
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>



Check there are no null values in the 'vehicle_type_key' column.


```sql
%%sql
SELECT count(DISTINCT vehicle_type_key)
FROM Crashes
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1145</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT count(key)
FROM Vehicle_type
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1145</td>
        </tr>
    </tbody>
</table>



Check the distinct 'vehicle_type_key' count in both the fact table (Crashes) and the dimensional table (Vehicle_type). They have the same count, which is 1145. Thus, we ensure that the fact table contains the right number of records.

**Work on Crash_time dimension table, modify fact table and link them together**


```sql
%%sql
DROP TABLE IF EXISTS Crash_time CASCADE;

CREATE TABLE Crash_time ( 
    key SERIAL PRIMARY KEY, 
    hour INT,
    day INT,
    year INT,
    month_of_year INT, 
    day_of_month INT, 
    day_of_week_str CHAR(9), 
    day_of_week INT, 
    is_weekend BOOLEAN, 
    is_weekday BOOLEAN, 
    hour_of_day INT, 
    quarter_of_year INT,
    combined_timestamp TIMESTAMP
);
```

     * postgresql://student@/Final_Project
    Done.
    Done.





    []



Finally, we create of last dimensional table 'Crash_time'. This table is important to our queries, this will house our weeks, and time variables.


```sql
%%sql
INSERT INTO Crash_time (hour, day, year, month_of_year, day_of_month, day_of_week_str, day_of_week, is_weekend, is_weekday, hour_of_day, quarter_of_year, combined_timestamp) 
SELECT DISTINCT 
    EXTRACT(HOUR FROM combined_timestamp) AS hour, 
    EXTRACT(DAY FROM combined_timestamp) AS day,
    EXTRACT(YEAR FROM combined_timestamp) AS year,
    EXTRACT(MONTH FROM combined_timestamp) AS month_of_year,
    EXTRACT(DAY FROM combined_timestamp) AS day_of_month,
    TO_CHAR(combined_timestamp, 'Day') AS day_of_week_str,
    EXTRACT(DOW FROM combined_timestamp) AS day_of_week,
    CASE WHEN EXTRACT(DOW FROM combined_timestamp) IN (0, 6) THEN TRUE ELSE FALSE END AS is_weekend,
    CASE WHEN EXTRACT(DOW FROM combined_timestamp) NOT IN (0,6) THEN TRUE ELSE FALSE END AS is_weekday,
    EXTRACT(HOUR FROM combined_timestamp) AS hour_of_day,
    CEIL(EXTRACT(MONTH FROM combined_timestamp) / 3.0) AS quarter_of_year,
    combined_timestamp
FROM 
    (SELECT crash_date + crash_time AS combined_timestamp FROM Crashes) AS combined;
```

     * postgresql://student@/Final_Project
    501871 rows affected.





    []



Here we are inserting the data into this table. Then, 'weekdays' to a specific numeric value and sequence so weekdays will have number 1,2,3,4,5 and weekends as 0,6. 


```sql
%%sql
SELECT DISTINCT day_of_week_str, day_of_week, is_weekend, is_weekday 
FROM Crash_time
ORDER BY day_of_week;
```

     * postgresql://student@/Final_Project
    7 rows affected.





<table>
    <thead>
        <tr>
            <th>day_of_week_str</th>
            <th>day_of_week</th>
            <th>is_weekend</th>
            <th>is_weekday</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Sunday   </td>
            <td>0</td>
            <td>True</td>
            <td>False</td>
        </tr>
        <tr>
            <td>Monday   </td>
            <td>1</td>
            <td>False</td>
            <td>True</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>2</td>
            <td>False</td>
            <td>True</td>
        </tr>
        <tr>
            <td>Wednesday</td>
            <td>3</td>
            <td>False</td>
            <td>True</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>4</td>
            <td>False</td>
            <td>True</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
        </tr>
        <tr>
            <td>Saturday </td>
            <td>6</td>
            <td>True</td>
            <td>False</td>
        </tr>
    </tbody>
</table>



This validates that our weekdays and weekends are being read correctly.


```sql
%%sql
ALTER TABLE Crashes
ADD COLUMN crash_time_key INTEGER, 
ADD CONSTRAINT fk_crash_time
FOREIGN KEY (crash_time_key) REFERENCES Crash_time (key);
```

     * postgresql://student@/Final_Project
    Done.





    []



We add 'crash_time_key' to the 'Crashes' table and add the constraint. This allows us to run the below query which shows us the values 'crash_time_key' contains. 


```sql
%%sql
SELECT * From Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>crash_date</th>
            <th>crash_time</th>
            <th>borough</th>
            <th>zip_code</th>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>contributing_factor_vehicle_1</th>
            <th>collision_id</th>
            <th>vehicle_type_code_1</th>
            <th>geography_key</th>
            <th>contributing_factor_key</th>
            <th>vehicle_type_key</th>
            <th>crash_time_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2021-09-11</td>
            <td>09:35:00</td>
            <td>BROOKLYN            </td>
            <td>11208</td>
            <td>0</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4456314</td>
            <td>Sedan                                                                                               </td>
            <td>167</td>
            <td>38</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>08:17:00</td>
            <td>BRONX               </td>
            <td>10475</td>
            <td>2</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4486660</td>
            <td>Sedan                                                                                               </td>
            <td>217</td>
            <td>38</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>21:10:00</td>
            <td>BROOKLYN            </td>
            <td>11207</td>
            <td>0</td>
            <td>0</td>
            <td>Driver Inexperience                                                                                 </td>
            <td>4487074</td>
            <td>Sedan                                                                                               </td>
            <td>201</td>
            <td>15</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>14:58:00</td>
            <td>MANHATTAN           </td>
            <td>10017</td>
            <td>0</td>
            <td>0</td>
            <td>Passing Too Closely                                                                                 </td>
            <td>4486519</td>
            <td>Sedan                                                                                               </td>
            <td>8</td>
            <td>16</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>16:50:00</td>
            <td>QUEENS              </td>
            <td>11413</td>
            <td>0</td>
            <td>0</td>
            <td>Turning Improperly                                                                                  </td>
            <td>4487127</td>
            <td>Sedan                                                                                               </td>
            <td>69</td>
            <td>34</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>23:10:00</td>
            <td>QUEENS              </td>
            <td>11434</td>
            <td>2</td>
            <td>0</td>
            <td>Reaction to Uninvolved Vehicle                                                                      </td>
            <td>4486635</td>
            <td>Sedan                                                                                               </td>
            <td>59</td>
            <td>56</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>12:54:00</td>
            <td>BROOKLYN            </td>
            <td>11217</td>
            <td>1</td>
            <td>0</td>
            <td>Unspecified                                                                                         </td>
            <td>4487052</td>
            <td>Sedan                                                                                               </td>
            <td>174</td>
            <td>38</td>
            <td>1108</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2021-12-14</td>
            <td>17:15:00</td>
            <td>BROOKLYN            </td>
            <td>11211</td>
            <td>1</td>
            <td>0</td>
            <td>Passing or Lane Usage Improper                                                                      </td>
            <td>4486556</td>
            <td>Bus                                                                                                 </td>
            <td>145</td>
            <td>21</td>
            <td>1007</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:02:00</td>
            <td>QUEENS              </td>
            <td>11373</td>
            <td>1</td>
            <td>0</td>
            <td>Lost Consciousness                                                                                  </td>
            <td>4513794</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>220</td>
            <td>46</td>
            <td>1140</td>
            <td>None</td>
        </tr>
        <tr>
            <td>2022-03-26</td>
            <td>16:05:00</td>
            <td>QUEENS              </td>
            <td>11411</td>
            <td>1</td>
            <td>0</td>
            <td>Following Too Closely                                                                               </td>
            <td>4514267</td>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>98</td>
            <td>32</td>
            <td>1140</td>
            <td>None</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT * from Crash_time
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>key</th>
            <th>hour</th>
            <th>day</th>
            <th>year</th>
            <th>month_of_year</th>
            <th>day_of_month</th>
            <th>day_of_week_str</th>
            <th>day_of_week</th>
            <th>is_weekend</th>
            <th>is_weekday</th>
            <th>hour_of_day</th>
            <th>quarter_of_year</th>
            <th>combined_timestamp</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:01:00</td>
        </tr>
        <tr>
            <td>2</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:05:00</td>
        </tr>
        <tr>
            <td>3</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:15:00</td>
        </tr>
        <tr>
            <td>4</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:23:00</td>
        </tr>
        <tr>
            <td>5</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:25:00</td>
        </tr>
        <tr>
            <td>6</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:33:00</td>
        </tr>
        <tr>
            <td>7</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:50:00</td>
        </tr>
        <tr>
            <td>8</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>1</td>
            <td>1</td>
            <td>Friday   </td>
            <td>5</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-01-01 00:51:00</td>
        </tr>
        <tr>
            <td>9</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>2</td>
            <td>1</td>
            <td>Monday   </td>
            <td>1</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-02-01 00:01:00</td>
        </tr>
        <tr>
            <td>10</td>
            <td>0</td>
            <td>1</td>
            <td>2016</td>
            <td>2</td>
            <td>1</td>
            <td>Monday   </td>
            <td>1</td>
            <td>False</td>
            <td>True</td>
            <td>0</td>
            <td>1</td>
            <td>2016-02-01 00:05:00</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
UPDATE Crashes
SET crash_time_key = Crash_time.key
FROM Crash_time
WHERE Crashes.crash_date + Crashes.crash_time = Crash_time.combined_timestamp;
```

     * postgresql://student@/Final_Project
    793560 rows affected.





    []



This will insert the contraint key 'Crash_time.key' into the 'Crashes' table. This allows the exact vales to be copied over. 

We build another contraint key 'Crash_time.combined_timestamp'. This will combine both the date and time into one column in the 'Crashes' table.


```sql
%%sql
SELECT count(*) From Crashes
Where crash_time_key is NULL
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
        </tr>
    </tbody>
</table>



Check there are no null values in the 'crash_time_key' column.


```sql
%%sql
SELECT count(DISTINCT crash_time_key)
FROM Crashes
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>501871</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
SELECT count(key)
FROM Crash_time
```

     * postgresql://student@/Final_Project
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>501871</td>
        </tr>
    </tbody>
</table>



Check the distinct 'crash_time_key' count in both the fact table (Crashes) and the dimensional table (Crash_time). They have the same count, which is 501871. Thus, we ensure that the fact table contains the right number of records.


```sql
%%sql
ALTER TABLE Crashes
DROP COLUMN crash_date,
DROP COLUMN crash_time,
DROP COLUMN borough,
DROP COLUMN zip_code,
DROP COLUMN contributing_factor_vehicle_1,
DROP COLUMN vehicle_type_code_1
```

     * postgresql://student@/Final_Project
    Done.





    []



Drop the original and unnecessary columns in the fact table.

**Look at the final fact table.**


```sql
%%sql
SELECT * From Crashes
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>number_of_persons_injured</th>
            <th>number_of_persons_killed</th>
            <th>collision_id</th>
            <th>geography_key</th>
            <th>contributing_factor_key</th>
            <th>vehicle_type_key</th>
            <th>crash_time_key</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3363638</td>
            <td>89</td>
            <td>9</td>
            <td>826</td>
            <td>549</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3364381</td>
            <td>56</td>
            <td>38</td>
            <td>207</td>
            <td>142114</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3364154</td>
            <td>1</td>
            <td>38</td>
            <td>207</td>
            <td>61933</td>
        </tr>
        <tr>
            <td>1</td>
            <td>0</td>
            <td>3364847</td>
            <td>49</td>
            <td>61</td>
            <td>207</td>
            <td>118146</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3364309</td>
            <td>88</td>
            <td>38</td>
            <td>804</td>
            <td>308219</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3364599</td>
            <td>209</td>
            <td>54</td>
            <td>883</td>
            <td>221645</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3365018</td>
            <td>215</td>
            <td>61</td>
            <td>207</td>
            <td>341302</td>
        </tr>
        <tr>
            <td>1</td>
            <td>0</td>
            <td>3365321</td>
            <td>212</td>
            <td>57</td>
            <td>826</td>
            <td>449301</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3592792</td>
            <td>119</td>
            <td>3</td>
            <td>1108</td>
            <td>62810</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>3365546</td>
            <td>200</td>
            <td>3</td>
            <td>826</td>
            <td>76390</td>
        </tr>
    </tbody>
</table>



### Ask 3: Data Analysis

We are a Consultation firm providing an overview of the New York City traffic incident trends from 2016-2022.


```python
image_url = 'https://i.ibb.co/FHPM3BG/Number-Of-Incidents.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/FHPM3BG/Number-Of-Incidents.jpg"/>



We can see that this is the total number of incidents from 2016- 2022. The lowest number of incidents is in 2020 (April) and we can see that the number of incidents has decreased significantly post 2020, which could possibly be because of post CoViD circumstances (e,g; more people choosing to Work from Home).


```python
image_url = 'https://i.ibb.co/6wbprvL/Total-Persons-Injured.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/6wbprvL/Total-Persons-Injured.jpg"/>



- The trend shows that even though the total number of incidents has decreased, we can see that the trend in the number of injuries is not similar (decreased).  



```python
image_url = 'https://i.ibb.co/p0LMkNC/Total-Persons-Killed.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/p0LMkNC/Total-Persons-Killed.jpg"/>



- The trend shows that even though the total number of incidents has decreased, we can see that the trend in the number of people killed remains the same. 

The following queries are intended for data extraction to generate the overall trend graphs above.


```python
!pwd
```

    /home/ubuntu/notebooks/FinalProjectGroup2



```python
!sudo chmod a+w /home/ubuntu/notebooks/Final\ Project
```


```sql
%%sql
COPY (
    SELECT
        Crash_time.year, 
        Crash_time.month_of_year,
        Crash_time.day_of_week_str,
        Geography.borough,
        count(Crashes.collision_id) AS number_of_incidents,
        SUM(Crashes.number_of_persons_killed) AS total_persons_killed,
        SUM(Crashes.number_of_persons_injured) AS total_persons_injured    
    FROM Crashes
    JOIN Crash_time ON Crashes.crash_time_key = Crash_time.key
    JOIN Geography ON Crashes.geography_key = Geography.key
    GROUP BY Crash_time.year, Crash_time.month_of_year, Crash_time.day_of_week_str, Geography.borough
    ORDER BY Crash_time.year, Crash_time.month_of_year, Crash_time.day_of_week_str, Geography.borough, number_of_incidents
) TO '/home/ubuntu/notebooks/output.csv' WITH CSV HEADER;
```


```sql
%%sql
COPY (
    SELECT
    Contributing_factor.contributing_factor_vehicle_1,
    SUM(Crashes.number_of_persons_killed) AS total_persons_killed,
    SUM(Crashes.number_of_persons_injured) AS total_persons_injured
FROM
    Crashes
JOIN
    Contributing_factor ON Crashes.contributing_factor_key = Contributing_factor.key
GROUP BY
    Contributing_factor.contributing_factor_vehicle_1
ORDER BY
   total_persons_killed DESC, total_persons_injured DESC
) TO '/home/ubuntu/notebooks/output1.csv' WITH CSV HEADER;
```

#### Analysis 1 for Car Insurance:

Examine the boroughs with the highest frequency of  traffic crashes and the vehicles involved in the highest  number of accidents focusing on developing  recommendations for car insurance companies by improving risk assessment models and enhancing competitiveness within the insurance market.

**First we want to know which top 10 vehicle types have the most crashes**


```sql
%%sql
SELECT Vehicle_type.vehicle_type_code_1, count(collision_id) as number_of_incidents
FROM
    Crashes 
JOIN
    Vehicle_type ON Crashes.vehicle_type_key = Vehicle_type.key
    
GROUP BY vehicle_type.vehicle_type_code_1
ORDER BY number_of_incidents desc
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>vehicle_type_code_1</th>
            <th>number_of_incidents</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Sedan                                                                                               </td>
            <td>335762</td>
        </tr>
        <tr>
            <td>Station Wagon/Sport Utility Vehicle                                                                 </td>
            <td>262104</td>
        </tr>
        <tr>
            <td>Taxi                                                                                                </td>
            <td>31858</td>
        </tr>
        <tr>
            <td>4 dr sedan                                                                                          </td>
            <td>26889</td>
        </tr>
        <tr>
            <td>Pick-up Truck                                                                                       </td>
            <td>20100</td>
        </tr>
        <tr>
            <td>PASSENGER VEHICLE                                                                                   </td>
            <td>15575</td>
        </tr>
        <tr>
            <td>Box Truck                                                                                           </td>
            <td>15067</td>
        </tr>
        <tr>
            <td>Bus                                                                                                 </td>
            <td>13226</td>
        </tr>
        <tr>
            <td>Bike                                                                                                </td>
            <td>8884</td>
        </tr>
        <tr>
            <td>SPORT UTILITY / STATION WAGON                                                                       </td>
            <td>8230</td>
        </tr>
    </tbody>
</table>



As it shows that there are some duplications in Vehicle type, due to the fact that no standards are followed by the people who did data entry. This results in the problem that each person enters the data differently, some will say Sedan, other will enter the car company name like 'chev' or 'ford'.

To solve this issue, we need first to identify and agree on the vehicle type classfication. Second, we need to automate the data entry so the data entry person will have a drop down list to chose the vehicle type.


 - Sedan vehicles top the list for the highest number of incidents, closely followed by Station Wagons and Sport Utility Vehicles.


```python
image_url = 'https://i.ibb.co/3FwJ2L0/Vehicle-Types.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/3FwJ2L0/Vehicle-Types.jpg"/>



**Which boroughs have the most crashes?**


```sql
%%sql
SELECT Geography.borough, 
count(collision_id) as number_of_incidents
FROM
    Crashes 
JOIN
    Geography ON Crashes.geography_key = Geography.key
    
GROUP BY Geography.borough
ORDER BY number_of_incidents desc
```

     * postgresql://student@/Final_Project
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>borough</th>
            <th>number_of_incidents</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>BROOKLYN            </td>
            <td>256981</td>
        </tr>
        <tr>
            <td>QUEENS              </td>
            <td>218871</td>
        </tr>
        <tr>
            <td>MANHATTAN           </td>
            <td>159491</td>
        </tr>
        <tr>
            <td>BRONX               </td>
            <td>127786</td>
        </tr>
        <tr>
            <td>STATEN ISLAND       </td>
            <td>30431</td>
        </tr>
    </tbody>
</table>




```python
image_url = 'https://i.ibb.co/L68CZqY/Borough.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/L68CZqY/Borough.jpg"/>



We find that 'BROOKLYN' and 'QUEENS' have the highest number of incidents

**Strategic recommendations to Car Insurance Companies:**

- We suggest raising insurance premiums for Sedans and SPORT UTILITY / STATION WAGON vehicle types, as well as for residents of BROOKLYN and QUEENS.

#### Analysis 2 for NY Traffic Police Office:

What recommendations can we provide to the New York Police Office responsible for traffic management, with the goal of reducing traffic accidents in high-risk areas?

**2-1: Which top 10 zip code areas have the highest number of persons injured due to crashes?**


```sql
%%sql
SELECT Geography.zip_code, 
SUM(Crashes.number_of_persons_injured) AS total_persons_injured
FROM
    Crashes 
JOIN
    Geography ON Crashes.geography_key = Geography.key
    
GROUP BY Geography.zip_code
ORDER BY total_persons_injured desc
LIMIT 10
```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>zip_code</th>
            <th>total_persons_injured</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>11207</td>
            <td>6060</td>
        </tr>
        <tr>
            <td>11236</td>
            <td>4998</td>
        </tr>
        <tr>
            <td>11203</td>
            <td>4522</td>
        </tr>
        <tr>
            <td>11212</td>
            <td>4380</td>
        </tr>
        <tr>
            <td>11234</td>
            <td>3804</td>
        </tr>
        <tr>
            <td>11208</td>
            <td>3787</td>
        </tr>
        <tr>
            <td>11226</td>
            <td>3782</td>
        </tr>
        <tr>
            <td>11434</td>
            <td>3545</td>
        </tr>
        <tr>
            <td>11233</td>
            <td>3111</td>
        </tr>
        <tr>
            <td>11385</td>
            <td>3049</td>
        </tr>
    </tbody>
</table>




```python
image_url = 'https://i.ibb.co/XLX9Kfj/Zip-Code-Map.jpg'

Image(url=image_url)
```




<img src="https://i.ibb.co/XLX9Kfj/Zip-Code-Map.jpg"/>



We use this website, https://maps.huge.info/zip.htm, to pinpoint the top 10 high-risk zip codes. The pinned locations are as follows: Pinned A: 11207, Pinned B: 11236, Pinned C: 11203, Pinned D: 11212, Pinned E: 11234, Pinned F: 11208, Pinned G: 11226, Pinned H: 11434, Pinned I: 11233, and Pinned J: 11385.

We recommend that the local police office initiate research to identify specific intersections within the identified high-risk zip codes (11207, 11236, 11203, 11212, 11234, 11208, 11226, 11434, 11208, 11226, 11434, 11233, 11385) and implement safety measures such as improved signage, road markings, and pedestrian crossings. Simultaneously, we suggest conducting traffic awareness campaigns in these areas to educate both drivers and pedestrians about safe practices.

**2-2: What combination of day of the week, hour of the day, and zip code experienced the top 30 highest number of persons injured in crashes?**


```sql
%%sql
SELECT DISTINCT
    Crash_time.day_of_week_str,
    Crash_time.hour_of_day,
    Geography.zip_code,
    SUM(Crashes.number_of_persons_injured) AS total_persons_injured
FROM
    Crashes
JOIN
    Crash_time ON Crashes.crash_time_key = Crash_time.key
JOIN
    Geography ON Crashes.geography_key = Geography.key
GROUP BY
    Crash_time.day_of_week_str, Crash_time.hour_of_day, Geography.zip_code
ORDER BY
    total_persons_injured DESC
limit 30
```

     * postgresql://student@/Final_Project
    30 rows affected.





<table>
    <thead>
        <tr>
            <th>day_of_week_str</th>
            <th>hour_of_day</th>
            <th>zip_code</th>
            <th>total_persons_injured</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Friday   </td>
            <td>17</td>
            <td>11207</td>
            <td>96</td>
        </tr>
        <tr>
            <td>Sunday   </td>
            <td>0</td>
            <td>11207</td>
            <td>75</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>16</td>
            <td>11207</td>
            <td>72</td>
        </tr>
        <tr>
            <td>Saturday </td>
            <td>22</td>
            <td>11207</td>
            <td>69</td>
        </tr>
        <tr>
            <td>Sunday   </td>
            <td>15</td>
            <td>11207</td>
            <td>68</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>17</td>
            <td>11234</td>
            <td>66</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>18</td>
            <td>11207</td>
            <td>66</td>
        </tr>
        <tr>
            <td>Saturday </td>
            <td>21</td>
            <td>11236</td>
            <td>65</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>17</td>
            <td>11207</td>
            <td>65</td>
        </tr>
        <tr>
            <td>Saturday </td>
            <td>15</td>
            <td>11207</td>
            <td>64</td>
        </tr>
        <tr>
            <td>Saturday </td>
            <td>18</td>
            <td>11236</td>
            <td>63</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>17</td>
            <td>11434</td>
            <td>63</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>14</td>
            <td>11207</td>
            <td>62</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>16</td>
            <td>11207</td>
            <td>61</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>20</td>
            <td>11207</td>
            <td>61</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>20</td>
            <td>11236</td>
            <td>61</td>
        </tr>
        <tr>
            <td>Sunday   </td>
            <td>0</td>
            <td>11236</td>
            <td>61</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>17</td>
            <td>10451</td>
            <td>61</td>
        </tr>
        <tr>
            <td>Wednesday</td>
            <td>15</td>
            <td>11203</td>
            <td>61</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>18</td>
            <td>11385</td>
            <td>60</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>15</td>
            <td>11207</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Friday   </td>
            <td>18</td>
            <td>11207</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Saturday </td>
            <td>19</td>
            <td>11236</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>14</td>
            <td>11207</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>17</td>
            <td>11203</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>17</td>
            <td>11234</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Thursday </td>
            <td>18</td>
            <td>11212</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>17</td>
            <td>11208</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>18</td>
            <td>11203</td>
            <td>58</td>
        </tr>
        <tr>
            <td>Tuesday  </td>
            <td>20</td>
            <td>11207</td>
            <td>58</td>
        </tr>
    </tbody>
</table>



**Police Traffic Command Schedule**

| Time        | Monday | Tuesday | Wednesday | Thursday | Friday | Saturday | Sunday |
|-------------|--------|---------|-----------|----------|--------|----------|--------|
| 0:00-1:00   |        |         |           |          |        |          | 11207 & 11236  |
| 1:00-2:00   |        |         |           |          |        |          |        |
| 2:00-3:00   |        |         |           |          |        |          |        |
| 3:00-4:00   |        |         |           |          |        |          |        |
| 4:00-5:00   |        |         |           |          |        |          |        |
| 5:00-6:00   |        |         |           |          |        |          |        |
| 6:00-7:00   |        |         |           |          |        |          |        |
| 7:00-8:00   |        |         |           |          |        |          |        |
| 8:00-9:00   |        |         |           |          |        |          |        |
| 9:00-10:00  |        |         |           |          |        |          |        |
| 10:00-11:00 |        |         |           |          |        |          |        |
| 11:00-12:00 |        |         |           |          |        |          |        |
| 12:00-13:00 |        |         |           |          |        |          |        |
| 13:00-14:00 |        |         |           |          |        |          |        |
| 14:00-15:00 |        |         |           | 11207          | 11207  |          |        |
| 15:00-16:00 |        |         | 11203     |          | 11207        | 11207    | 11207  |
| 16:00-17:00 |        |         |           | 11207    | 11207  |          |        |
| 17:00-18:00 |        | 11234 & 10451 & 11208   |     11234 & 11203      | 11207 & 11434    | 11207  |          |        |
| 18:00-19:00 |        | 11207 & 11203|           | 11212          | 11385 & 11207  | 11236    |        |
| 19:00-20:00 |        |         |           |          |        | 11236    |        |
| 20:00-21:00 |        | 11207         |           |          | 11207 & 11236        |          |        |
| 21:00-22:00 |        |         |           |          |        | 11236    |        |
| 22:00-23:00 |        |         |           |          |        | 11207    |        |
| 23:00-0:00  |        |         |           |          |        |          |        |

We recommend that the local police office **allocate additional police presence** to the identified combinations of the day of the week, hour of the day, and zip code with high-risk crash incidents based on the above schedule. For example, the analysis indicates that Friday at 5:00 PM in the zip code 11207 has the highest number of persons injured due to car crashes. The police department could consider increasing patrols and visibility during this specific time and in this particular zip code.

Furthermore, we suggest **implement more security cameras to monitor and track vehicle speeds and behaviour** would enhance overall road safety.

#### Analysis 3 for New York Department of Motor Vehicles (DMV):

**Identify the top 10 contributing factors to the highest number of fatalities** and leverage this critical information to provide strategic recommendations to NY Department of Motor Vehicles (DMV). Enable the implementation of targeted interventions and policies aimed at addressing the identified factors and reducing the overall impact of road incidents on public safety.


```sql
%%sql
SELECT
    Contributing_factor.contributing_factor_vehicle_1,
    SUM(Crashes.number_of_persons_killed) AS total_persons_killed,
    SUM(Crashes.number_of_persons_injured) AS total_persons_injured
FROM
    Crashes
JOIN
    Contributing_factor ON Crashes.contributing_factor_key = Contributing_factor.key
GROUP BY
    Contributing_factor.contributing_factor_vehicle_1
ORDER BY
   total_persons_killed DESC, total_persons_injured DESC
LIMIT 10;

```

     * postgresql://student@/Final_Project
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>contributing_factor_vehicle_1</th>
            <th>total_persons_killed</th>
            <th>total_persons_injured</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Unspecified                                                                                         </td>
            <td>259</td>
            <td>51456</td>
        </tr>
        <tr>
            <td>Failure to Yield Right-of-Way                                                                       </td>
            <td>150</td>
            <td>31632</td>
        </tr>
        <tr>
            <td>Driver Inattention/Distraction                                                                      </td>
            <td>125</td>
            <td>59476</td>
        </tr>
        <tr>
            <td>Unsafe Speed                                                                                        </td>
            <td>123</td>
            <td>7731</td>
        </tr>
        <tr>
            <td>Traffic Control Disregarded                                                                         </td>
            <td>78</td>
            <td>11997</td>
        </tr>
        <tr>
            <td>Pedestrian/Bicyclist/Other Pedestrian Error/Confusion                                               </td>
            <td>45</td>
            <td>4538</td>
        </tr>
        <tr>
            <td>Alcohol Involvement                                                                                 </td>
            <td>25</td>
            <td>4448</td>
        </tr>
        <tr>
            <td>Driver Inexperience                                                                                 </td>
            <td>24</td>
            <td>3963</td>
        </tr>
        <tr>
            <td>Illnes                                                                                              </td>
            <td>19</td>
            <td>780</td>
        </tr>
        <tr>
            <td>Backing Unsafely                                                                                    </td>
            <td>18</td>
            <td>4810</td>
        </tr>
    </tbody>
</table>



**Analysis and Recommendations for Top 10 factors:**

1. Unspecified:

Recommendation: Improve data recording practices to specify contributing factors accurately. Enhance awareness campaigns to encourage reporting details of incidents in an automated way.

2. Failure to Yield Right-of-Way:

Recommendation: Strengthen education and enforcement regarding right-of-way rules. Conduct targeted driver education programs emphasizing yielding protocols.

3. Driver Inattention/Distraction:

Recommendation: Implement stricter penalties for distracted driving. Promote public awareness on the dangers of distracted driving and encourage the use of hands-free technology.

4. Unsafe Speed:

Recommendation: Increase enforcement of speed limits. Implement speed-calming measures in high-risk areas. Launch campaigns emphasizing the correlation between speed and accident severity.

5. Traffic Control Disregarded:

Recommendation: Enhance traffic control infrastructure and visibility. Enforce strict penalties for violations. Conduct public awareness campaigns on the importance of obeying traffic signals.

6. Pedestrian/Bicyclist/Other Pedestrian Error/Confusion:

Recommendation: Invest in pedestrian and cyclist safety infrastructure. Develop educational programs for both drivers and pedestrians to reduce confusion and errors.

7. Alcohol Involvement:

Recommendation: Intensify DUI enforcement. Increase public awareness on the dangers of drinking and driving. Implement stricter penalties for alcohol-related offenses.

8. Driver Inexperience:

Recommendation: Strengthen driver education programs, especially for new drivers. Consider implementing graduated licensing systems.

9. Illness:

Recommendation: Encourage individuals with known medical conditions to assess their fitness to drive regularly. Raise awareness among healthcare professionals regarding reporting concerns about patients' ability to drive.

10. Backing Unsafely:

Recommendation: Promote safe backing practices through educational campaigns. Consider technological solutions, such as backup cameras, to assist drivers in preventing accidents.

**Overall Recommendations:**

a) Data Quality Improvement: Enhance data collection methods to ensure accurate and detailed recording of contributing factors.

b) Education and Awareness: Launch targeted public awareness campaigns addressing specific contributing factors.

c) Enforcement: Strengthen law enforcement measures, especially for high-impact factors like distracted driving and speeding.

d) Infrastructure Improvement: Invest in infrastructure enhancements, such as improved traffic control and pedestrian safety measures.

e) Policy Development: Consider revising and implementing policies that address the identified contributing factors effectively.

f) Implementing a multi-faceted approach, combining education, enforcement, and infrastructure improvements, will likely yield the most significant impact in reducing fatalities and injuries related to road incidents.


**Strategic recommendations to NY DMV:**

- As per current Licence Violation Points, the point deduction for 'Failure to Yield Right-of-Way'is 3. We recommended an increase of point-deduction to 5 to implement stricter regulations. Based on our analysis, we see that the total number of people killed by this contributing factor for the last 7 years is 150.

- Based on our analysis, we see that the total number of people killed by 'Driver Inattention/Distraction' is more than those killed by 'Unsafe Speed'. As per current Licence Violation Points, the point deduction for 'Improper cellphone usage' & 'Use of portable electronic device 'Texting' is 5 points whereas, for 'Speeding', it is 3-11 points. We recommend the DMV to increase the points deducted for this particular factor.

##### Below is the screenshot from the NY DMV Official website (https://dmv.ny.gov/tickets/about-nys-driver-point-system) for License Violation Pionts Deduction:


```python
image_url = 'https://i.ibb.co/w7wpqJp/Whats-App-Image-2023-12-09-at-12-24-16-PM.jpg'
Image(url=image_url)
```




<img src="https://i.ibb.co/w7wpqJp/Whats-App-Image-2023-12-09-at-12-24-16-PM.jpg"/>




```python
!sudo chmod a+w /home/ubuntu/notebooks
```


```python
!pwd
```

    /home/ubuntu/notebooks/FinalProjectGroup2



```sql
%%sql
COPY (
    SELECT
        Crash_time.year, 
        Crash_time.month_of_year,
        Crash_time.day_of_week_str,
        Geography.borough,
        count(Crashes.collision_id) AS number_of_incidents,
        SUM(Crashes.number_of_persons_killed) AS total_persons_killed,
        SUM(Crashes.number_of_persons_injured) AS total_persons_injured    
    FROM Crashes
    JOIN Crash_time ON Crashes.crash_time_key = Crash_time.key
    JOIN Geography ON Crashes.geography_key = Geography.key
    GROUP BY Crash_time.year, Crash_time.month_of_year, Crash_time.day_of_week_str, Geography.borough
    ORDER BY Crash_time.year, Crash_time.month_of_year, Crash_time.day_of_week_str, Geography.borough, number_of_incidents
) TO '/home/ubuntu/notebooks/output.csv' WITH CSV HEADER;
```

     * postgresql://student@/Final_Project
    2940 rows affected.





    []




```sql
%%sql
COPY (
    SELECT
    Contributing_factor.contributing_factor_vehicle_1,
    SUM(Crashes.number_of_persons_killed) AS total_persons_killed,
    SUM(Crashes.number_of_persons_injured) AS total_persons_injured
FROM
    Crashes
JOIN
    Contributing_factor ON Crashes.contributing_factor_key = Contributing_factor.key
GROUP BY
    Contributing_factor.contributing_factor_vehicle_1
ORDER BY
   total_persons_killed DESC, total_persons_injured DESC
) TO '/home/ubuntu/notebooks/output1.csv' WITH CSV HEADER;
```

     * postgresql://student@/Final_Project
    61 rows affected.





    []




```python

```
