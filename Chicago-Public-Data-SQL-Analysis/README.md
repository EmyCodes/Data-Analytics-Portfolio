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
<table>
    <thead>
        <tr>
            <th>COUNT(*)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>533</td>
        </tr>
    </tbody>
</table>

**Interpretation**: There were 533 crime records in the dataset.

### Problem 2: Communities with per capita income < 11,000
```sql
%%sql
SELECT COMMUNITY_AREA_NAME, COMMUNITY_AREA_NUMBER, PER_CAPITA_INCOME
FROM CENSUS_DATA
WHERE PER_CAPITA_INCOME < 11000;
```
<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
            <th>COMMUNITY_AREA_NUMBER</th>
            <th>PER_CAPITA_INCOME</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>West Garfield Park</td>
            <td>26.0</td>
            <td>10934</td>
        </tr>
        <tr>
            <td>South Lawndale</td>
            <td>30.0</td>
            <td>10402</td>
        </tr>
        <tr>
            <td>Fuller Park</td>
            <td>37.0</td>
            <td>10432</td>
        </tr>
        <tr>
            <td>Riverdale</td>
            <td>54.0</td>
            <td>8201</td>
        </tr>
    </tbody>
</table>

**Interpretation**: West Garfield Park, South Lawndale, Fuller Park, and Riverdale have per capita incomes below $11,000, indicating significant economic challenges in these communities.


### Problem 3: Crimes involving minors
```sql
%%sql
SELECT CASE_NUMBER, DESCRIPTION
FROM CHICAGO_CRIME_DATA
WHERE DESCRIPTION LIKE '%MINOR%';
```
<table>
    <thead>
        <tr>
            <th>CASE_NUMBER</th>
            <th>DESCRIPTION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HL266884</td>
            <td>SELL/GIVE/DEL LIQUOR TO MINOR</td>
        </tr>
        <tr>
            <td>HK238408</td>
            <td>ILLEGAL CONSUMPTION BY MINOR</td>
        </tr>
    </tbody>
</table>

**Interpretation**: Two crimes involving minors; one related to selling/giving liquor to a minor and another related to illegal consumption by a minor.

### Problem 4: Kidnapping crimes involving a child
```sql
%%sql
SELECT CASE_NUMBER, PRIMARY_TYPE, DESCRIPTION
FROM CHICAGO_CRIME_DATA
WHERE PRIMARY_TYPE = 'KIDNAPPING'
  AND DESCRIPTION LIKE '%CHILD%';
```
<table>
    <thead>
        <tr>
            <th>CASE_NUMBER</th>
            <th>PRIMARY_TYPE</th>
            <th>DESCRIPTION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HN144152</td>
            <td>KIDNAPPING</td>
            <td>CHILD ABDUCTION/STRANGER</td>
        </tr>
    </tbody>
</table>

**Interpretation**: One kidnapping case involving a child, specifically a child abduction by a stranger.

### Problem 5: Crimes recorded at schools (distinct types)
```sql
%%sql
SELECT DISTINCT PRIMARY_TYPE, DESCRIPTION, LOCATION_DESCRIPTION
FROM CHICAGO_CRIME_DATA
WHERE LOCATION_DESCRIPTION LIKE '%SCHOOL%';
```
<table>
    <thead>
        <tr>
            <th>CASE_NUMBER</th>
            <th>PRIMARY_TYPE</th>
            <th>DESCRIPTION</th>
            <th>LOCATION_DESCRIPTION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HL353697</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HL725506</td>
            <td>BATTERY</td>
            <td>PRO EMP HANDS NO/MIN INJURY</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>HP716225</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>HH639427</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>JA460432</td>
            <td>BATTERY</td>
            <td>SIMPLE</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HS200939</td>
            <td>CRIMINAL DAMAGE</td>
            <td>TO VEHICLE</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HK577020</td>
            <td>NARCOTICS</td>
            <td>POSS: HEROIN(WHITE)</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HS305355</td>
            <td>NARCOTICS</td>
            <td>MANU/DEL:CANNABIS 10GM OR LESS</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
        <tr>
            <td>HT315369</td>
            <td>ASSAULT</td>
            <td>PRO EMP HANDS NO/MIN INJURY</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HR585012</td>
            <td>CRIMINAL TRESPASS</td>
            <td>TO LAND</td>
            <td>SCHOOL, PUBLIC, GROUNDS</td>
        </tr>
        <tr>
            <td>HH292682</td>
            <td>PUBLIC PEACE VIOLATION</td>
            <td>BOMB THREAT</td>
            <td>SCHOOL, PRIVATE, BUILDING</td>
        </tr>
        <tr>
            <td>G635735</td>
            <td>PUBLIC PEACE VIOLATION</td>
            <td>BOMB THREAT</td>
            <td>SCHOOL, PUBLIC, BUILDING</td>
        </tr>
    </tbody>
</table>

**Interpretation**: Crimes at schools include various types of battery, criminal damage, narcotics offenses, assault, trespassing, and bomb threats. Both public and private school grounds/buildings are affected.


### Problem 6: Average safety score by school type
```sql
%%sql
SELECT `Elementary, Middle, or High School`, AVG(SAFETY_SCORE)
FROM CHICAGO_PUBLIC_SCHOOLS
GROUP BY `Elementary, Middle, or High School`;
```
<table>
    <thead>
        <tr>
            <th>Elementary, Middle, or High School</th>
            <th>AVG(SAFETY_SCORE)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ES</td>
            <td>49.52038369304557</td>
        </tr>
        <tr>
            <td>HS</td>
            <td>49.62352941176471</td>
        </tr>
        <tr>
            <td>MS</td>
            <td>48.0</td>
        </tr>
    </tbody>
</table>

**Interpretation**: High schools have a slightly higher average safety score compared to elementary and middle schools, but the differences are minimal.

### Problem 7: Top 5 communities by % households below poverty
```sql
%%sql
SELECT COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY
FROM CENSUS_DATA
ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC
LIMIT 5;
```
<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
            <th>PERCENT_HOUSEHOLDS_BELOW_POVERTY</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Riverdale</td>
            <td>56.5</td>
        </tr>
        <tr>
            <td>Fuller Park</td>
            <td>51.2</td>
        </tr>
        <tr>
            <td>Englewood</td>
            <td>46.6</td>
        </tr>
        <tr>
            <td>North Lawndale</td>
            <td>43.1</td>
        </tr>
        <tr>
            <td>East Garfield Park</td>
            <td>42.4</td>
        </tr>
    </tbody>
</table>

**Interpretation**: Riverdale, Fuller Park, Englewood, North Lawndale, East Garfield Park have the highest poverty rates.

### Problem 8: Community area number with most crimes
```sql
%%sql
SELECT COMMUNITY_AREA_NUMBER
FROM (
    SELECT COMMUNITY_AREA_NUMBER, COUNT(*) AS CNT
    FROM CHICAGO_CRIME_DATA
    GROUP BY COMMUNITY_AREA_NUMBER
    ORDER BY CNT DESC
)
LIMIT 1;
```
<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NUMBER</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>25.0</td>
        </tr>
    </tbody>
</table>

**Interpretation**: Community area number 25.0 has the highest number of recorded crimes in the dataset.

### Problem 9: Community with highest hardship index
```sql
%%sql
SELECT COMMUNITY_AREA_NAME
FROM CENSUS_DATA
WHERE HARDSHIP_INDEX = (
    SELECT MAX(HARDSHIP_INDEX)
    FROM CENSUS_DATA
);
```
<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Riverdale</td>
        </tr>
    </tbody>
</table>

**Interpretation**: Riverdale has the highest hardship index, indicating it faces significant socioeconomic challenges.

### Problem 10: Community name with the most crimes
```sql
%%sql
SELECT COMMUNITY_AREA_NAME
FROM CENSUS_DATA C
JOIN (
    SELECT COMMUNITY_AREA_NUMBER, COUNT(*) AS CRIME_COUNT
    FROM CHICAGO_CRIME_DATA
    GROUP BY COMMUNITY_AREA_NUMBER
) D
ON C.COMMUNITY_AREA_NUMBER = D.COMMUNITY_AREA_NUMBER
ORDER BY CRIME_COUNT DESC
LIMIT 1;
```

<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Austin</td>
        </tr>
    </tbody>
</table>

**Interpretation**: Austin is the community area with the most recorded crimes in the dataset.