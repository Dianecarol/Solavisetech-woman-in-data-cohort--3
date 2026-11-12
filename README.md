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

 2.1 Access a value

```python
product = {"product": "Notebook", "category": "Stationery", "price": 1500, "quantity": 20}
print("Price:", product["price"])
```

 2.2 Add and update values

```python
product["supplier"] = "Ade Stationers"
product["quantity"] = 35
print("Updated product:", product)
```

 2.3 Loop through a dictionary

```python
print("Prices:")
for item, price in prices.items():
    print(f"{item} costs ₦{price:,}")

print("Calculator lookup:", prices.get("Calculator", "Not stocked"))
```

 2.4 Use a dictionary comprehension

```python
sale_prices = {item: price * 0.9 for item, price in prices.items()}
print("Prices after a 10% discount:", sale_prices)
```

 2.5 Count products by category

```python
category_counts = {}
for item in inventory:
    category = item["category"]
    category_counts[category] = category_counts.get(category, 0) + 1

print("Products per category:", category_counts)
```

Note: Stationery shows 4 because the duplicate "Pen" row is counted twice. This is expected, not a bug. Duplicates are handled in section 5.3.



 3. Functions

 3.1 Calculate stock value

```python
def total_value(price, quantity):
    """Return stock value as price multiplied by quantity."""
    if quantity is None:
        return 0
    return price * quantity

print("Notebook stock value:", total_value(1500, 40))
```

### 3.2 Apply a discount

```python
def apply_discount(price, percent=10):
    """Return the price after applying a percentage discount."""
    if price < 0:
        raise ValueError("price must not be negative")
    if not 0 <= percent <= 100:
        raise ValueError("percent must be between 0 and 100")
    return price * (1 - percent / 100)

print("10% discount:", apply_discount(1500))
print("25% discount:", apply_discount(1500, 25))
```

 3.3 Classify stock levels

```python
def stock_status(quantity):
    """Describe a product's stock level, including an unknown quantity."""
    if quantity is None:
        return "Unknown stock"
    if quantity == 0:
        return "Out of stock"
    if quantity < 10:
        return "Low stock"
    return "In stock"

for quantity in (0, 5, 40, None):
    print(f"{quantity} -> {stock_status(quantity)}")
```

Note: When inventory is loaded into a pandas DataFrame, `None` in the quantity column becomes `NaN` (and the column becomes float). The functions above check only for `None`, so they would not catch a `NaN` if applied to the DataFrame. That is fine for this lesson.

 3.4 Find products in a category

```python
def products_in_category(inventory_list, category):
    """Return product names that belong to the requested category."""
    names = []
    for item in inventory_list:
        if item["category"] == category:
            names.append(item["product"])
    return names

print("Electronics:", products_in_category(inventory, "Electronics"))
```


 4. NumPy: Working with Numerical Arrays

 4.1 Inspect the array

```python
print("Shape:", daily_sales.shape)
print("Data type:", daily_sales.dtype)
```

 4.2 Calculate summary statistics

```python
print("Total:", np.sum(daily_sales))
print("Mean:", round(float(np.mean(daily_sales)), 2))
print("Highest:", int(np.max(daily_sales)))
print("Standard deviation:", round(float(np.std(daily_sales)), 2))
```

 4.3 Reshape into two weeks

```python
weekly_sales = daily_sales.reshape(2, 7)
print("Weekly sales:\n", weekly_sales)
print("Weekly sales shape:", weekly_sales.shape)
```

 4.4 Compare the mean and median

```python
print("Median:", float(np.median(daily_sales)))
print("Mean:", round(float(np.mean(daily_sales)), 2))
```

The median is 252.5 and the mean is 310.71. The outlier (980 on day 13) pulls the mean up. The total is 4,350.


5. pandas: Inspect Tabular Data

 5.1 Create and display a DataFrame

```python
df = pd.DataFrame(inventory)
print(df)
```

 5.2 Preview and summarize

```python
print("First five rows:")
print(df.head())

print("Last three rows:")
print(df.tail(3))

df.info()

print("Numeric summary:")
print(df.describe())
```

 5.3 Check shape, missing values, and duplicates

```python
print("Shape (rows, columns):", df.shape)
print("Missing values per column:\n", df.isna().sum())
print("Duplicate rows:", df.duplicated().sum())
```

---

 Bonus: Reusable Inspection Function

```python
def inspect_data(dataframe):
    """Print dimensions, missing values, and duplicate-row count."""
    print(f"Rows: {dataframe.shape[0]}")
    print(f"Columns: {dataframe.shape[1]}")
    print("\nMissing values per column:")
    print(dataframe.isna().sum())
    print(f"\nDuplicate rows: {dataframe.duplicated().sum()}")

print("Inventory data check:")
inspect_data(df)
```

---

 Bonus: Inspect a Real Dataset

Note: `netflix_titles_cleaned.csv` must be in the same folder as this notebook/script (the repo root) for this cell to run.

```python
netflix = pd.read_csv("netflix_titles_cleaned.csv")
inspect_data(netflix)

print("\nFirst 5 rows:")
print(netflix.head())

print("\n'Not Given' entries:")
print("director:", netflix["director"].eq("Not Given").sum())
print("country:", netflix["country"].eq("Not Given").sum())
```

---

 Summary

- Inventory: 11 rows, 1 missing value (Highlighter Set), 1 duplicate row (Pen)
- Netflix cleaned: 8,790 rows, 10 columns, 0 missing, 0 duplicates
- Hidden gaps: "Not Given" placeholders in director and country

---

 Key Takeaways

- Dictionaries organize related data as key-value pairs
- Functions let you reuse logic
- NumPy is useful for numerical arrays
- pandas is useful for rows and columns
- Always check for missing values and duplicates


 Final Reflection

This lesson helped me build a strong foundation in core Python and data tools. I learned how to work with dictionaries, write reusable functions, use NumPy for calculations, and use pandas to inspect and clean tabular data. These are essential skills for data analysis, machine learning, and AI projects.

I am excited to continue this learning journey in the SolaviseTech Women in Data program and to keep growing in Data Science, Data Analysis, and Artificial Intelligence.
