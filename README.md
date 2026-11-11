
Data Structures, Functions & First Look at Data

Author: Caroline Diana Njama

Program: SolaviseTech Women in Data, Cohort 3

Welcome! This beginner-friendly lesson introduces Python dictionaries and functions, then uses NumPy and pandas to explore simple inventory and sales data. Work through each section in order, running the code cells as you go.

 Learning objectives

By the end of this lesson, you should be able to:

- Create, access, update, and loop through dictionaries.
- Write functions that accept inputs and return results.
- Use NumPy arrays to calculate basic statistics and reshape data.
- Create and inspect a pandas DataFrame.
- Identify missing values and duplicate rows.

Before you begin

You need Python 3 and the NumPy and pandas libraries. Install them from a terminal if they are not already available:

```bash
python -m pip install numpy pandas
```

You can run the examples in a Jupyter notebook, Google Colab, or a Python file. In a notebook, put each numbered code example in its own cell. Run the setup cell below first.

1. Set up the example data

An inventory is a list of dictionaries. Each dictionary represents one product, and its keys (`product`, `category`, `price`, and `quantity`) describe that product. `None` represents an unknown quantity; it is not the same as zero stock.

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

# A small product-to-price dictionary for the dictionary exercises.
prices = {
    "Notebook": 1500,
    "Pen": 200,
    "Backpack": 12000,
    "Water Bottle": 2500,
}

# Daily sales in thousands of naira, for 14 days (two weeks).
daily_sales = np.array([
    220, 250, 240, 230, 260, 300, 310,
    235, 245, 255, 240, 265, 980, 320,
])
```

 2. Dictionaries

A dictionary stores information as key–value pairs. For example, `product["price"]` retrieves the value associated with the `"price"` key.

 2.1 Access a value

```python
product = {
    "product": "Notebook",
    "category": "Stationery",
    "price": 1500,
    "quantity": 20,
}

print("Price:", product["price"])
```

Square brackets work when you know the key exists. If a key might be missing, use `.get()` to provide a fallback value instead of getting a `KeyError`.

2.2 Add and update values

Assigning a value to a new key adds it. Assigning to an existing key updates it.

```python
product["supplier"] = "Ade Stationers"
product["quantity"] = 35
print("Updated product:", product)
```

2.3 Loop through a dictionary

The `.items()` method gives each key and value, which makes it useful for looping through a dictionary.

```python
print("Prices:")
for item, price in prices.items():
    print(f"{item} costs ₦{price:,}")

print("Calculator lookup:", prices.get("Calculator", "Not stocked"))
```

2.4 Use a dictionary comprehension

A comprehension creates a new collection from an existing one. Here, each price is multiplied by 0.9 to apply a 10% discount.

```python
sale_prices = {
    item: price * 0.9
    for item, price in prices.items()
}
print("Prices after a 10% discount:", sale_prices)
```

2.5 Count products by category

We start with an empty dictionary. For each inventory item, `.get(category, 0)` gives the current count (or 0 if this is the first product in that category), then we add one.

```python
category_counts = {}
for item in inventory:
    category = item["category"]
    category_counts[category] = category_counts.get(category, 0) + 1

print("Products per category:", category_counts)
```

Try it: Add another product to `inventory` and run the cell again. How do the category counts change?

3. Functions

A function is a reusable block of code. It can accept inputs (called parameters) and return a result.

 3.1 Calculate stock value

Stock value is the unit price multiplied by the number of units. This example uses positional arguments: `1500` is the price and `40` is the quantity.

```python
def total_value(price, quantity):
    """Return stock value as price multiplied by quantity."""
    return price * quantity

print("Notebook stock value:", total_value(1500, 40))
```

 3.2 Apply a discount

The `percent` parameter has a default value of 10. You can use that default or pass a different percentage. The function checks that the price is not negative and the discount is between 0 and 100, so accidental invalid inputs are caught early.

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

`if`, `elif`, and `else` let a function choose an answer based on its input. A missing quantity (`None`) is reported as unknown rather than incorrectly classified as in stock.

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

 3.4 Find products in a category

This function checks each dictionary in the inventory and collects the names whose category matches the one requested.

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

Try it: Call the function with `"Bags"` or `"Stationery"`.

4. NumPy: working with numerical arrays

NumPy arrays make it convenient to perform calculations across many numbers. The `daily_sales` values are in thousands of naira, so a value of 220 represents ₦220,000.

4.1 Inspect the array

`shape` describes the dimensions of the array, and `dtype` reports the type of values it stores.

```python
print("Shape:", daily_sales.shape)
print("Data type:", daily_sales.dtype)
```

Note: The data type usually prints as `int64`. On some older Windows setups it may show `int32`. Both are whole-number types.

 4.2 Calculate summary statistics

```python
print("Total:", np.sum(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
print("Highest:", np.max(daily_sales))
print("Standard deviation:", round(np.std(daily_sales), 2))
```

The standard deviation describes how spread out the values are around their mean. NumPy's `np.std()` uses the population standard deviation by default.

 4.3 Reshape into two weeks

There are 14 values, so they can be reshaped into two rows of seven days. `reshape` changes the layout, not the underlying values.

```python
weekly_sales = daily_sales.reshape(2, 7)
print("Weekly sales:\n", weekly_sales)
print("Weekly sales shape:", weekly_sales.shape)
```

 4.4 Compare the mean and median

The mean is affected by unusually large or small values. The median is the middle value after sorting and is often more representative when there is an outlier.

```python
print("Median:", np.median(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
```

The median is 252.5, while the mean is about 310.71. The high value of 980 on day 13 pulls the mean upward. (The total is 4,350 across 14 days.)

 5. pandas: inspect tabular data

pandas stores table-shaped data in a DataFrame. Each inventory dictionary becomes a row, and each dictionary key becomes a column.

5.1 Create and display a DataFrame

```python
df = pd.DataFrame(inventory)
print(df)
```

 5.2 Preview and summarize the data

- `head()` displays the first rows (five by default).
- `tail(3)` displays the last three rows.
- `info()` shows column types and non-missing values.
- `describe()` summarizes numeric columns.

```python
print("First five rows:")
print(df.head())

print("Last three rows:")
print(df.tail(3))

print("DataFrame information:")
df.info()

print("Numeric summary:")
print(df.describe())
```

Note: The `quantity` column is shown as decimals (for example `40.0`) because pandas stores a missing value as `NaN`, which forces the column to a float type. `describe()` ignores the missing value when it calculates statistics.

 5.3 Check the table's shape, missing values, and duplicates

`shape` returns (rows, columns). `isna().sum()` counts missing values in each column. `duplicated().sum()` counts rows that repeat an earlier row; in this data, the repeated Pen entry is a duplicate.

```python
print("Shape (rows, columns):", df.shape)
print("Missing values per column:\n", df.isna().sum())
print("Duplicate rows:", df.duplicated().sum())
```

 Bonus: create a reusable inspection function

Putting common checks in a function makes them easy to reuse for another DataFrame.

```python
def inspect_data(dataframe):
    """Print the dimensions, missing values, and duplicate-row count."""
    print(f"Rows: {dataframe.shape[0]}")
    print(f"Columns: {dataframe.shape[1]}")
    print("\nMissing values per column:")
    print(dataframe.isna().sum())
    print(f"\nDuplicate rows: {dataframe.duplicated().sum()}")

print("Inventory data check:")
inspect_data(df)
```

Bonus: inspect a real dataset



```python
netflix = pd.read_csv("netflix_titles_cleaned.csv")
inspect_data(netflix)
```

You should see 8,786 rows and 11 columns, with 2,587 missing directors, 688 missing countries, and 0 duplicate rows.

Key takeaways

- Dictionaries organize related data as key–value pairs.
- Functions let you reuse logic and make programs easier to understand.
- NumPy is useful for numerical arrays and calculations.
- pandas is useful for working with rows and columns of data.
- Always check for missing values and duplicates before drawing conclusions.

 Practice challenges

1. Add a new product to `inventory` and recalculate the category counts.
2. Write a function that returns the total value for an inventory item. Decide how it should handle an unknown quantity.
3. Find the product with the highest price in the inventory.
4. Calculate the average price for each category. (Stretch task. Hint: look up `df.groupby("category")["price"].mean()`.)
5. Remove duplicate rows from the DataFrame and compare its shape before and after:

```python
df_without_duplicates = df.drop_duplicates()
print("Before:", df.shape)
print("After:", df_without_duplicates.shape)
```