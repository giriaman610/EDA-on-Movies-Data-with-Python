# Exploratory-Data-Analysis-on-Movies-Data
# 🎬 Movie Dataset — Exploratory Data Analysis (EDA)

A complete end-to-end EDA project on a movies dataset using Python. This project explores box office performance, genre trends, critic ratings, and actor insights through visual analysis.

---

## 📁 Project Structure

```
Movies-EDA/
│
├── Movies.ipynb           # Main Jupyter Notebook
├── movies_dataset.csv     # Dataset used for analysis
└── README.md              # Project documentation
```

---

## 📊 Dataset Overview

The dataset contains information on movies spanning multiple decades and countries, with **17 columns** covering financial performance, ratings, and metadata.

| Column | Description |
|---|---|
| `MovieID` | Unique identifier for each movie |
| `Title` | Movie title |
| `Genre` | Genre (Comedy, Horror, Drama, etc.) |
| `ReleaseYear` | Year of release |
| `ReleaseDate` | Full release date |
| `Country` | Country of origin |
| `BudgetUSD` | Production budget in USD |
| `US_BoxOfficeUSD` | US box office collection |
| `Global_BoxOfficeUSD` | Global box office collection |
| `Opening_Day_SalesUSD` | Opening day revenue |
| `One_Week_SalesUSD` | First-week revenue |
| `IMDbRating` | IMDb rating score |
| `RottenTomatoesScore` | Rotten Tomatoes critic score |
| `NumVotesIMDb` | Number of IMDb votes |
| `NumVotesRT` | Number of Rotten Tomatoes votes |
| `Director` | Director name |
| `LeadActor` | Lead actor name |

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-green?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Numerical-lightblue?logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-teal)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## 🔍 Analysis Breakdown

### 1. 📥 Data Loading & Initial Inspection
- Loaded dataset from CSV using `pd.read_csv()`
- Inspected shape, data types, and column names with `df.info()` and `df.describe()`
- Confirmed **zero null values** and **zero duplicates** — clean dataset ready for analysis

```python
df.isna().sum().sum()   # 0
df.duplicated().sum()   # 0
```

---

### 2. 📈 Univariate Analysis — Numeric Columns

Plotted distribution histograms with KDE curves for all 9 numeric columns:
- `BudgetUSD`, `US_BoxOfficeUSD`, `Global_BoxOfficeUSD`
- `Opening_Day_SalesUSD`, `One_Week_SalesUSD`
- `IMDbRating`, `RottenTomatoesScore`, `NumVotesIMDb`, `NumVotesRT`

```python
for col in numeric_columns:
    sns.histplot(data=df, x=col, bins=50, kde=True)
    plt.title(f'{col}_distribution')
    plt.show()
```
### charts
<img src="https://github.com/giriaman610/EDA-on-Movies-Data-with-Python/blob/main/Screenshot%202026-04-21%20152059.png" width="600"/>
<img src="https://github.com/giriaman610/EDA-on-Movies-Data-with-Python/blob/main/Screenshot%202026-04-21%20152828.png" width="600"/>
<img src="https://github.com/giriaman610/EDA-on-Movies-Data-with-Python/blob/main/Screenshot%202026-04-21%20153030.png" width="600"/>
<img src="https://github.com/giriaman610/EDA-on-Movies-Data-with-Python/blob/main/Screenshot%202026-04-21%20153145.png" width="600"/>

> 💡 **Key Insight:** `BudgetUSD` was highly right-skewed. A **log transformation** was applied to better visualize the actual budget spread.

```python
sns.histplot(np.log1p(df['BudgetUSD']), bins=50, kde=True)
plt.title("Log Transformed Budget Distribution")
plt.show()
```

---

### 3. 📊 Univariate Analysis — Categorical Columns

Bar charts for the **Top 20** values in each categorical column:

```python
Categorical_Columns = ['Genre', 'Country', 'Director', 'LeadActor']

for col in Categorical_Columns:
    df[col].value_counts().head(20).plot(kind='bar')
    plt.title(f"Top20_{col}_Distribution")
    plt.show()
```
<img src="https://github.com/giriaman610/EDA-on-Movies-Data-with-Python/blob/main/Screenshot%202026-04-23%20004425.png" width="600"/>
<img src="https://github.com/giriaman610/EDA-on-Movies-Data-with-Python/blob/main/Screenshot%202026-04-23%20004607.png" width="600"/>

---

### 4. 🔗 Bivariate Analysis

#### 💰 Budget vs Global Box Office
Scatter plot on a log-log scale to understand ROI patterns across movies.

```python
sns.scatterplot(data=df, x='BudgetUSD', y='Global_BoxOfficeUSD', alpha=0.3)
plt.xscale('log')
plt.yscale('log')
plt.title("Movie Budget vs Box Office Collection")
plt.show()
```

#### ⭐ IMDb vs Rotten Tomatoes
Correlation between audience ratings and critic scores.

```python
sns.scatterplot(data=df, x='IMDbRating', y='RottenTomatoesScore', alpha=0.4)
plt.title("IMDb vs Rotten Tomatoes")
plt.show()
```

---

### 5. 🎭 Genre-Level Aggregated Analysis

Grouped by `Genre` to compute average budget, global box office, IMDb rating, and RT score.

```python
genre_year_stats = df.groupby(['Genre']).agg({
    "Global_BoxOfficeUSD": 'mean',
    "BudgetUSD": 'mean',
    'IMDbRating': 'mean',
    'RottenTomatoesScore': 'mean'
}).reset_index()
```

Values were converted to **millions** for readability, followed by a **line plot** showing average Global Box Office trends across genres and years.

---

### 6. 🏆 Top Actors by IMDb Rating

Identified the top 10 lead actors with the highest average IMDb ratings across their movies.

```python
Top_Actors = df.groupby('LeadActor')['IMDbRating'].mean()\
               .sort_values(ascending=False).round(2).head(10)
```

---

## 📌 Key Takeaways

- The dataset is clean with no missing or duplicate records
- Budget distributions are heavily right-skewed — most movies operate on modest budgets
- Higher budgets show a **general positive correlation** with global box office revenue (on log scale)
- IMDb and Rotten Tomatoes scores show **moderate alignment**, but notable divergence exists
- Genre-level trends reveal distinct earning patterns over the decades
- Top actors by average IMDb rating provide a view into consistent critical performers

---

## 🚀 How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/Movies-EDA.git
   cd Movies-EDA
   ```

2. **Install dependencies**
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```

3. **Launch the notebook**
   ```bash
   jupyter notebook Movies.ipynb
   ```

> Make sure `movies_dataset.csv` is in the same directory as the notebook.

---

## 🤝 Connect with Me

If you found this project helpful or want to discuss data analytics, feel free to connect!

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/your-profile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/your-username)

---

> ⭐ If you liked this project, please consider giving it a star on GitHub!
