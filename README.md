
 Data Structures, Functions & First Look at Data
Caroline  SolaviseTech Women in Data, Cohort 3
This assignment covers Python dictionaries and functions, basic NumPy operations, and introductory data inspection with pandas.

 Setup and sample data
```python
import numpy as np
import pandas as pd
 Each dictionary represents one inventory record.
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
# Product prices for Part 1.
prices = {
    "Notebook": 1500,
    "Pen": 200,
    "Backpack": 12000,
    "Water Bottle": 2500,
}
# Daily sales in thousands of naira for 14 days.
daily_sales = np.array([
    220, 250, 240, 230, 260, 300, 310,
    235, 245, 255, 240, 265, 980, 320,
])
```
 Part 1: Dictionaries
### Task 1.1 — Create and access a product dictionary
```python
product = {
    "name": "Notebook",
    "category": "Stationery",
    "price": 1500,
    "quantity": 20,
}
print(product["price"])
```

**Output:** `1500`
### Task 1.2 — Add and update dictionary values
```python
product["supplier"] = "Ade Stationers"
product["quantity"] = 35

print(product)
```
### Task 1.3 — Loop through prices
```python
for item, price in prices.items():
    print(f"{item} costs N{price:,}")
# .get() returns a default value if the key is not found.
print(prices.get("Calculator", "Not stocked"))
```
Task 1.4 — Apply a 10% discount with a dictionary comprehension
```python
sale_prices = {
    item: price * 0.9
    for item, price in prices.items()
}
print(sale_prices)
```
Task 1.5 — Count inventory records by category
```python
category_counts = {}
for item in inventory:
    category = item["category"]
    category_counts[category] = category_counts.get(category, 0) + 1

print(category_counts)
```
Result: `{'Stationery': 4, 'Bags': 2, 'Drinkware': 2, 'Electronics': 3}`
 Part 2: Functions
 Task 2.1 — Calculate stock value
```python
def total_value(price, quantity):
    """Return the total stock value: price multiplied by quantity."""
    return price  quantity
print(total_value(1500, 40))
```
Output: `60000`
 Task 2.2 — Apply a discount
```python
def apply_discount(price, percent=10):
    """Return the price after applying a percentage discount."""
    return price - (price * percent / 100)
print(apply_discount(1500))      # Uses the default 10% discount.
print(apply_discount(1500, 25))  # Applies a 25% discount.
```
Output: `1350.0` and `1125.0`
 Task 2.3 — Check stock status
```python
def stock_status(quantity):
    """Return the stock status, including a message for missing quantities."""
    if pd.isna(quantity):
        return "Unknown (missing quantity)"
    elif quantity == 0:
        return "Out of stock"
    elif quantity < 10:
        return "Low stock"
    else:
        return "In stock"

for quantity in (0, 5, 40, None):
    print(quantity, "->", stock_status(quantity))
```
 Task 2.4 — Find products in a category
```python
def products_in_category(inventory, category):
    """Return product names belonging to the specified category."""
    return [
        item["product"]
        for item in inventory
        if item["category"] == category
    ]
print(products_in_category(inventory, "Electronics"))
```
Output: `['USB Flash Drive', 'Earphones', 'Desk Lamp']`
 Part 3: NumPy
 Task 3.1 — Inspect the sales array
```python
print("Shape:", daily_sales.shape)
print("Data type:", daily_sales.dtype)
```
**Result:** The array has shape `(14,)` and contains integers.
Task 3.2 — Calculate summary statistics
```python
print("Total:", np.sum(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
print("Highest:", np.max(daily_sales))
print("Standard deviation:", round(np.std(daily_sales), 2))
```
Results: Total: `4350`; mean: `310.71`; highest: `980`; standard deviation: `187.92`.
 Task 3.3 — Reshape sales into two weeks
```python
weekly_sales = daily_sales.reshape(2, 7)
print(weekly_sales)
print("Shape:", weekly_sales.shape)
```
The reshaped array has two rows, one for each week, and seven columns, one for each day.
Task 3.4 — Compare the median and mean
```python
print("Median:", np.median(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
```
 The median, 252.5, better represents a typical day than the mean, approximately 310.71. The mean is pulled upward by the unusually high sales value of 980 on day 13. The other daily values range from 220 to 320, so the median is less affected by this outlier.
Part 4: pandas — Inspecting the data
Task 4.1 — Create a DataFrame
```python
df = pd.DataFrame(inventory)
df
```
Task 4.2 — Preview and summarize the DataFrame
```python
# Display the first five rows.
df.head()
# Display the last three rows.
df.tail(3)
# Show column names, non-null values, and data types.
df.info()
# Show summary statistics for numeric columns.
df.describe()
```
 Task 4.3 — Check the shape and missing values
```python
print("Shape (rows, columns):", df.shape)
print("\nMissing values per column:")
print(df.isnull().sum())
```
 The DataFrame has 11 rows and 4 columns. The `quantity` column contains one missing value.
Task 4.4 — Check for duplicate rows
```python
print("Duplicate rows:", df.duplicated().sum())
```
 There is one duplicate row: the Pen record appears twice.
 Task 4.5 — Answer questions about the data
```python
print("Average price:", round(df["price"].mean(), 2))
print("Highest price:", df["price"].max())
print("\nData types:")
print(df.dtypes)
print("\nProduct with a missing quantity:")
print(df.loc[df["quantity"].isna(), ["product"]])
print("\nDuplicate records:")
print(df[df.duplicated(keep=False)])
```
1. The data has 11 rows and 4 columns: `product`, `category`, `price`, and `quantity`.
2. The `quantity` column has one missing value, belonging to the Highlighter Set.
3. There is one duplicate row: the Pen record appears twice.
4. `price` is an integer column, `quantity` is a floating-point column, and `product` and `category` are text columns.
5. The `quantity` column is represented as floating point because it contains a missing value. A standard NumPy-backed integer column cannot store `NaN`, so pandas commonly uses a floating-point dtype instead.
6. The average price is approximately 4,427.27, and the highest price is 12,000, for the Backpack.
Bonus challenge — Reusable data inspection function
```python
def inspect_data(dataframe):
    """Print dimensions, missing-value counts, and duplicate-row count."""
    print("Rows:", dataframe.shape[0])
    print("Columns:", dataframe.shape[1])
    print("\nMissing values per column:")
    print(dataframe.isnull().sum())
    print("\nDuplicate rows:", dataframe.duplicated().sum())
inspect_data(df)
```





```python
netflix = pd.read_csv("netflix1.csv")
inspect_data(netflix)

# Check placeholder values that pandas does not treat as missing.
print("\n'Not Given' entries:")
print("'director':", netflix["director"].eq("Not Given").sum())
print("'country':", netflix["country"].eq("Not Given").sum())


