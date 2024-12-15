<h1 align ="Center"> Chicago Crime EDA </h1>

## Overview
Chicago has faced a longstanding reputation for high crime rates. While crime levels have fluctuated over the years, certain community areas consistently report higher crime rates, often linked to economic disparities, unemployment, and systemic challenges. 

![Chicago Crime](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Chicago%20Crimes.png)

*Chicago Crime from 2019-2023 (Source: Chicago City Data Portal)*

The primary goal of this project is to assess and analyze Chicago's crime patterns and their potential correlation with socioeconomic conditions and school performance. By the end of this project, I hope we can discover the following:

- Identify crime hotspots and trends in different community areas.
- Examine the relationship between crime and socioeconomic factors, such as income levels and hardship index.
- Assess school safety and its connection to crime in the vicinity.

## Task
This project involves analyzing three key datasets from Chicago using SQL and Python to provide insights into crime, socioeconomic conditions, and school performance. The datasets are integrated into an SQLite database for efficient querying and include:

1. [Chicago Crime Data](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/dataset/ChicagoCrimeData.csv): A record of reported incidents of crime in Chicago from 2001 to the present, with details about types of crimes, locations, and arrest information.
2. [Socioeconomic Indicators (Census Data)](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/dataset/ChicagoCensusData.csv): Data showing socioeconomic factors like per capita income, poverty levels, and a hardship index for each Chicago community area.
3. [Chicago Public Schools Data](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/dataset/ChicagoPublicSchools.csv): Performance metrics and safety scores for public schools during the 2011–2012 academic year.



First we load all the data sets into the notebook, as well as the desired python packages we will be utilising:

```python
import pandas as pd
import csv, sqlite3

con = sqlite3.connect("FinalDB.db")
cur = con.cursor()
%load_ext sql
Census_Data = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01")
Chicago_PS = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01")
Chicago_Crime = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01")
Census_Data.to_sql("CENSUS_DATA", con, if_exists='replace', index=False,method="multi")
Chicago_PS.to_sql("CHICAGO_PUBLIC_SCHOOLS", con, if_exists='replace', index=False,method="multi")
Chicago_Crime.to_sql("CHICAGO_CRIME_DATA", con, if_exists='replace', index=False,method="multi")
%sql sqlite:///FinalDB.db
```

The full list of datasets can be found [here](https://github.com/YaasirM/Chicago_Crime_EDA/tree/main/assets/dataset). 

## Questions

Now I wil try to use SQL to answer the following questions below:

### Question 1
Find the total number of crimes recorded in the CRIME table.

```sql
SELECT
  COUNT(*)
FROM
 Chicago_Crime_DATA
```
![No. of Crimes](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/No.%20of%20Crimes.png)

### Question 2
List community area names and numbers with per capita income less than 11000.

```sql
SELECT
  Community_AREA_NUMBER, COMMUNITY_AREA_NAME
FROM
  CENSUS_DATA
WHERE
 PER_CAPITA_INCOME <11000 

```
![Question 2](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Quesion%202.png)

### Question 3
List all case numbers for crimes involving minors (children are not considered minors for the purposes of crime analysis).

```sql
SELECT
  CASE_NUMBER, PRIMARY_TYPE
FROM
  CHICAGO_CRIME_DATA
WHERE
  DESCRIPTION
LIKE
 ("%minor%")

```
| ID       | CASE_NUMBER | DATE       | BLOCK                    | IUCR | PRIMARY_TYPE          | DESCRIPTION                     | LOCATION_DESCRIPTION | ARREST | DOMESTIC | BEAT | DISTRICT | WARD | COMMUNITY_AREA_NUMBER | FBICODE | X_COORDINATE | Y_COORDINATE | YEAR | LATITUDE    | LONGITUDE     | LOCATION                            |
|----------|-------------|------------|--------------------------|------|-----------------------|---------------------------------|-----------------------|--------|----------|------|----------|------|----------------------|---------|-------------|-------------|------|-------------|---------------|-------------------------------------|
| 3987219  | HL266884    | 2005-03-31 | 024XX N CLARK ST        | 2210 | LIQUOR LAW VIOLATION | SELL/GIVE/DEL LIQUOR TO MINOR  | CONVENIENCE STORE    | 1      | 0        | 2333 | 19       | 43.0 | 7.0                  | 22      | 1172680.0   | 1916483.0   | 2005 | 41.92626872 | -87.64089934 | (41.926268719, -87.640899336)       |
| 3266814  | HK238408    | 2004-03-13 | 093XX S STONY ISLAND AVE | 2230 | LIQUOR LAW VIOLATION | ILLEGAL CONSUMPTION BY MINOR   | ALLEY                | 1      | 0        | 413  | 4        | 8.0  | 48.0                 | 22      | 1188539.0   | 1843379.0   | 2004 | 41.72530099 | -87.58496589 | (41.72530099, -87.584965887)        |

**(Picture was too long to fit)*

### Question 4
List all kidnapping crimes involving a child.

```sql
SELECT
 *
FROM
 CHICAGO_CRIME_DATA
WHERE
 PRIMARY_TYPE = 'KIDNAPPING'
```

| ID       | CASE_NUMBER | DATE       | BLOCK                    | IUCR | PRIMARY_TYPE | DESCRIPTION              | LOCATION_DESCRIPTION | ARREST | DOMESTIC | BEAT | DISTRICT | WARD | COMMUNITY_AREA_NUMBER | FBICODE | X_COORDINATE | Y_COORDINATE | YEAR | LATITUDE    | LONGITUDE     | LOCATION                            |
|----------|-------------|------------|--------------------------|------|--------------|--------------------------|-----------------------|--------|----------|------|----------|------|----------------------|---------|-------------|-------------|------|-------------|---------------|-------------------------------------|
| 5276766  | HN144152    | 2007-01-26 | 050XX W VAN BUREN ST    | 1792 | KIDNAPPING   | CHILD ABDUCTION/STRANGER | STREET                | 0      | 0        | 1533 | 15       | 29.0 | 25.0                 | 20      | 1143050.0   | 1897546.0   | 2007 | 41.87490841 | -87.75024931 | (41.874908413, -87.750249307)       |

**(Picture was too long to fit)*

### Question 5
List the kind of crimes that were recorded at schools (No repetitions).
```sql
SELECT
 DISTINCT(PRIMARY_TYPE), ID, LOCATION_DESCRIPTION
FROM
 CHICAGO_CRIME_DATA
WHERE
 LOCATION_DESCRIPTION
LIKE
 ('%School%')
```
![Question 5](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Question%205.png)

### Question 6
List the type of schools along with the average safety score for each type.

```sql
SELECT
 AVG(SAFETY_SCORE), "Elementary, Middle, or High School"
FROM
 CHICAGO_PUBLIC_SCHOOLS
GROUP BY
 "Elementary, Middle, or High School"
```
![Question 6](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Question%206.png)

### Question 7
What 5 community areas with highest % of households below poverty line?

```sql
SELECT
 COMMUNITY_AREA_NUMBER, COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY
FROM
 CENSUS_DATA
ORDER BY
 PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC
LIMIT
 5
```
![Question 7](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Question%207.png)

### Question 8
Which community area is most crime prone (coumminty area number only)?
```sql
SELECT
COMMUNITY_AREA_NAME, HARDSHIP_INDEX
FROM
 CENSUS_DATA
ORDER BY
 HARDSHIP_INDEX DESC
LIMIT
 1
```
![Question 8](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Question%208.png)

### Question 9
What was the name of the community area with highest hardship index?
```sql
SELECT
 COMMUNITY_AREA_NAME, HARDSHIP_INDEX
FROM
 CENSUS_DATA
ORDER BY
 HARDSHIP_INDEX DESC
LIMIT
 1
```
![Question 9](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Question%209.png)

### Question 10
What was the Community Area Name with most number of crimes?

```sql
SELECT
 cd.COMMUNITY_AREA_NAME, MAX(num_crimes)
AS
 MAX_NUM_CRIMES
FROM
 CENSUS_DATA
AS
 cd
JOIN
 (SELECT
   COMMUNITY_AREA_NUMBER, COUNT(*)
  AS
   num_crimes
  FROM
   CHICAGO_CRIME_DATA
  WHERE
   COMMUNITY_AREA_NUMBER IS NOT NULL
  GROUP BY
   COMMUNITY_AREA_NUMBER
 )
AS
 crime_counts ON cd.COMMUNITY_AREA_NUMBER = crime_counts.COMMUNITY_AREA_NUMBER
GROUP BY
 cd.COMMUNITY_AREA_NAME
ORDER BY
 max_num_crimes DESC
LIMIT
 1
```
![Question 10](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/images/Question%2010.png)

## Conclusion
From the SQL analysis, we have discovered insights into crime, socio-economic conditions, and education in Chicago:

- Crime Patterns: A total of 533 crimes were recorded, with minors frequently involved. Schools were common crime locations, highlighting the need for better safety measures.
- Crime Hotspots: Austin had the highest crime rates, while Riverdale showed the greatest hardship index and poverty levels. Fuller Park and Englewood also faced severe hardship, correlating with higher crime rates.
- School Safety: Public schools showed varying safety scores, with elementary and high schools averaging higher safety ratings than middle schools, indicating a need for more consistent safety standards.
Using SQL subqueries, the analysis identified community areas with the highest crime and hardship levels, demonstrating the value of structured data analysis. These findings reveal the link between socio-economic challenges, crime, and education, guiding targeted policy efforts.

This project helped to develop my SQL skills further. Areas that improved the most included subqueries, aggregate functions, and joins to analyze crime statistics, socio-economic factors, and public school safety. Advanced skills were developed, including case statements for conditional analysis and window functions for ranking and partitioning data. Working with large datasets improved query optimization and the ability to write efficient, structured SQL scripts.

## Jupyter Notebook
The analysis and modelling were performed using Python, SQL and Jupyter Notebook. You can find the complete notebook [here](https://github.com/YaasirM/Chicago_Crime_EDA/blob/main/assets/Chicago_Crime_EDA_Notebook.ipynb)
