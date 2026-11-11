# Data Structures, Functions & First Look at Data Caroline · SolaviseTech Women in Data, Cohort 3

```
import numpy as np
import pandas as pd

# A list of dictionaries: each dictionary is one product
inventory = [
    {"product": "Notebook", "category": "Stationery",
     "price": 1500, "quantity": 40},
    {"product": "Pen", "category": "Stationery",
     "price": 200, "quantity": 120},
    {"product": "Backpack", "category": "Bags",
     "price": 12000, "quantity": 15},
    {"product": "Water Bottle", "category": "Drinkware",
     "price": 2500, "quantity": 0},
    {"product": "Laptop Sleeve", "category": "Bags",
     "price": 6500, "quantity": 8},
    {"product": "Highlighter Set", "category": "Stationery",
     "price": 1800, "quantity": None},
    {"product": "Coffee Mug", "category": "Drinkware",
     "price": 3000, "quantity": 22},
    {"product": "USB Flash Drive", "category": "Electronics",
     "price": 4500, "quantity": 12},
    {"product": "Earphones", "category": "Electronics",
     "price": 7000, "quantity": 5},
    {"product": "Pen", "category": "Stationery",
     "price": 200, "quantity": 120},
    {"product": "Desk Lamp", "category": "Electronics",
     "price": 9500, "quantity": 6},
]

# A simple dictionary of prices (used in Part 1)
prices = {"Notebook": 1500, "Pen": 200,
          "Backpack": 12000, "Water Bottle": 2500}

# Daily sales (in thousands of naira) for 14 days, 2 weeks (used in Part 3)
daily_sales = np.array([220, 250, 240, 230, 260, 300, 310,
                        235, 245, 255, 240, 265, 980, 320])
```

## Part 1: Dictionaries

### Task 1.1

```
# Task 1.1: create a product dictionary and print the price by its key
product = {"name": "Notebook", "category": "Stationery", "price": 1500, "quantity": 20}
print(product["price"])
```

```
1500
```

### Task 1.2

```
# Task 1.2: add a supplier, change quantity, print the whole dictionary
product["supplier"] = "Ade Stationers"
product["quantity"] = 35
print(product)
```

```
{'name': 'Notebook', 'category': 'Stationery', 'price': 1500, 'quantity': 35, 'supplier': 'Ade Stationers'}
```

### Task 1.3

```
# Task 1.3: loop through prices and print each line
for item, price in prices.items():
    print(f"{item} costs N{price:,}")

# .get() returns a default value instead of raising a KeyError
print(prices.get("Calculator", "Not stocked"))
```

```
Notebook costs N1,500
Pen costs N200
Backpack costs N12,000
Water Bottle costs N2,500
Not stocked
```

### Task 1.4

```
# Task 1.4: dictionary comprehension, 10% discount on every product
sale_prices = {item: price * 0.9 for item, price in prices.items()}
print(sale_prices)
```

```
{'Notebook': 1350.0, 'Pen': 180.0, 'Backpack': 10800.0, 'Water Bottle': 2250.0}
```

### Task 1.5

```
# Task 1.5: count how many products are in each category
category_counts = {}
for item in inventory:
    cat = item["category"]
    category_counts[cat] = category_counts.get(cat, 0) + 1
print(category_counts)
```

```
{'Stationery': 4, 'Bags': 2, 'Drinkware': 2, 'Electronics': 3}
```

## Part 2: Functions

### Task 2.1

```
# Task 2.1
def total_value(price, quantity):
    """Return the total value of stock: price multiplied by quantity."""
    return price * quantity

print(total_value(1500, 40))
```

```
60000
```

### Task 2.2

```
# Task 2.2
def apply_discount(price, percent=10):
    """Return the price after taking off the given percentage discount."""
    return price - (price * percent / 100)

print(apply_discount(1500))       # default 10%
print(apply_discount(1500, 25))   # 25%
```

```
1350.0
1125.0
```

### Task 2.3

```
# Task 2.3
def stock_status(quantity):
    """Return 'Out of stock', 'Low stock' or 'In stock' for a quantity."""
    if quantity == 0:
        return "Out of stock"
    elif quantity < 10:
        return "Low stock"
    else:
        return "In stock"

for q in (0, 5, 40):
    print(q, "->", stock_status(q))
```

```
0 -> Out of stock
5 -> Low stock
40 -> In stock
```

### Task 2.4

```
# Task 2.4
def products_in_category(inventory, category):
    """Return a list of product names that belong to the given category."""
    names = []
    for item in inventory:
        if item["category"] == category:
            names.append(item["product"])
    return names

print(products_in_category(inventory, "Electronics"))
```

```
['USB Flash Drive', 'Earphones', 'Desk Lamp']
```

## Part 3: NumPy

### Task 3.1

```
# Task 3.1
print("Shape:", daily_sales.shape)
print("Data type:", daily_sales.dtype)
```

```
Shape: (14,)
Data type: int64
```

### Task 3.2

```
# Task 3.2
print("Total:", np.sum(daily_sales))
print("Mean:", round(np.mean(daily_sales), 2))
print("Highest:", np.max(daily_sales))
print("Standard deviation:", round(np.std(daily_sales), 2))
```

```
Total: 4350
Mean: 310.71
Highest: 980
Standard deviation: 187.92
```

### Task 3.3

```
# Task 3.3: 2 weeks x 7 days
weekly_sales = daily_sales.reshape(2, 7)
print(weekly_sales.shape)
```

```
(2, 7)
```

### Task 3.4

```
# Task 3.4
print("Median:", np.median(daily_sales))
print("Mean:  ", round(np.mean(daily_sales), 2))
```

```
Median: 252.5
Mean:   310.71
```

Answer: The median (252.5) describes a typical day better than the mean (about 310.71). The mean is pulled up by one unusually high day, 980 on day 13, which is far above every other day (the rest sit between 220 and 320). The median is the middle value, so one extreme day barely affects it. Most days really are around 250, not 310.

## Part 4: pandas, Inspecting the Data

### Task 4.1

```
# Task 4.1
df = pd.DataFrame(inventory)
df
```

```
            product     category  price  quantity
0          Notebook   Stationery   1500      40.0
1               Pen   Stationery    200     120.0
2          Backpack         Bags  12000      15.0
3      Water Bottle    Drinkware   2500       0.0
4     Laptop Sleeve         Bags   6500       8.0
5   Highlighter Set   Stationery   1800       NaN
6        Coffee Mug    Drinkware   3000      22.0
7   USB Flash Drive  Electronics   4500      12.0
8         Earphones  Electronics   7000       5.0
9               Pen   Stationery    200     120.0
10        Desk Lamp  Electronics   9500       6.0
```

### Task 4.2

```
# Task 4.2a
df.head()
```

```
         product    category  price  quantity
0       Notebook  Stationery   1500      40.0
1            Pen  Stationery    200     120.0
2       Backpack        Bags  12000      15.0
3   Water Bottle   Drinkware   2500       0.0
4  Laptop Sleeve        Bags   6500       8.0
```

```
# Task 4.2b
df.tail(3)
```

```
      product     category  price  quantity
8   Earphones  Electronics   7000       5.0
9         Pen   Stationery    200     120.0
10  Desk Lamp  Electronics   9500       6.0
```

```
# Task 4.2c
df.info()
```

```
<class 'pandas.DataFrame'>
RangeIndex: 11 entries, 0 to 10
Data columns (total 4 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   product   11 non-null     str    
 1   category  11 non-null     str    
 2   price     11 non-null     int64  
 3   quantity  10 non-null     float64
dtypes: float64(1), int64(1), str(2)
memory usage: 484.0 bytes
```

```
# Task 4.2d
df.describe()
```

```
              price   quantity
count     11.000000   10.00000
mean    4427.272727   34.80000
std     3885.380524   46.26926
min      200.000000    0.00000
25%     1650.000000    6.50000
50%     3000.000000   13.50000
75%     6750.000000   35.50000
max    12000.000000  120.00000
```

### Task 4.3

```
# Task 4.3
print("Shape (rows, columns):", df.shape)
print()
print(df.isnull().sum())
```

```
Shape (rows, columns): (11, 4)

product     0
category    0
price       0
quantity    1
dtype: int64
```

### Task 4.4

```
# Task 4.4
print("Duplicate rows:", df.duplicated().sum())
```

```
Duplicate rows: 1
```

### Task 4.5

```
# Supporting numbers for the answers below
print("Average price:", round(df["price"].mean(), 2))
print("Highest price:", df["price"].max())
print(df.dtypes)
print(df[df["quantity"].isnull()][["product"]])
print(df[df.duplicated(keep=False)])
```

```
Average price: 4427.27
Highest price: 12000
product         str
category        str
price         int64
quantity    float64
dtype: object
           product
5  Highlighter Set
  product    category  price  quantity
1     Pen  Stationery    200     120.0
9     Pen  Stationery    200     120.0
```

Answers\
\
1\. The data has 11 rows and 4 columns (product, category, price, quantity).\
2\. The quantity column has one missing value. It belongs to the Highlighter Set.\
3\. Yes, there is 1 duplicate row: the Pen (Stationery, 200, 120) appears twice.\
4\. price is \`int64\` (whole numbers), category is text (\`object\`, shown as \`str\` in newer pandas versions), and quantity is \`float64\`.\
5\. Quantity is \`float64\` because of the missing value. In pandas, a missing value (NaN) is a float, and an integer column cannot hold NaN. So pandas converts the whole column to float so that the NaN can sit alongside the other numbers.\
6\. The average price is about 4,427.27 and the highest price is 12,000 (the Backpack).

## Bonus Challenge

```
# Bonus: inspect_data works on any DataFrame
def inspect_data(df):
    """Print rows, columns, missing values per column and duplicate row count."""
    print("Rows:", df.shape[0])
    print("Columns:", df.shape[1])
    print("\nMissing values per column:")
    print(df.isnull().sum())
    print("\nDuplicate rows:", df.duplicated().sum())

inspect_data(df)
```

```
Rows: 11
Columns: 4

Missing values per column:
product     0
category    0
price       0
quantity    1
dtype: int64

Duplicate rows: 1
```

```
# Netflix dataset (needs netflix1.csv in the same folder as this notebook)
netflix = pd.read_csv("netflix1.csv")
inspect_data(netflix)
```

```
Rows: 8790
Columns: 10

Missing values per column:
show_id         0
type            0
title           0
director        0
country         0
date_added      0
release_year    0
rating          0
duration        0
listed_in       0

Duplicate rows: 0
```

Two sentences on the Netflix data: The dataset has 8,790 rows and 10 columns, with 0 missing values and 0 duplicate rows, so it looks clean at first glance. However, director has 2,588 entries and country has 287 entries filled with the placeholder text "Not Given", which pandas does not count as missing, so those gaps are hidden and need checking before any analysis.