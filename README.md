# UK Property Price official data 
This project just focuses on using Python for data cleaning and handling, Power Query and DAX to for more data transformation and Power BI to create the dashboard.
## Data from Kaggle 
https://www.kaggle.com/datasets/lorentzyeung/price-paid-data-202304
## KPI Questions
________________________________________
# Data Cleaning Steps
## Python Data Cleaning
-	Data Loading and Initial Inspection
``` 
import pandas as pd
import csv

filepath ='C:/Users/dines/Downloads/archive/pp-complete.csv'
file_object=open(filepath, 'r')

df_header = ['price', 'transfer_date', 'postcode', 'property_type',
    'is_new_build', 'duration', 'paon', 'saon', 'street', 'locality',
    'town_city', 'district', 'county', 'ppd_category_type', 'record_status']    

df = pd.read_csv(file_object, header = None, names = df_header)
print(df.head())
```
-	 Data Type Conversion and Column Dropping
```
df['transfer_date'] = pd.to_datetime(df['transfer_date'], format='%Y-%m-%d %H:%M')
df['price'] = pd.to_numeric(df['price'])
df = df.drop(columns=['ppd_category_type', 'record_status', 'saon', ], errors='ignore')
```
