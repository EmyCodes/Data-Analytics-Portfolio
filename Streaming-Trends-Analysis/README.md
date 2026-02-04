# Exploring Streaming Trends of the 1990s
**Technical Research Case Study | Python Data Exploration**

## 📌 Project Objective
In this research project, I acted as a Data Analyst for a production company specializing in nostalgic media styles. The goal was to perform an **Exploratory Data Analysis (EDA)** on a comprehensive dataset of movie titles to uncover specific content patterns from the **1990s**. This research serves to provide the creative team with data-backed insights into "traditional" movie structures to guide new "nostalgic" productions.

---

## 🛠️ Technical Toolkit
* **Language:** Python
* **Libraries:** Pandas (Data Manipulation), Matplotlib (Visualization)
* **Environment:** Jupyter Notebook / DataLab
* **Techniques:** Boolean Indexing, String Vectorization, Descriptive Statistics, Temporal Filtering

---

## 🧮 Research Methodology
To extract the required insights, I executed a multi-stage data pipeline:

1. I loaded the `netflix_data.csv` and audited the schema, checking for data types and general structure across columns like `type`, `title`, `release_year`, and `duration`.
2. I isolated the "1990s Decade" subset by filtering the `release_year` for values $\ge$ 1990 and $<$ 2000.
3. I utilized the `.mode()` function to identify the most standard movie length from that decade, providing a benchmark for the "traditional" theatrical run-time of that era.
4.  I applied string filtering to isolate 'Action' movies and categorized them by duration to count "short-form" content (under 90 minutes).

---

## 📊 Findings & Statistical Outcome
The analysis yielded specific quantitative benchmarks that define the movie landscape of the 1990s:

* My analysis identified that the **most frequent movie duration (Mode)** in the 1990s was **94 minutes**. This suggests that contemporary "nostalgic" productions should aim for a sub-100 minute runtime to authentically replicate the pacing of that decade.
* Despite the 90s being an era of major blockbusters, I found that there were only **7 short-form action movies** (under 90 minutes) in the dataset. This indicates that 90s action content was predominantly longer-form, making these 7 titles significant outliers for study.


---

## 💻 Technical Implementation

```python
# Importing pandas and matplotlib
import pandas as pd
import matplotlib.pyplot as plt

# Read in the Netflix CSV as a DataFrame
netflix_df = pd.read_csv("netflix_data.csv")
```

```python
import pandas as pd
import matplotlib.pyplot as plt
"""Perform exploratory data analysis 
on the netflix_data.csv data to understand more 
about movies from the 1990s decade.
"""
# Read in the Netflix CSV as a DataFrame
netflix_df = pd.read_csv("netflix_data.csv")

"""1. What was the most frequent movie 
duration in the 1990s? Save an approximate answer 
as an integer called duration 
(use 1990 as the decade's start year).
"""

# Segmenting the dataset for the 1990s decade
movies_1990s = netflix_df[(netflix_df['release_year'] >= 1990) & (netflix_df['release_year'] < 2000)]

# Identifying the most frequent movie duration (Mode)
# Result: 94 minutes
duration = movies_1990s['duration'].mode()[0]

# Filtering for 'Action' movies with a duration under 90 minutes
# Result: 7 movies
short_duration_threshold = 90
short_movie_count = movies_1990s[(movies_1990s['genre'].str.contains('Action')) & 
                                (movies_1990s['duration'] < short_duration_threshold)].shape[0]

print(f"Most Frequent Movie Duration (1990s): {duration}")
print(f"Number of Short Action Movies (1990s): {short_movie_count}")
```
**Note**: *This project is part of my Python Data Exploration series, focusing on using programmatic logic to solve industry-specific research questions.*

---
*This project is part of my "Practice to Perfection" journey. I am currently expanding this portfolio to include SQL and Python projects.*


**Connect with me:** [LinkedIn](https://linkedIn.com/in/emycodes)

---

©️ **EmyCodes | 2026**