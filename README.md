# Video Games Sales with Ratings - Exploratory Data Analysis (EDA)

This repository contains Python code for exploring and analyzing video game sales data. The dataset includes information about video game titles, platforms, release years, sales across regions, and more.

## Table of Contents
- [Dataset Description](#dataset-description)
- [Setup and Installation](#setup-and-installation)
- [Data Cleaning](#data-cleaning)
- [Data Analysis](#data-analysis)
  - [Top 10 Game Releases by Year](#top-10-game-releases-by-year)
  - [Total Sales by Year](#total-sales-by-year)
  - [Top 3 and Least 3 Platforms](#top-3-and-least-3-platforms)
  - [Regional Sales Analysis](#regional-sales-analysis)

## Dataset Description
The dataset used for this analysis includes the following features:
- Name
- Platform
- Year of Release
- NA_Sales (North America Sales)
- EU_Sales (Europe Sales)
- JP_Sales (Japan Sales)
- Global_Sales
- Additional features such as Critic Scores, User Scores, Developer, and Rating (not used in the current analysis)

## Setup and Installation
1. Clone the repository and ensure you have Python installed.
2. Install the required libraries:
   ```bash
   pip install pandas numpy
   ```
3. Place the dataset (`video_games_sales_with_rateings.csv`) in the appropriate directory.

## Data Cleaning

### Handling Missing Values
- Checked and visualized missing values sorted in descending order:
  ```python
  data.isnull().sum().sort_values(ascending=False)
  ```
- Replaced missing values in the `Name` column with "N/A":
  ```python
  data["Name"] = data["Name"].fillna("N/A")
  ```

### Removing Unwanted Columns
- Dropped columns unrelated to sales analysis:
  ```python
  sales = data.drop(['Critic_Score','Critic_Count','User_Score','User_Count','Developer','Rating'], axis=1)
  ```

## Data Analysis

### Top 10 Game Releases by Year
Identified the top 10 years with the highest number of game releases:
```python
sales['Year_of_Release'].value_counts().sort_values(ascending=False).head(10)
```

### Total Sales by Year
Analyzed total global sales by year:
```python
pub_level_sales = pd.pivot_table(sales, values=['Global_Sales'], aggfunc=np.sum, index='Year_of_Release')
pub_level_sales.sort_values(ascending=False, by='Global_Sales')
```

### Top 3 and Least 3 Platforms
Identified the top 3 and least 3 gaming platforms by number of releases:
```python
top_3 = sales['Platform'].value_counts(ascending=False).head(3)
last_3 = sales['Platform'].value_counts(ascending=False).tail(3)
top_3_last_3_platforms = pd.concat([top_3.to_frame(), last_3.to_frame()])
```

### Regional Sales Analysis
Created a new dataframe to analyze regional sales (NA, EU, JP):
```python
sales_data = sales[['NA_Sales', 'EU_Sales', 'JP_Sales']]
total_sales = pd.pivot_table(sales_data, values=['NA_Sales', 'EU_Sales', 'JP_Sales'], aggfunc=np.sum, margins_name="sales", index=type)
```

## Results Interpretation
- **Top Game Release Years:** Identified years with the highest number of game launches.
- **Sales Trends:** Observed total global sales by year and the contribution of different platforms.
- **Platform Popularity:** Highlighted the most and least popular gaming platforms based on game releases.
- **Regional Sales:** Summarized total sales across North America, Europe, and Japan.


