"""
Data Structures, Functions & First Look at Data
Author: Caroline · SolaviseTech Women in Data, Cohort 3
"""

import numpy as np
import pandas as pd

# ==========================================
# SETUP DATA
# ==========================================

# A list of dictionaries: each dictionary is one product
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

# A simple dictionary of prices (used in Part 1)
prices = {
    "Notebook": 1500,
    "Pen": 200,
    "Backpack": 12000,
    "Water Bottle": 2500,
}

# Daily sales (in thousands of naira) for 14 days, 2 weeks (used in Part 3)
daily_sales = np.array([
    220, 250, 240, 230, 260, 300, 310,
    235, 245, 255, 240, 265, 980, 320,
])

# ==========================================
# PART 1: Dictionaries
# ==========================================

# Task 1.1: Create a product dictionary and print the price by its key
product = {"name": "Notebook", "category": "Stationery", "price": 1500, "quantity": 20}
print("Task 1.1 Price:", product["price"])

# Task 1.2: Add a supplier, change quantity, print the whole dictionary
product["supplier"] = "Ade Stationers"
product["quantity"] = 35
print("Task 1.2 Updated Product:", product)

# Task 1.3: Loop through prices and print each line
print("\nTask 1.3 Prices Loop:")
for item, price in prices.items():
    print(f"{item} costs N{price:,}")

# .get() returns a default value instead of raising a KeyError
print("Lookup Calculator:", prices.get("Calculator", "Not stocked"))

# Task 1.4: Dictionary comprehension, 10% discount on every product
sale_prices = {item: price * 0.9 for item, price in prices.items()}
print("\nTask 1.4 Sale Prices:", sale_prices)

# Task 1.5: Count how many products are in each category
category_counts = {}
for item in inventory:
    cat = item["category"]
    category_counts[cat] = category_counts.get(cat, 0) + 1
print("\nTask 1.5 Category Counts:", category_counts)

# ==========================================
# PART 2: Functions
# ==========================================

# Task 2.1
def total_value(price, quantity):
    """Return the total value of stock: price multiplied by quantity."""
    return price * quantity


print("\nTask 2.1 Total Value:", total_value(1500, 40))

# Task 2.2
def apply_discount(price, percent=10):
    """Return the price after taking off the given percentage discount."""
    return price - (price * percent / 100)


print("Task 2.2 Default Discount (10%):", apply_discount(1500))
print("Task 2.2 Custom Discount (25%):", apply_discount(1500, 25))

# Task 2.3
def stock_status(quantity):
    """Return 'Out of stock', 'Low stock' or 'In stock' for a quantity."""
    if quantity == 0:
        return "Out of stock"
    elif quantity < 10:
        return "Low stock"
    else:
        return "In stock"


print("\nTask 2.3 Stock Status Checks:")
for q in (0, 5, 40):
    print(f"{q} -> {stock_status(q)}")

# Task 2.4
def products_in_category(inventory_list, category):
    """Return a list of product names that belong to the given category."""
    names = []
    for item in inventory_list:
        if item["category"] == category:
            names.append(item["product"])
    return names


print("\nTask 2.4 Electronics Products:", products_in_category(inventory, "Electronics"))

# ==========================================
# PART 3: NumPy
# ==========================================

print("\n--- PART 3: NumPy ---")

# Task 3.1
print("Shape:", daily_sales.shape)
print("Data type:", daily_sales.dtype)

# Task 3.2
print("Total:", np.sum(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
print("Highest:", np.max(daily_sales))
print("Standard deviation:", round(np.std(daily_sales), 2))

# Task 3.3: 2 weeks x 7 days
weekly_sales = daily_sales.reshape(2, 7)
print("Weekly Sales Shape:", weekly_sales.shape)

# Task 3.4
print("Median:", np.median(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))

# Note: The median (252.5) describes a typical day better than the mean (310.71)
# due to the outlier on day 13 (980).

# ==========================================
# PART 4: pandas, Inspecting the Data
# ==========================================

print("\n--- PART 4: Pandas ---")
df = pd.DataFrame(inventory)
print(df)

# Task 4.2
print("\nFirst 5 rows:")
print(df.head())
print("\nLast 3 rows:")
print(df.tail(3))
print("\nDataFrame Info:")
df.info()
print("\nDataFrame Describe:")
print(df.describe())

# Task 4.3 & 4.4
print("\nShape (rows, columns):", df.shape)
print("Missing values per column:\n", df.isnull().sum())
print("Duplicate rows:", df.duplicated().sum())

# ==========================================
# BONUS CHALLENGE
# ==========================================

def inspect_data(dataframe):
    """Print rows, columns, missing values per column and duplicate row count."""
    print(f"Rows: {dataframe.shape[0]}")
    print(f"Columns: {dataframe.shape[1]}")
    print("\nMissing values per column:")
    print(dataframe.isnull().sum())
    print(f"\nDuplicate rows: {dataframe.duplicated().sum()}")


print("\n--- Bonus: Inspecting Inventory DataFrame ---")
inspect_data(df)

# To run the Netflix dataset check locally (ensure 'netflix1.csv' is in the folder):
# netflix = pd.read_csv("netflix1.csv")
# inspect_data(netflix)
