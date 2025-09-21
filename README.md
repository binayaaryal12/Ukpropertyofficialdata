# UK Property Price official data 
This project just focuses on using Python for data cleaning and handling, Power Query and DAX to for more data transformation and Power BI to create the dashboard.
## Data from Kaggle 
https://www.kaggle.com/datasets/lorentzyeung/price-paid-data-202304
## KPI Questions
________________________________________
# Data Cleaning Steps
## Python Data Cleaning
### Data Loading and Initial Inspection
``` 
import pandas as pd
**import csv**

filepath ='C:/Users/dines/Downloads/archive/pp-complete.csv'
file_object=open(filepath, 'r')

df_header = ['price', 'transfer_date', 'postcode', 'property_type',
    'is_new_build', 'duration', 'paon', 'saon', 'street', 'locality',
    'town_city', 'district', 'county', 'ppd_category_type', 'record_status']    

df = pd.read_csv(file_object, header = None, names = df_header)
print(df.head())
```
### Data Type Conversion and Column Dropping
  
```
df['transfer_date'] = pd.to_datetime(df['transfer_date'], format='%Y-%m-%d %H:%M')
df['price'] = pd.to_numeric(df['price'])
df = df.drop(columns=['ppd_category_type', 'record_status', 'saon', ], errors='ignore')
```

- **pd.to_datetime() converts the transfer_date column from a string to a proper datetime object, which is essential for any time-based analysis.<b>**

- **pd.to_numeric() converts the price column to a numeric type, allowing for mathematical calculations.<b>**

- **df.drop() removes columns that are not needed for the analysis: ppd_category_type, record_status, and saon. The errors='ignore' argument prevents the code from crashing if a specified column doesn't exist.<b>**

### Handling Missing Values and Data Cleaning

```
print(df.isnull().sum())     
df.dropna(subset=['postcode'], inplace=True)
```

- **print(df.isnull().sum()) calculates and prints the number of missing values (nulls) in each column. This is a crucial step for assessing data quality.**
  
- **df.dropna(subset=['postcode'], inplace=True) removes any rows where the postcode column has a missing value. inplace=True modifies the DataFrame directly.**

### Categorical Data Transformation

```
df['property_type'] = df['property_type'].replace(...)
df['is_new_build'] = df['is_new_build'].replace(...)
df['duration'] = df['duration'].replace(...)
```

- **use the .replace() method to substitute single-letter codes with more descriptive text.** 
- **property_type codes like 'D', 'S', and 'T' are replaced with 'Detached', 'Semi-Detached', and 'Terraced'.**
- **is_new_build codes 'Y' and 'N' become 'New Build' and 'Existing Property'.**
- **duration codes 'F' and 'L' are replaced with 'Freehold' and 'Leasehold'**

### Standardizing Text Columns
```
df['street'] = df['street'].str.upper().str.strip()
df['town_city'] = df['town_city'].str.upper().str.strip()
```

- **str.upper() converts all text to uppercase, ensuring consistency (e.g., 'Main Street' and 'main street' are treated the same).**
- **str.strip() removes any leading or trailing whitespace, which is a common source of data inconsistencies and can cause problems when matching or filtering data.**

### Outlier Removal
```
lower_bound = df['price'].quantile(0.01)
upper_bound = df['price'].quantile(0.99)
df = df[(df['price'] >= lower_bound) & (df['price'] <= upper_bound)]
```

- **This part of the script identifies and removes outliers in the price column.**
- **df['price'].quantile(0.01) finds the value below which 1% of the data falls (the 1st percentile).**
- **df['price'].quantile(0.99) finds the value below which 99% of the data falls (the 99th percentile).**








