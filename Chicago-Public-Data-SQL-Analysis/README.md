# 🏙️ Urban Analytics with SQL & Python: Crime, Education, and Census Data (Chicago)

## 📌 Project Overview

Cities are complex systems where **crime**, **education**, and **socioeconomic conditions** intersect.  
This project investigates those intersections using real-world datasets from the **City of Chicago**, structured into a relational database and queried using SQL.

The goal was not visualization or prediction, but **building a clean analytical pipeline**:  
raw CSV files → relational tables → SQL-driven answers to real civic questions.

## Real-World Problem

Urban decision-makers need to answer questions such as:

- Which communities experience the highest crime burden?
- How does poverty relate to crime concentration?
- What kinds of crimes occur around schools?
- Which communities face the highest socioeconomic hardship?

Answering these requires **integrated data**, not isolated spreadsheets.

## 📂 Data Sources

Official Chicago open datasets (subset versions prepared for SQL analysis):

1. [**Chicago Census Data**]("dataset/Chicago-Census-Data.csv")  
   Socioeconomic indicators and hardship index by community area  
2. [**Chicago Public Schools Data**]("dataset/Chicago-Public-Schools.csv")  
   School-level performance, safety, and attendance metrics  
3. [**Chicago Crime Data**]("dataset/Chicago-Crime-Data.csv")  
   Reported crime incidents by type, location, year, and community area  

## ⚙️ Tools & Technologies

- Python (Pandas, sqlite3)  
- SQLite  
- SQL  
- Jupyter Notebook  
- ipython-sql  

## 🗄️ Database Schema

### 1️⃣ [CENSUS_DATA]("dataset/Chicago-Census-Data.csv")

| Column                              | Description                              |
|-------------------------------------|------------------------------------------|
| COMMUNITY_AREA_NUMBER               | Community identifier                     |
| COMMUNITY_AREA_NAME                 | Community name                           |
| PER_CAPITA_INCOME                   | Income per resident                      |
| PERCENT_HOUSEHOLDS_BELOW_POVERTY    | Poverty percentage                       |
| HARDSHIP_INDEX                      | Composite socioeconomic hardship score   |

### 2️⃣ [CHICAGO_PUBLIC_SCHOOLS]("dataset/Chicago-Public-Schools.csv")

| Column                            | Description                  |
|-----------------------------------|------------------------------|
| School_ID                         | Unique school ID             |
| NAME_OF_SCHOOL                    | School name                  |
| SAFETY_SCORE                      | Safety score                 |
| AVERAGE_STUDENT_ATTENDANCE        | Attendance rate              |
| COMMUNITY_AREA_NUMBER             | Community location           |

### 3️⃣ [CHICAGO_CRIME_DATA]("dataset/Chicago-Crime-Data.csv")

| Column                  | Description              |
|-------------------------|--------------------------|
| ID                      | Crime record ID          |
| CASE_NUMBER             | Police case number       |
| PRIMARY_TYPE            | Crime category           |
| DESCRIPTION             | Crime description        |
| LOCATION_DESCRIPTION    | Location of crime        |
| ARREST                  | Arrest made (0/1)        |
| YEAR                    | Year                     |
| COMMUNITY_AREA_NUMBER   | Community location       |

## 🔄 Data Loading (Python ETL)

```python
!pip install pandas
!pip install ipython-sql prettytable

import pandas as pd
import sqlite3
import prettytable
prettytable.DEFAULT = 'DEFAULT'

con = sqlite3.connect("FinalDB.db")
cur = con.cursor()

df = pd.read_csv("ChicagoCensusData.csv")
df.to_sql("CENSUS_DATA", con, if_exists='replace', index=False, method="multi")

df = pd.read_csv("ChicagoPublicSchools.csv")
df.to_sql("CHICAGO_PUBLIC_SCHOOLS", con, if_exists='replace', index=False, method="multi")

df = pd.read_csv("ChicagoCrimeData.csv")
df.to_sql("CHICAGO_CRIME_DATA", con, if_exists='replace', index=False, method="multi")
```
## 🔍 SQL Analysis (Jupyter Notebook)
Using magic commands to connect to the SQLite database with prefixed code, '%%sql' for cell magic and '%sql' for line magic as shown below:
```python 
%load_ext sql
%sql sqlite:///FinalDB.db
```

## Analytical Questions, SQL Queries & Outcomes
### Problem 1: Total number of crimes recorded
```sql
%%sql
SELECT COUNT(*)
FROM CHICAGO_CRIME_DATA;
```
**Result**: 533 crimes recorded in the dataset.

### Problem 2: Communities with per capita income < 11,000
```sql
%%sql
SELECT COMMUNITY_AREA_NAME, COMMUNITY_AREA_NUMBER, PER_CAPITA_INCOME
FROM CENSUS_DATA
WHERE PER_CAPITA_INCOME < 11000;
```
**Result**: West Garfield Park, South Lawndale, Fuller Park, Riverdale

### Problem 3: Crimes involving minors
```sql
%%sql
SELECT CASE_NUMBER, DESCRIPTION
FROM CHICAGO_CRIME_DATA
WHERE DESCRIPTION LIKE '%MINOR%';
```
**Result**: Cases related to illegal alcohol provision / consumption by minors