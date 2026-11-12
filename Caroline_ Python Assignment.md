
 Data Structures, Functions & First Look at Data
 
Author: Caroline Diana Njama 

Program: SolaviseTech Women in Data, Cohort 3  

My learning journey and goals in the SolaviseTech Women in Data Cohort 3 program. SolaviseTech is a program that helps women master Data Science & ML, Data Analysis, and Artificial Intelligence. At the end of this program, I wish to be an expert in these three domains and have multiple opportunities.

 Welcome
This beginner-friendly lesson introduces Python dictionaries and functions, then uses NumPy and pandas to explore simple inventory and sales data.

 Learning Objectives
- Create, access, update, and loop through dictionaries
- Write functions that accept inputs and return results
- Use NumPy arrays to calculate basic statistics and reshape data
- Create and inspect a pandas DataFrame
- Identify missing values and duplicate rows

 1. Setup
```python
import numpy as np
import pandas as pd

inventory = [
    {"product": "Notebook", "category": "Stationery", "price": 1500, "quantity": 40},
    {"product": "Pen", "category": "Stationery", "price": 200, "quantity": 120},
    {"product": "Backpack", "category": "Bags", "price": 12000, "quantity": 15},
    {"product": "Water Bottle", "category": "Drinkware", "price": 2500, "quantity": 0},
    {"product": "Laptop Sleeve", "category": "Bags", "price": 6500, "quantity": 8},
    {"product": "Highlighter Set", "category": "Stationery", "price": 1800, "quantity": None},
    {"product": "Coffee Mug", "category": "Drinkware", "price": 3000, "quantity": 22},
    {"product": "USB Flash Drive", "category": "Electronics", "price": 4500, "quantity": 12},
    {"product": "Earphones", "category": "Electronics", "price": 7000, "quantity": 5},
    {"product": "Pen", "category": "Stationery", "price": 200, "quantity": 120},
    {"product": "Desk Lamp", "category": "Electronics", "price": 9500, "quantity": 6},
]

prices = {"Notebook": 1500, "Pen": 200, "Backpack": 12000, "Water Bottle": 2500}
daily_sales = np.array([220, 250, 240, 230, 260, 300, 310, 235, 245, 255, 240, 265, 980, 320])
```

 2. Dictionaries
```python
product = {"product": "Notebook", "category": "Stationery", "price": 1500, "quantity": 20}
print("Price:", product["price"])
product["supplier"] = "Ade Stationers"
product["quantity"] = 35

for item, price in prices.items():
    print(f"{item} costs N{price:,}")

sale_prices = {item: price * 0.9 for item, price in prices.items()}

category_counts = {}
for item in inventory:
    category = item["category"]
    category_counts[category] = category_counts.get(category, 0) + 1
print("Products per category:", category_counts)
```
Note: Stationery shows 4 because the duplicate Pen is counted twice. This is handled in Section 5.

 3. Functions
```python
def total_value(price, quantity):
    if quantity is None:
        return 0
    return price * quantity

def apply_discount(price, percent=10):
    if price < 0:
        raise ValueError("price must not be negative")
    if not 0 <= percent <= 100:
        raise ValueError("percent must be between 0 and 100")
    return price * (1 - percent / 100)

def stock_status(quantity):
    if quantity is None:
        return "Unknown stock"
    if quantity == 0:
        return "Out of stock"
    if quantity < 10:
        return "Low stock"
    return "In stock"

def products_in_category(inventory_list, category):
    names = []
    for item in inventory_list:
        if item["category"] == category:
            names.append(item["product"])
    return names
```

 4. NumPy
```python
print("Shape:", daily_sales.shape)
print("Total:", np.sum(daily_sales))
print("Mean:", round(float(np.mean(daily_sales)), 2))
print("Highest:", int(np.max(daily_sales)))

weekly_sales = daily_sales.reshape(2, 7)
print("Weekly sales:\n", weekly_sales)
print("Median:", float(np.median(daily_sales)))
```
Observation: Median is 252.5 vs. Mean 310.71 because the outlier (980) pulls the mean up. Total sales equal 4350.

 5. pandas
```python
df = pd.DataFrame(inventory)
print(df.head())
print(df.tail(3))
df.info()
print(df.describe())
print("Shape:", df.shape)
print(df.isna().sum())
print("Duplicate rows:", df.duplicated().sum())

def inspect_data(dataframe):
    print(f"Rows: {dataframe.shape[0]}")
    print(f"Columns: {dataframe.shape[1]}")
    print(dataframe.isna().sum())
    print(f"Duplicates: {dataframe.duplicated().sum()}")

inspect_data(df)

netflix = pd.read_csv("netflix_titles_cleaned.csv")
inspect_data(netflix)
```

 Summary
- Inventory: 11 rows, 1 missing value (`Highlighter Set`), and 1 duplicate row (`Pen`).
- Netflix cleaned: 8,790 rows, 10 columns, 0 missing values, and 0 duplicates (contains hidden `"Not Given"` placeholders).

