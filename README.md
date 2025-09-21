# UK Property Price official data 
This project just focuses on using Python for data cleaning and handling, Power Query and DAX to for more data transformation and Power BI to create the dashboard.
## Data from Kaggle 
https://www.kaggle.com/datasets/lorentzyeung/price-paid-data-202304
- file too big to attach for final file after cleaning. 
## KPI Questions
- What is the total number of property transactions? 
- What is the overall average property price? 
- What is the average price of a property by its type? 
- What is the most frequently occurring postcode in the dataset? 
- What is the average price of properties in each county? 
- What is the distribution of property types sold (e.g., Detached, Semi-Detached)? 
- How does the average property price change over time? 
- What is the breakdown of sales by property duration (Freehold vs. Leasehold)? 
- How do the average prices of new builds compare to existing properties? 
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

## Loading data in Power BI and using Power Query to transform data. 

### Data Transformation in Power Query
The data used in this project was transformed using Power Query within Power BI. The following steps were executed to clean and prepare the raw data for analysis.

- Initial Data Structuring - <br/>
The process began by promoting the first row of the raw data to serve as the column headers, giving clear names to each data field. Following this, the data types of key columns were set correctly: price was designated as Currency, transfer_date as Date, and postcode as Text. This ensures that subsequent operations and calculations are performed accurately.

- Data Cleaning and Standardization - <br/>
To ensure data quality, several cleaning steps were performed:

- Filtering - <br/>
Any rows with a missing or blank postcode were removed from the dataset to ensure every record is complete.

- Text Formatting - <br/>
Key text columns, including paon, street, locality, town_city, district, and county, were converted to uppercase and had all leading and trailing whitespace removed. This standardization is crucial for consistent filtering and grouping of geographical data.

- Data Consolidation and Enrichment - <br/>
To create more useful fields, some columns were combined and new ones were generated:

- Combining Columns - <br/>
The paon (Primary Addressable Object Name) and street columns were merged to create a new, consolidated Street column, providing a more complete street address in a single field.

- Date Extraction - <br/>
A new column named Transer Date Year was added to the table, which extracts just the year from the transfer_date field. This enables powerful analysis by year without having to use the full date.

- Final Output - <br/>
Finally, the columns were reordered into a logical and user-friendly sequence, with key identifiers like price, transfer_date, and postcode placed at the beginning. This provides a clean, organized, and ready-to-use dataset for building visualizations and reports in Power BI.


###  Data Analysis (DAX)
Several DAX measures were created to perform key calculations and provide insights:

- Total Transactions- <br/>
A simple COUNTROWS measure to show the total number of property sales.

- Count of New Builds- <br/>
A CALCULATE measure that counts new builds while respecting filters on other columns, but not on the is_new_build column itself.

- Most Frequent Postcode-  <br/>
A complex measure using SUMMARIZE, TOPN, and MAXX to find the postcode with the highest number of transactions, which dynamically updates with user-applied filters.

________________________________________
- Dashboard Interaction <a href= "https://github.com/binayaaryal12/netflix-eda/blob/main/netflix.pdf](https://github.com/binayaaryal12/Ukpropertyofficialdata/blob/main/ukpricing.pdf"> View Dashboard</a> 



