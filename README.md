# Chicago Crime EDA

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

## Test

Before completing the analysis, we first have to test if the datasets have been imported correctly onto the Jupyter Notebook:

```sql
SELECT COUNT(*) FROM Chicago_Crime_DATA
```
The result that came from actioning this query was 533, which is the correct count for this dataset.


