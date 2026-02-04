# Stock vs. Revenue: Automated Data Extraction & Visualization

## 📌 Project Overview
This project was completed as the **final assignment for the IBM Python Project for Data Science**. It represents a deliberate shift from **theoretical mathematics to practical data engineering**, focusing on how raw financial data can be harvested, transformed, and translated into insight.

Rather than relying on static CSV files, this project builds an **automated data pipeline** that pulls **live stock prices** via an API and **quarterly revenue data** via web scraping. The relationship between **market valuation** and **company fundamentals** is explored using two contrasting case studies:

- **Tesla (TSLA)** – a growth-driven technology company  
- **GameStop (GME)** – a volatility-driven market anomaly  

The result is a clean, reproducible workflow that extracts, cleans, integrates, and visualizes financial data end-to-end.

---

## Technical Stack & Libraries

- **API Extraction:** `yfinance` – real-time and historical stock data  
- **Web Scraping:** `requests` (HTML retrieval), `BeautifulSoup` (DOM parsing)  
- **Data Manipulation:** `pandas` – ETL (Extract, Transform, Load)  
- **Visualization:** `matplotlib` – custom financial dashboards  

---

## Key Problems Solved & Solutions

### 1. Live Web Harvesting (BeautifulSoup Solution)
Quarterly revenue data was embedded in **unstructured HTML tables**, not available via API.

**Solution:**  
HTML was fetched using `requests`, parsed with `BeautifulSoup`, and the **second `<tbody>` element** was explicitly targeted to isolate **quarterly revenue**, excluding annual summaries.

---

### 2. Multi-Source Integration (ETL Challenge)
Stock prices (API) and revenue figures (scraped HTML) came from different sources with different formats.

**Solution:**  
A structured ETL pipeline was applied:
- Regex-based cleaning to remove `$` and `,`
- Removal of missing and empty values
- Index resetting and date alignment  
This produced synchronized datasets suitable for comparative visualization.

---

## Full Python Implementation

### Setup & Custom Dashboard Function: 
### 1. Installed required Libraries
```bash
!pip install -r requirements.txt
```
### 2. Imported neccassary python Libraries
```python
import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import matplotlib.pyplot as plt
```
### 3. Developed Custom function to visualize the relationship between stock price and revenue

```py
def make_graph(stock_data, revenue_data, stock):
    stock_data_specific = stock_data[stock_data.Date <= '2021-06-14']
    revenue_data_specific = revenue_data[revenue_data.Date <= '2021-04-30']

    fig, axes = plt.subplots(2, 1, figsize=(12, 8), sharex=True)

    # Stock price plot
    axes[0].plot(
        pd.to_datetime(stock_data_specific.Date),
        stock_data_specific.Close.astype("float")
    )
    axes[0].set_ylabel("Price ($US)")
    axes[0].set_title(f"{stock} - Historical Share Price")

    # Revenue plot
    axes[1].plot(
        pd.to_datetime(revenue_data_specific.Date),
        revenue_data_specific.Revenue.astype("float")
    )
    axes[1].set_ylabel("Revenue ($US Millions)")
    axes[1].set_xlabel("Date")
    axes[1].set_title(f"{stock} - Historical Revenue")

    plt.tight_layout()
    plt.show()
```
### Tesla Pipeline: API + Web Scraping
```py
# Extract Tesla stock data
tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")
tesla_data.reset_index(inplace=True)

# Scrape Tesla revenue data
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
html_data = requests.get(url).text
beautiful_soup = BeautifulSoup(html_data, "html.parser")

tesla_revenue = pd.DataFrame(columns=["Date", "Revenue"])
table_body = beautiful_soup.find_all("tbody")[1]

for row in table_body.find_all("tr"):
    col = row.find_all("td")
    date, revenue = col[0].text, col[1].text
    tesla_revenue = pd.concat(
        [tesla_revenue, pd.DataFrame({"Date": [date], "Revenue": [revenue]})],
        ignore_index=True
    )

# Data cleaning
tesla_revenue["Revenue"] = tesla_revenue["Revenue"].str.replace(r"[,\$]", "", regex=True)
tesla_revenue.dropna(inplace=True)
tesla_revenue = tesla_revenue[tesla_revenue["Revenue"] != ""]

# Visualization
# make_graph(tesla_data, tesla_revenue, "Tesla")
```

### GameStop Pipeline: API + Web Scraping
```py
# Extract GameStop stock data
gme = yf.Ticker("GME")
gme_data = gme.history(period="max")
gme_data.reset_index(inplace=True)

# Scrape GameStop revenue data
url_gme = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
html_data_2 = requests.get(url_gme).text
beautiful_soup = BeautifulSoup(html_data_2, "html.parser")

gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])
table_body_gme = beautiful_soup.find_all("tbody")[1]

for row in table_body_gme.find_all("tr"):
    col = row.find_all("td")
    date, revenue = col[0].text, col[1].text
    gme_revenue = pd.concat(
        [gme_revenue, pd.DataFrame({"Date": [date], "Revenue": [revenue]})],
        ignore_index=True
    )

# Data cleaning
gme_revenue["Revenue"] = gme_revenue["Revenue"].str.replace(r"[,\$]", "", regex=True)
gme_revenue.dropna(inplace=True)
gme_revenue = gme_revenue[gme_revenue["Revenue"] != ""]

# Visualization
# make_graph(gme_data, gme_revenue, "GameStop")
```

## 📊 Visual Dashboards & Analysis
### Tesla (TSLA)
```py
tesla_revenue.tail()
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48</th>
      <td>2010-09-30</td>
      <td>31</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2010-06-30</td>
      <td>28</td>
    </tr>
    <tr>
      <th>50</th>
      <td>2010-03-31</td>
      <td>21</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2009-09-30</td>
      <td>46</td>
    </tr>
    <tr>
      <th>53</th>
      <td>2009-06-30</td>
      <td>27</td>
    </tr>
  </tbody>
</table>
</div>

```py
make_graph(tesla_data, tesla_revenue, "Tesla")
```
![Tesla Dashboard](images/tesla_graph.png)

The dashboard shows a strong coupling between revenue growth and stock appreciation, particularly during Tesla’s late-2020 expansion phase. The market appears to price in scaling production and future earnings.

---
### GameStop (GME)
```py
gme_revenue.tail()
```
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>57</th>
      <td>2006-01-31</td>
      <td>1667.0</td>
    </tr>
    <tr>
      <th>58</th>
      <td>2005-10-31</td>
      <td>534.0</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2005-07-31</td>
      <td>416.0</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2005-04-30</td>
      <td>475.0</td>
    </tr>
    <tr>
      <th>61</th>
      <td>2005-01-31</td>
      <td>709.0</td>
    </tr>
  </tbody>
</table>
</div>

```py
make_graph(gme_data, gme_revenue, "GameStop")
```

![GameStop Dashboard](images/gme_graph.png)
In the Dashboard above, a clear decoupling emerges in early 2021. Revenue remains relatively flat while the stock price experiences an extreme, sentiment-driven surge—an illustration of market psychology overpowering fundamentals.

---

### 📈 Practice to Perfection

I am a 3MTT Fellow and ForbesBLK Member with a background in Industrial Mathematics (FUTA). This project reflects my commitment to moving beyond theory into applied data analytics and engineering, where data is not just collected—but made to speak.

>*Data is most powerful when it serves as a clear, honest bridge between raw numbers and strategic growth.*

---

**Connect with Me**:

**LinkedIn**: [https://linkedin.com/in/emycodes](https://linkedin.com/in/emycodes)

**GitHub Portfolio**: [https://github.com/EmyCodes/Data-Analytics-Portfolio](https://github.com/EmyCodes/Data-Analytics-Portfolio)

---

©️ **EmyCodes | 2026**