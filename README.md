# Model-Deployment-with-Streamlit
```
Directory structure:
└── kaveenpramuditha-model-deployment-with-streamlit/
    ├── README.md
    ├── requirements.txt
    ├── data/
    │   ├── Boston.csv
    │   └── Boston_cleaned.csv
    └── notebooks/
        └── model_training.ipynb
```
```
================================================
FILE: requirements.txt
================================================
```
```
 pandas
numpy
matplotlib
seaborn
scikit-learn
streamlit

================================================
FILE: notebooks/model_training.ipynb
================================================
# Jupyter notebook converted to Python script.

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import streamlit as st
import sklearn as sk

# Load the data
df = pd.read_csv('../data/Boston.csv')

#  Shape of dataset 
print(df.shape)

# Column data types
print(df.dtypes)

# First and last 5 rows
print(df.head())
print(df.tail())

#  Stat summary
print(df.describe())


# Find missing values
missing = df.isnull().sum()
missing = missing[missing > 0]
print(missing)

# Percentage of missing values
missing_percent = (missing / len(df)) * 100
print(missing_percent)

# Visualize using heatmap
sns.heatmap(df.isnull(), cbar=False)
plt.title("Missing Data Heatmap")
plt.show()

# Output:
#   Series([], dtype: int64)

#   Series([], dtype: float64)

#   <Figure size 640x480 with 1 Axes>

duplicates = df.duplicated()
print("Number of exact duplicates:", duplicates.sum())



# Output:
#   Number of exact duplicates: 0


# Use describe to spot strange values
print(df[['AGE', 'TAX', 'DIS',]].describe())

# Optional: visual boxplot
sns.boxplot(x=df['AGE'])
plt.title("Boxplot of AGE")
plt.show()



# Check if any rows have all guests as 0
guest_zeros = df[(df['AGE'] == 0) & (df['INDUS'] == 0) & (df['TAX'] == 0)]
print("Rows where all guests are 0:", guest_zeros.shape[0])

# Output:
#   Rows where all guests are 0: 0


"""
**Data Cleaning Implementation**
"""

# Check duplicates
duplicates = df.duplicated()
print("Number of exact duplicate rows:", duplicates.sum())

# Drop duplicates if found
df = df.drop_duplicates()
print("Shape after removing duplicates:", df.shape)

# Output:
#   Number of exact duplicate rows: 0

#   Shape after removing duplicates: (506, 14)


def remove_outliers_iqr(df, column):
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    original = df.shape[0]
    df_cleaned = df[(df[column] >= lower) & (df[column] <= upper)]
    print(f"{column}: Removed {original - df_cleaned.shape[0]} outliers")
    return df_cleaned


# (crime rate)
df = remove_outliers_iqr(df, 'CRIM')

# Apply to others if needed
for col in ['ZN', 'INDUS', 'RM', 'AGE', 'DIS', 'TAX', 'PTRATIO', 'LSTAT', 'MEDV']:
    df = remove_outliers_iqr(df, col)
