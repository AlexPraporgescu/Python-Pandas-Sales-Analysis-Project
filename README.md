  # Python Pandas Sales Analysis

  ## <ins>Table of Contents</ins>

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Preparation and Cleaning](#data-preparation-and-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Results and Findings](#results-and-findings)
- [Limitations](#limitations)
- [References](#references)
- [Notes](#notes)

## <ins>Project Overview</ins>

The goal of this project is to analyze sales data to gain insights into various aspects of the sales operation.
This includes cleaning the data, performing data exploration and answering specific questions related to sales performance.

  ## <ins>Data sources</ins>

The dataset used for this analysis includes 12 month worth of information about sales orders, such as order details, products, quantities, prices and more.
The dataset was obtained from [www.kaggle.com](https://www.kaggle.com/datasets/zulkhaireesulaiman/sales-analysis-2019-excercise).

  ## <ins>Tools</ins>

 - Python Pandas and Python Matplotlib - Data preparation, data cleaning, data analysis, data visualisation.

  ## <ins>Data Preparation and Cleaning</ins>

  ### Importing necessary libraries

```python
import pandas as pd
import matplotlib.pyplot as plt
```

  ### Importing all sales data

```python
df1 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_January_2019.csv")
df2 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_February_2019.csv")
df3 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_March_2019.csv")
df4 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_April_2019.csv")
df5 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_May_2019.csv")
df6 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_June_2019.csv")
df7 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_July_2019.csv")
df8 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_August_2019.csv")
df9 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_September_2019.csv")
df10 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_October_2019.csv")
df11 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_November_2019.csv")
df12 = pd.read_csv("D:\Data Analysis\Sales Data Pandas Project\Sales_December_2019.csv")
```

  ### Merging all sales data into a single dataframe

```python
df = pd.concat([df1, df2, df3, df4, df5, df5, df6, df7, df8, df9, df10, df11, df12])
```

  ### Dropping rows with missing data

```python
df = df.dropna(how="all")
```

 ### Deleting duplicated data

```python
df = df.drop_duplicates()
df = df[df["Order ID"] != "Order ID"]
```

  ### Converting columns to appropriate data types

```python
df["Product"] = df["Product"].astype(str)
df["Purchase Address"] = df["Purchase Address"].astype(str)
df["Order ID"] = df["Order ID"].astype(int)
df["Quantity Ordered"] = df["Quantity Ordered"].astype(int)
df["Price Each"] = df["Price Each"].astype(float)
df["Order Date"] = pd.to_datetime(df["Order Date"], format="mixed")
```

  ## <ins>Exploratory Data Analysis</ins>

  ### Question 1: What was the best month for sales? How much was earned that month?

 - Adding a "Month" column:

```python
df["Month"] = df["Order Date"].dt.month
```

 - Adding a "Sales" column:

```python
df["Sales"] = df["Quantity Ordered"] * df["Price Each"]
```

 - Best month for sales:

```python
results1 = df.groupby("Month").sum("Sales")
```

```python
months = range(1,13)
plt.bar(months, results1["Sales"])
plt.xticks(months)
plt.ylabel("Sales in USD")
plt.xlabel("Month Number")
plt.show()
```

![image](https://github.com/AlexPraporgescu/Python-Pandas-Sales-Analysis-Project/assets/158141333/af9c41de-8315-4843-b07c-74910af86b05)

 ### Question 2: What city had the highest number of sales?

 - Creating "Street", "City", "Zip" columns:

```python
df[["Street", "City", "Zip"]] = df['Purchase Address'].str.split(', ',expand=True)
```

 - City with highest number of sales:

```python
results2 = df.groupby("City").sum("Sales")
```

```python
city = [city for city, df13 in df.groupby("City")]
plt.bar(city, results2["Sales"])
plt.xticks(city, rotation="vertical", size=8)
plt.ylabel("Sales in USD")
plt.xlabel("City Name")
plt.show()
```

![image](https://github.com/AlexPraporgescu/Python-Pandas-Sales-Analysis-Project/assets/158141333/2ace31a9-d76a-49d8-8ac8-c0e1bad30de2)

 ### Question 3: What time should advertisements be displayed to maximize likelihood of customers buying products?

 - Creating an "Order Hour" column:

```python
df["Order Hour"] = df["Order Date"].dt.hour
```

 - Times advertisements should be displayed:

```python
hour = [hour for hour, df14 in df.groupby("Order Hour")]
plt.plot(hour, df.groupby(["Order Hour"]).count())
plt.xticks(hour)
plt.ylabel("Sales")
plt.grid()
plt.xlabel("Hour")
plt.show()
```

![image](https://github.com/AlexPraporgescu/Python-Pandas-Sales-Analysis-Project/assets/158141333/bb4c1e4c-8090-4da9-8fca-622961535bef)

 ### Question 4: What products are most often sold together?

 - Preparing a new dataframe by grouping the products with same Order ID in a new column "Grouped":

```python
df15 = df[df["Order ID"].duplicated(keep=False)]
df15["Grouped"] = df15.groupby("Order ID")["Product"].transform(lambda x: ",".join(x))
```

```python
df15 = df15[["Order ID", "Grouped"]].drop_duplicates()
```

 - Products often sold together:

```python
from itertools import combinations
from collections import Counter
count = Counter()
for row in df15["Grouped"]:
    row_list = row.split(",")
    count.update(Counter(combinations(row_list, 2)))
count.most_common(10)
```

 ### Question 5: What product sold the most?

 - The most sold product:

```python
results3 = df.groupby("Product").sum("Quantity Ordered").sort_values(['Quantity Ordered'], ascending=[False])
```

```python
product = [product for product, results5 in df.groupby("Product")]
plt.bar(product, results4["Quantity Ordered"])
plt.ylabel("Quantity Ordered")
plt.xlabel("Product")
plt.xticks(product, rotation="vertical", size=8)
plt.show()
```

![image](https://github.com/AlexPraporgescu/Python-Pandas-Sales-Analysis-Project/assets/158141333/13c9c551-fc59-4f28-a415-49fb78bf41e5)

  ### <ins>Results and Findings</ins>

Some of the most important insights are:
 - The best month for sales was December when sales reached $4608295.70.
 - The city with the highest number of sales was San Francisco with sales reaching $8254743.55.
 - Based on the most sales per hour, the advertisements should be displayed arround 11am and 7pm.
 - The products that are most often sold toghether are iPhone and Lighting Charging Cable.
 - The product that sold the most was AAA Batteries (4-pack).

  ### <ins>Limitations</ins>

 - The dataset that was used in the analysis contained fictios data intended for practice use only. Therefore the results are not to be taken into account
for real life purposes.

  ### <ins>References</ins>

1. [Youtube](https://www.youtube.com/)
2. [Stack Overflow](https://stackoverflow.com/)
3. [Kaggle.com](https://www.kaggle.com/)

  ### <ins>Notes</ins>

- This project represents my practice work in the field of data analysis. The dataset used does not represent the reality and therefore the findings should be treated in accordance.
- For more details about me and my work, or if you want to get in touch with me, please access my [LinkedIn profile](https://www.linkedin.com/in/alexpraporgescu/).
