Data Structures, Functions & First Look at Data

Author: Caroline Diana Njama
  
Program: SolaviseTech Women in Data, Cohort 3

My learning journey and goals in the SolaviseTech Women in Data Cohort 3 program. SolaviseTech is a program that helps women master Data Science & ML, Data Analysis, and Artificial Intelligence. At the end of this program, I wish to be an expert in these three domains and have multiple opportunities.

Welcome
This beginner-friendly lesson introduces Python dictionaries and functions, then uses NumPy and pandas to explore simple inventory and sales data. Work through each section in order, running the code cells as you go.

Learning Objectives
- Create, access, update, and loop through dictionaries
- Write functions that accept inputs and return results
- Use NumPy arrays to calculate basic statistics and reshape data
- Create and inspect a pandas DataFrame
- Identify missing values and duplicate rows

Setup
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

2.1 Access a value
```python
product = {"product": "Notebook", "category": "Stationery", "price": 1500, "quantity": 20}
print("Price:", product["price"])
```

2.2 Add and update
```python
product["supplier"] = "Ade Stationers"
product["quantity"] = 35
print("Updated product:", product)
```

2.3 Loop
```python
print("Prices:")
for item, price in prices.items():
    print(f"{item} costs N{price:,}")
print("Calculator lookup:", prices.get("Calculator", "Not stocked"))
```

2.4 Comprehension

```python
sale_prices = {item: price * 0.9 for item, price in prices.items()}
print("Prices after a 10% discount:", sale_prices)
```

2.5 Count by category
```python
category_counts = {}
for item in inventory:
    category = item["category"]
    category_counts[category] = category_counts.get(category, 0) + 1
print("Products per category:", category_counts)
```

 3. Functions

3.1 Stock value
```python
def total_value(price, quantity):
    return price * quantity
print("Notebook stock value:", total_value(1500, 40))
```

3.2 Discount
```python
def apply_discount(price, percent=10):
    if price < 0: raise ValueError("price must not be negative")
    if not 0 <= percent <= 100: raise ValueError("percent must be between 0 and 100")
    return price * (1 - percent / 100)
print("10% discount:", apply_discount(1500))
print("25% discount:", apply_discount(1500, 25))
```

3.3 Stock status
```python
def stock_status(quantity):
    if quantity is None: return "Unknown stock"
    if quantity == 0: return "Out of stock"
    if quantity < 10: return "Low stock"
    return "In stock"
for quantity in (0, 5, 40, None):
    print(f"{quantity} -> {stock_status(quantity)}")
```

3.4 Products in category
```python
def products_in_category(inventory_list, category):
    names = []
    for item in inventory_list:
        if item["category"] == category:
            names.append(item["product"])
    return names
print("Electronics:", products_in_category(inventory, "Electronics"))
```

4. NumPy
```python
print("Shape:", daily_sales.shape)
print("Total:", np.sum(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
print("Highest:", np.max(daily_sales))
print("Standard deviation:", round(np.std(daily_sales), 2))
weekly_sales = daily_sales.reshape(2, 7)
print("Weekly sales:\n", weekly_sales)
print("Weekly sales shape:", weekly_sales.shape)
print("Median:", np.median(daily_sales))
# Median 252.5 vs Mean 310.71 - outlier 980 pulls mean up
```

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
```

Bonus
```python
def inspect_data(dataframe):
    print(f"Rows: {dataframe.shape[0]}")
    print(f"Columns: {dataframe.shape[1]}")
    print("\nMissing values per column:")
    print(dataframe.isna().sum())
    print(f"\nDuplicate rows: {dataframe.duplicated().sum()}")
inspect_data(df)

netflix = pd.read_csv("netflix_titles_cleaned.csv")
inspect_data(netflix)
print(netflix.head())
print("\n'Not Given' entries:")
print("director:", netflix["director"].eq("Not Given").sum())
print("country:", netflix["country"].eq("Not Given").sum())
```

Summary: Inventory 11 rows, 1 missing, 1 duplicate. Netflix cleaned 8790 rows, 0 missing, 0 duplicates. Hidden "Not Given" placeholders.
```



