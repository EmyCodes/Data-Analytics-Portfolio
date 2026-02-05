# Urban Analytics with SQL & Python: Crime, Education, and Census Data (Chicago)

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

1. [**Chicago Census Data**]("data/ChicagoCensusData.csv")  
   Socioeconomic indicators and hardship index by community area  
2. [**Chicago Public Schools Data**]("data/ChicagoPublicSchools.csv")  
   School-level performance, safety, and attendance metrics  
3. [**Chicago Crime Data**]("data/ChicagoCrimeData.csv")  
   Reported crime incidents by type, location, year, and community area  

## ⚙️ Tools & Technologies

- Python (Pandas, sqlite3)  
- SQLite  
- SQL  
- Jupyter Notebook  
- ipython-sql  

**Dependencies** are listed in [`requirements.txt`](./requirements.txt).

## 🗄️ Database Schema

### 1️⃣ [CENSUS_DATA]("data/ChicagoCensusData.csv")

<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NUMBER</th>
            <th>COMMUNITY_AREA_NAME</th>
            <th>PERCENT_OF_HOUSING_CROWDED</th>
            <th>PERCENT_HOUSEHOLDS_BELOW_POVERTY</th>
            <th>PERCENT_AGED_16__UNEMPLOYED</th>
            <th>PERCENT_AGED_25__WITHOUT_HIGH_SCHOOL_DIPLOMA</th>
            <th>PERCENT_AGED_UNDER_18_OR_OVER_64</th>
            <th>PER_CAPITA_INCOME</th>
            <th>HARDSHIP_INDEX</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1.0</td>
            <td>Rogers Park</td>
            <td>7.7</td>
            <td>23.6</td>
            <td>8.7</td>
            <td>18.2</td>
            <td>27.5</td>
            <td>23939</td>
            <td>39.0</td>
        </tr>
        <tr>
            <td>2.0</td>
            <td>West Ridge</td>
            <td>7.8</td>
            <td>17.2</td>
            <td>8.8</td>
            <td>20.8</td>
            <td>38.5</td>
            <td>23040</td>
            <td>46.0</td>
        </tr>
        <tr>
            <td>3.0</td>
            <td>Uptown</td>
            <td>3.8</td>
            <td>24.0</td>
            <td>8.9</td>
            <td>11.8</td>
            <td>22.2</td>
            <td>35787</td>
            <td>20.0</td>
        </tr>
        <tr>
            <td>4.0</td>
            <td>Lincoln Square</td>
            <td>3.4</td>
            <td>10.9</td>
            <td>8.2</td>
            <td>13.4</td>
            <td>25.5</td>
            <td>37524</td>
            <td>17.0</td>
        </tr>
        <tr>
            <td>5.0</td>
            <td>North Center</td>
            <td>0.3</td>
            <td>7.5</td>
            <td>5.2</td>
            <td>4.5</td>
            <td>26.2</td>
            <td>57123</td>
            <td>6.0</td>
        </tr>
    </tbody>
</table>

[//]: # ()
[//]: # (| Column                              | Description                              |)

[//]: # (|-------------------------------------|------------------------------------------|)

[//]: # (| COMMUNITY_AREA_NUMBER               | Community identifier                     |)

[//]: # (| COMMUNITY_AREA_NAME                 | Community name                           |)

[//]: # (| PER_CAPITA_INCOME                   | Income per resident                      |)

[//]: # (| PERCENT_HOUSEHOLDS_BELOW_POVERTY    | Poverty percentage                       |)

[//]: # (| HARDSHIP_INDEX                      | Composite socioeconomic hardship score   |)

### 2️⃣ [CHICAGO_PUBLIC_SCHOOLS]("data/ChicagoPublicSchools.csv")

<table>
    <thead>
        <tr>
            <th>School_ID</th>
            <th>NAME_OF_SCHOOL</th>
            <th>Elementary, Middle, or High School</th>
            <th>Street_Address</th>
            <th>City</th>
            <th>State</th>
            <th>ZIP_Code</th>
            <th>Phone_Number</th>
            <th>Link</th>
            <th>Network_Manager</th>
            <th>Collaborative_Name</th>
            <th>Adequate_Yearly_Progress_Made_</th>
            <th>Track_Schedule</th>
            <th>CPS_Performance_Policy_Status</th>
            <th>CPS_Performance_Policy_Level</th>
            <th>HEALTHY_SCHOOL_CERTIFIED</th>
            <th>Safety_Icon</th>
            <th>SAFETY_SCORE</th>
            <th>Family_Involvement_Icon</th>
            <th>Family_Involvement_Score</th>
            <th>Environment_Icon</th>
            <th>Environment_Score</th>
            <th>Instruction_Icon</th>
            <th>Instruction_Score</th>
            <th>Leaders_Icon</th>
            <th>Leaders_Score</th>
            <th>Teachers_Icon</th>
            <th>Teachers_Score</th>
            <th>Parent_Engagement_Icon</th>
            <th>Parent_Engagement_Score</th>
            <th>Parent_Environment_Icon</th>
            <th>Parent_Environment_Score</th>
            <th>AVERAGE_STUDENT_ATTENDANCE</th>
            <th>Rate_of_Misconducts__per_100_students_</th>
            <th>Average_Teacher_Attendance</th>
            <th>Individualized_Education_Program_Compliance_Rate</th>
            <th>Pk_2_Literacy__</th>
            <th>Pk_2_Math__</th>
            <th>Gr3_5_Grade_Level_Math__</th>
            <th>Gr3_5_Grade_Level_Read__</th>
            <th>Gr3_5_Keep_Pace_Read__</th>
            <th>Gr3_5_Keep_Pace_Math__</th>
            <th>Gr6_8_Grade_Level_Math__</th>
            <th>Gr6_8_Grade_Level_Read__</th>
            <th>Gr6_8_Keep_Pace_Math_</th>
            <th>Gr6_8_Keep_Pace_Read__</th>
            <th>Gr_8_Explore_Math__</th>
            <th>Gr_8_Explore_Read__</th>
            <th>ISAT_Exceeding_Math__</th>
            <th>ISAT_Exceeding_Reading__</th>
            <th>ISAT_Value_Add_Math</th>
            <th>ISAT_Value_Add_Read</th>
            <th>ISAT_Value_Add_Color_Math</th>
            <th>ISAT_Value_Add_Color_Read</th>
            <th>Students_Taking__Algebra__</th>
            <th>Students_Passing__Algebra__</th>
            <th>9th Grade EXPLORE (2009)</th>
            <th>9th Grade EXPLORE (2010)</th>
            <th>10th Grade PLAN (2009)</th>
            <th>10th Grade PLAN (2010)</th>
            <th>Net_Change_EXPLORE_and_PLAN</th>
            <th>11th Grade Average ACT (2011)</th>
            <th>Net_Change_PLAN_and_ACT</th>
            <th>College_Eligibility__</th>
            <th>Graduation_Rate__</th>
            <th>College_Enrollment_Rate__</th>
            <th>COLLEGE_ENROLLMENT</th>
            <th>General_Services_Route</th>
            <th>Freshman_on_Track_Rate__</th>
            <th>X_COORDINATE</th>
            <th>Y_COORDINATE</th>
            <th>Latitude</th>
            <th>Longitude</th>
            <th>COMMUNITY_AREA_NUMBER</th>
            <th>COMMUNITY_AREA_NAME</th>
            <th>Ward</th>
            <th>Police_District</th>
            <th>Location</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>610038</td>
            <td>Abraham Lincoln Elementary School</td>
            <td>ES</td>
            <td>615 W Kemper Pl</td>
            <td>Chicago</td>
            <td>IL</td>
            <td>60614</td>
            <td>(773) 534-5720</td>
            <td>http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_610038.pdf</td>
            <td>Fullerton Elementary Network</td>
            <td>NORTH-NORTHWEST SIDE COLLABORATIVE</td>
            <td>No</td>
            <td>Standard</td>
            <td>Not on Probation</td>
            <td>Level 1</td>
            <td>Yes</td>
            <td>Very Strong</td>
            <td>99.0</td>
            <td>Very Strong</td>
            <td>99</td>
            <td>Strong</td>
            <td>74.0</td>
            <td>Strong</td>
            <td>66.0</td>
            <td>Weak</td>
            <td>65</td>
            <td>Strong</td>
            <td>70</td>
            <td>Strong</td>
            <td>56</td>
            <td>Average</td>
            <td>47</td>
            <td>96.00%</td>
            <td>2.0</td>
            <td>96.40%</td>
            <td>95.80%</td>
            <td>80.1</td>
            <td>43.3</td>
            <td>89.6</td>
            <td>84.9</td>
            <td>60.7</td>
            <td>62.6</td>
            <td>81.9</td>
            <td>85.2</td>
            <td>52</td>
            <td>62.4</td>
            <td>66.3</td>
            <td>77.9</td>
            <td>69.7</td>
            <td>64.4</td>
            <td>0.2</td>
            <td>0.9</td>
            <td>Yellow</td>
            <td>Green</td>
            <td>67.1</td>
            <td>54.5</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>813</td>
            <td>33</td>
            <td>NDA</td>
            <td>1171699.458</td>
            <td>1915829.428</td>
            <td>41.92449696</td>
            <td>-87.64452163</td>
            <td>7</td>
            <td>LINCOLN PARK</td>
            <td>43</td>
            <td>18</td>
            <td>(41.92449696, -87.64452163)</td>
        </tr>
        <tr>
            <td>610281</td>
            <td>Adam Clayton Powell Paideia Community Academy Elementary School</td>
            <td>ES</td>
            <td>7511 S South Shore Dr</td>
            <td>Chicago</td>
            <td>IL</td>
            <td>60649</td>
            <td>(773) 535-6650</td>
            <td>http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_610281.pdf</td>
            <td>Skyway Elementary Network</td>
            <td>SOUTH SIDE COLLABORATIVE</td>
            <td>No</td>
            <td>Track_E</td>
            <td>Not on Probation</td>
            <td>Level 1</td>
            <td>No</td>
            <td>Average</td>
            <td>54.0</td>
            <td>Strong</td>
            <td>66</td>
            <td>Strong</td>
            <td>74.0</td>
            <td>Very Strong</td>
            <td>84.0</td>
            <td>Weak</td>
            <td>63</td>
            <td>Strong</td>
            <td>76</td>
            <td>Weak</td>
            <td>46</td>
            <td>Average</td>
            <td>50</td>
            <td>95.60%</td>
            <td>15.7</td>
            <td>95.30%</td>
            <td>100.00%</td>
            <td>62.4</td>
            <td>51.7</td>
            <td>21.9</td>
            <td>15.1</td>
            <td>29</td>
            <td>42.8</td>
            <td>38.5</td>
            <td>27.4</td>
            <td>44.8</td>
            <td>42.7</td>
            <td>14.1</td>
            <td>34.4</td>
            <td>16.8</td>
            <td>16.5</td>
            <td>0.7</td>
            <td>1.4</td>
            <td>Green</td>
            <td>Green</td>
            <td>17.2</td>
            <td>27.3</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>521</td>
            <td>46</td>
            <td>NDA</td>
            <td>1196129.985</td>
            <td>1856209.466</td>
            <td>41.76032435</td>
            <td>-87.55673627</td>
            <td>43</td>
            <td>SOUTH SHORE</td>
            <td>7</td>
            <td>4</td>
            <td>(41.76032435, -87.55673627)</td>
        </tr>
        <tr>
            <td>610185</td>
            <td>Adlai E Stevenson Elementary School</td>
            <td>ES</td>
            <td>8010 S Kostner Ave</td>
            <td>Chicago</td>
            <td>IL</td>
            <td>60652</td>
            <td>(773) 535-2280</td>
            <td>http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_610185.pdf</td>
            <td>Midway Elementary Network</td>
            <td>SOUTHWEST SIDE COLLABORATIVE</td>
            <td>No</td>
            <td>Standard</td>
            <td>Not on Probation</td>
            <td>Level 2</td>
            <td>No</td>
            <td>Strong</td>
            <td>61.0</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>Average</td>
            <td>50.0</td>
            <td>Weak</td>
            <td>36.0</td>
            <td>Weak</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>Average</td>
            <td>47</td>
            <td>Weak</td>
            <td>41</td>
            <td>95.70%</td>
            <td>2.3</td>
            <td>94.70%</td>
            <td>98.30%</td>
            <td>53.7</td>
            <td>26.6</td>
            <td>38.3</td>
            <td>34.7</td>
            <td>43.7</td>
            <td>57.3</td>
            <td>48.8</td>
            <td>39.2</td>
            <td>46.8</td>
            <td>44</td>
            <td>7.5</td>
            <td>21.9</td>
            <td>18.3</td>
            <td>15.5</td>
            <td>-0.9</td>
            <td>-1.0</td>
            <td>Red</td>
            <td>Red</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>1324</td>
            <td>44</td>
            <td>NDA</td>
            <td>1148427.165</td>
            <td>1851012.215</td>
            <td>41.74711093</td>
            <td>-87.73170248</td>
            <td>70</td>
            <td>ASHBURN</td>
            <td>13</td>
            <td>8</td>
            <td>(41.74711093, -87.73170248)</td>
        </tr>
        <tr>
            <td>609993</td>
            <td>Agustin Lara Elementary Academy</td>
            <td>ES</td>
            <td>4619 S Wolcott Ave</td>
            <td>Chicago</td>
            <td>IL</td>
            <td>60609</td>
            <td>(773) 535-4389</td>
            <td>http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_609993.pdf</td>
            <td>Pershing Elementary Network</td>
            <td>SOUTHWEST SIDE COLLABORATIVE</td>
            <td>No</td>
            <td>Track_E</td>
            <td>Not on Probation</td>
            <td>Level 1</td>
            <td>No</td>
            <td>Average</td>
            <td>56.0</td>
            <td>Average</td>
            <td>44</td>
            <td>Average</td>
            <td>45.0</td>
            <td>Weak</td>
            <td>37.0</td>
            <td>Weak</td>
            <td>65</td>
            <td>Average</td>
            <td>48</td>
            <td>Average</td>
            <td>53</td>
            <td>Strong</td>
            <td>58</td>
            <td>95.50%</td>
            <td>10.4</td>
            <td>95.80%</td>
            <td>100.00%</td>
            <td>76.9</td>
            <td>NDA</td>
            <td>26</td>
            <td>24.7</td>
            <td>61.8</td>
            <td>49.7</td>
            <td>39.2</td>
            <td>27.2</td>
            <td>69.7</td>
            <td>60.6</td>
            <td>9.1</td>
            <td>18.2</td>
            <td>11.1</td>
            <td>9.6</td>
            <td>0.9</td>
            <td>2.4</td>
            <td>Green</td>
            <td>Green</td>
            <td>42.9</td>
            <td>25</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>556</td>
            <td>42</td>
            <td>NDA</td>
            <td>1164504.29</td>
            <td>1873959.199</td>
            <td>41.8097569</td>
            <td>-87.6721446</td>
            <td>61</td>
            <td>NEW CITY</td>
            <td>20</td>
            <td>9</td>
            <td>(41.8097569, -87.6721446)</td>
        </tr>
        <tr>
            <td>610513</td>
            <td>Air Force Academy High School</td>
            <td>HS</td>
            <td>3630 S Wells St</td>
            <td>Chicago</td>
            <td>IL</td>
            <td>60609</td>
            <td>(773) 535-1590</td>
            <td>http://schoolreports.cps.edu/SchoolProgressReport_Eng/Spring2011Eng_610513.pdf</td>
            <td>Southwest Side High School Network</td>
            <td>SOUTHWEST SIDE COLLABORATIVE</td>
            <td>NDA</td>
            <td>Standard</td>
            <td>Not on Probation</td>
            <td>Not Enough Data</td>
            <td>Yes</td>
            <td>Average</td>
            <td>49.0</td>
            <td>Strong</td>
            <td>60</td>
            <td>Strong</td>
            <td>60.0</td>
            <td>Average</td>
            <td>55.0</td>
            <td>Weak</td>
            <td>45</td>
            <td>Average</td>
            <td>54</td>
            <td>Average</td>
            <td>53</td>
            <td>Average</td>
            <td>49</td>
            <td>93.30%</td>
            <td>15.6</td>
            <td>96.90%</td>
            <td>100.00%</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>None</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>14.6</td>
            <td>14.8</td>
            <td>NDA</td>
            <td>16</td>
            <td>1.4</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>NDA</td>
            <td>302</td>
            <td>40</td>
            <td>91.8</td>
            <td>1175177.622</td>
            <td>1880745.126</td>
            <td>41.82814609</td>
            <td>-87.63279369</td>
            <td>34</td>
            <td>ARMOUR SQUARE</td>
            <td>11</td>
            <td>9</td>
            <td>(41.82814609, -87.63279369)</td>
        </tr>
    </tbody>
</table>

[//]: # ()
[//]: # (| Column                            | Description                  |)

[//]: # (|-----------------------------------|------------------------------|)

[//]: # (| School_ID                         | Unique school ID             |)

[//]: # (| NAME_OF_SCHOOL                    | School name                  |)

[//]: # (| SAFETY_SCORE                      | Safety score                 |)

[//]: # (| AVERAGE_STUDENT_ATTENDANCE        | Attendance rate              |)

[//]: # (| COMMUNITY_AREA_NUMBER             | Community location           |)

### 3️⃣ [CHICAGO_CRIME_DATA]("data/ChicagoCrimeData.csv")

<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>CASE_NUMBER</th>
            <th>DATE</th>
            <th>BLOCK</th>
            <th>IUCR</th>
            <th>PRIMARY_TYPE</th>
            <th>DESCRIPTION</th>
            <th>LOCATION_DESCRIPTION</th>
            <th>ARREST</th>
            <th>DOMESTIC</th>
            <th>BEAT</th>
            <th>DISTRICT</th>
            <th>WARD</th>
            <th>COMMUNITY_AREA_NUMBER</th>
            <th>FBICODE</th>
            <th>X_COORDINATE</th>
            <th>Y_COORDINATE</th>
            <th>YEAR</th>
            <th>LATITUDE</th>
            <th>LONGITUDE</th>
            <th>LOCATION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>3512276</td>
            <td>HK587712</td>
            <td>2004-08-28</td>
            <td>047XX S KEDZIE AVE</td>
            <td>890</td>
            <td>THEFT</td>
            <td>FROM BUILDING</td>
            <td>SMALL RETAIL STORE</td>
            <td>0</td>
            <td>0</td>
            <td>911</td>
            <td>9</td>
            <td>14.0</td>
            <td>58.0</td>
            <td>6</td>
            <td>1155838.0</td>
            <td>1873050.0</td>
            <td>2004</td>
            <td>41.8074405</td>
            <td>-87.70395585</td>
            <td>(41.8074405, -87.703955849)</td>
        </tr>
        <tr>
            <td>3406613</td>
            <td>HK456306</td>
            <td>2004-06-26</td>
            <td>009XX N CENTRAL PARK AVE</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>OTHER</td>
            <td>0</td>
            <td>0</td>
            <td>1112</td>
            <td>11</td>
            <td>27.0</td>
            <td>23.0</td>
            <td>6</td>
            <td>1152206.0</td>
            <td>1906127.0</td>
            <td>2004</td>
            <td>41.89827996</td>
            <td>-87.71640551</td>
            <td>(41.898279962, -87.716405505)</td>
        </tr>
        <tr>
            <td>8002131</td>
            <td>HT233595</td>
            <td>2011-04-04</td>
            <td>043XX S WABASH AVE</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>NURSING HOME/RETIREMENT HOME</td>
            <td>0</td>
            <td>0</td>
            <td>221</td>
            <td>2</td>
            <td>3.0</td>
            <td>38.0</td>
            <td>6</td>
            <td>1177436.0</td>
            <td>1876313.0</td>
            <td>2011</td>
            <td>41.81593313</td>
            <td>-87.62464213</td>
            <td>(41.815933131, -87.624642127)</td>
        </tr>
        <tr>
            <td>7903289</td>
            <td>HT133522</td>
            <td>2010-12-30</td>
            <td>083XX S KINGSTON AVE</td>
            <td>840</td>
            <td>THEFT</td>
            <td>FINANCIAL ID THEFT: OVER $300</td>
            <td>RESIDENCE</td>
            <td>0</td>
            <td>0</td>
            <td>423</td>
            <td>4</td>
            <td>7.0</td>
            <td>46.0</td>
            <td>6</td>
            <td>1194622.0</td>
            <td>1850125.0</td>
            <td>2010</td>
            <td>41.74366532</td>
            <td>-87.56246276</td>
            <td>(41.743665322, -87.562462756)</td>
        </tr>
        <tr>
            <td>10402076</td>
            <td>HZ138551</td>
            <td>2016-02-02</td>
            <td>033XX W 66TH ST</td>
            <td>820</td>
            <td>THEFT</td>
            <td>$500 AND UNDER</td>
            <td>ALLEY</td>
            <td>0</td>
            <td>0</td>
            <td>831</td>
            <td>8</td>
            <td>15.0</td>
            <td>66.0</td>
            <td>6</td>
            <td>1155240.0</td>
            <td>1860661.0</td>
            <td>2016</td>
            <td>41.7734553</td>
            <td>-87.70648047</td>
            <td>(41.773455295, -87.706480471)</td>
        </tr>
    </tbody>
</table>

[//]: # ()
[//]: # (| Column                  | Description              |)

[//]: # (|-------------------------|--------------------------|)

[//]: # (| ID                      | Crime record ID          |)

[//]: # (| CASE_NUMBER             | Police case number       |)

[//]: # (| PRIMARY_TYPE            | Crime category           |)

[//]: # (| DESCRIPTION             | Crime description        |)

[//]: # (| LOCATION_DESCRIPTION    | Location of crime        |)

[//]: # (| ARREST                  | Arrest made &#40;0/1&#41;        |)

[//]: # (| YEAR                    | Year                     |)

[//]: # (| COMMUNITY_AREA_NUMBER   | Community location       |)

## 🔄 Setup & Data Loading (Python ETL)

1. Install dependencies:

```bash
pip install -r requirements.txt
```

2. Run the following in a Jupyter Notebook (or Python script):

```python
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

3. Using magic commands to connect to the SQLite database with prefixed code, `*%%sql*` for cell magic and `*%sql*` for line magic as shown below:
```python
%load_ext sql
%sql sqlite:///FinalDB.db
```

## Analytical Questions, SQL Queries & Outcomes
### 1.  What is the total number of crimes recorded in the dataset?
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

### 2.  Which communities have a per capita income < $11,000?
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


### 3.  Which crimes involve minors?
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

### 4.  Are there any kidnapping incidents specifically involving a child?
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

### 5.  What types of crimes (distinct) have been recorded at school locations?
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


### 6.  What is the average safety score for each type of school (Elementary, Middle, High)?

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

### 8. Which community area number has the highest number of recorded crimes?
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

### 9.  Which community has the highest hardship index?
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

### 10. Which community (by name) recorded the highest number of crimes?
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

---
## Key Takeaways
 - Crime is unevenly distributed across Chicago communities
 - High poverty and hardship indices strongly correlate with crime concentration
 - Schools are common locations for various crime types
 - Relational databases + SQL enable powerful cross-domain insights
 - Clean data pipelines are essential for urban analytics
 - This project showcases how integrated data analysis can inform urban policy and community interventions.


## Final Note
This project shows end-to-end analytical ownership:  
Data ingestion → Database design → SQL analysis → Interpretation — all using real urban data.  

[//]: # (This is how civic data becomes evidence.)

---

### Connect with me:
* **LinkedIn:** [linkedin.com/in/emycodes](https://linkedin.com/in/emycodes)
* **Role Interests:** Data Analyst, Business Intelligence Analyst, Product Analyst.

---
*"Data is most powerful when it serves as a clear, honest bridge between raw numbers and strategic growth."*

---

©️ **EmyCodes | 2026**
