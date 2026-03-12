# Pandas Complete Reference Guide

This document covers all major pandas functions with syntax and explanations.

---

## Table of Contents

1. [DataFrame and Series Creation](#1-dataframe-and-series-creation)
2. [Data I/O](#2-data-io)
3. [Data Selection and Indexing](#3-data-selection-and-indexing)
4. [Data Manipulation](#4-data-manipulation)
5. [Data Transformation](#5-data-transformation)
6. [Merge, Join, and Concatenate](#6-merge-join-and-concatenate)
7. [GroupBy and Aggregation](#7-groupby-and-aggregation)
8. [String Operations](#8-string-operations)
9. [DateTime Operations](#9-datetime-operations)
10. [Statistical Functions](#10-statistical-functions)
11. [Window Functions](#11-window-functions)
12. [Plotting](#12-plotting)
13. [Utilities and Info](#13-utilities-and-info)

---

## 1. DataFrame and Series Creation

### pd.Series

**Syntax:**
```python
pd.Series(data=None, index=None, dtype=None, name=None, copy=False)
```

**Explanation:** Creates a one-dimensional array with optional indexing. Can be created from lists, numpy arrays, dictionaries, or scalars.

**Examples:**
```python
import pandas as pd
import numpy as np

# Create Series from list
s = pd.Series([1, 2, 3, 4, 5])
print(s)

# Create Series with custom index
s = pd.Series([10, 20, 30], index=['a', 'b', 'c'])
print(s)

# Create Series from dictionary
s = pd.Series({'a': 1, 'b': 2, 'c': 3})
print(s)

# Create Series with name
s = pd.Series([1, 2, 3], name='numbers')
print(s)
```

---

### pd.DataFrame

**Syntax:**
```python
pd.DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)
```

**Explanation:** Creates a 2-dimensional labeled data structure with columns of potentially different types.

**Examples:**
```python
# Create DataFrame from dictionary
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35],
    'Salary': [50000, 60000, 70000]
})
print(df)

# Create DataFrame from list of dictionaries
df = pd.DataFrame([
    {'Name': 'Alice', 'Age': 25},
    {'Name': 'Bob', 'Age': 30}
])
print(df)

# Create DataFrame from numpy array
df = pd.DataFrame(np.array([[1, 2], [3, 4], [5, 6]]), 
                  columns=['A', 'B'], 
                  index=['x', 'y', 'z'])
print(df)

# Create empty DataFrame
df = pd.DataFrame(columns=['A', 'B', 'C'])
print(df)
```

---

### pd.concat

**Syntax:**
```python
pd.concat(objs, axis=0, join='outer', ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=False, copy=True)
```

**Explanation:** Concatenates pandas objects along a particular axis with optional set logic along the other axes.

**Examples:**
```python
# Concatenate along axis 0 (rows)
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})
result = pd.concat([df1, df2])
print(result)

# Concatenate along axis 1 (columns)
df3 = pd.DataFrame({'C': [9, 10], 'D': [11, 12]})
result = pd.concat([df1, df3], axis=1)
print(result)

# Ignore index
result = pd.concat([df1, df2], ignore_index=True)
print(result)
```

---

### pd.merge

**Syntax:**
```python
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)
```

**Explanation:** Performs database-style join operations on DataFrame objects using columns as keys.

**Examples:**
```python
left = pd.DataFrame({'key': ['K0', 'K1', 'K2'], 'A': ['A0', 'A1', 'A2']})
right = pd.DataFrame({'key': ['K0', 'K1', 'K2'], 'B': ['B0', 'B1', 'B2']})

# Inner merge
result = pd.merge(left, right, on='key', how='inner')
print(result)

# Outer merge
result = pd.merge(left, right, on='key', how='outer')
print(result)

# Left merge
result = pd.merge(left, right, on='key', how='left')
print(result)

# Right merge
result = pd.merge(left, right, on='key', how='right')
print(result)
```

---

### pd.DataFrame.from_dict

**Syntax:**
```python
pd.DataFrame.from_dict(data, orient='columns', dtype=None, columns=None)
```

**Explanation:** Creates a DataFrame from a dictionary with 'dict' (column-oriented) or 'series' (index-oriented) orientation.

**Examples:**
```python
# From dict with 'columns' orientation (default)
data = {'A': [1, 2], 'B': [3, 4], 'C': [5, 6]}
df = pd.DataFrame.from_dict(data)
print(df)

# From dict with 'index' orientation
data = {'row1': [1, 2, 3], 'row2': [4, 5, 6]}
df = pd.DataFrame.from_dict(data, orient='index', columns=['A', 'B', 'C'])
print(df)
```

---

### pd.DataFrame.from_records

**Syntax:**
```python
pd.DataFrame.from_records(data, index=None, exclude=None, columns=None, dtype=None, copy=False)
```

**Explanation:** Creates a DataFrame from a list of tuples, arrays, or records.

**Examples:**
```python
# From list of tuples/records
data = [('Alice', 25, 50000), ('Bob', 30, 60000), ('Charlie', 35, 70000)]
df = pd.DataFrame.from_records(data, columns=['Name', 'Age', 'Salary'])
print(df)
```

---

## 2. Data I/O

### pd.read_csv

**Syntax:**
```python
pd.read_csv(filepath_or_buffer, sep=',', header='infer', names=None, index_col=None, usecols=None, dtype=None, skiprows=None, nrows=None, na_values=None, keep_default_na=True, thousands=None, decimal='.', parse_dates=False, date_parser=None, skipfooter=0, encoding=None, dialect=None)
```

**Explanation:** Reads a comma-separated values (csv) file into DataFrame with various parameters for handling delimiters, headers, columns, and data types.

**Examples:**
```python
import io

csv_data = """Name,Age,Salary
Alice,25,50000
Bob,30,60000
Charlie,35,70000"""

# Read from string
df = pd.read_csv(io.StringIO(csv_data))
print(df)

# Read with custom delimiter
csv_data2 = "Name;Age;Salary\nAlice;25;50000\nBob;30;60000"
df = pd.read_csv(io.StringIO(csv_data2), sep=';')
print(df)

# Read with specific columns
df = pd.read_csv(io.StringIO(csv_data), usecols=['Name', 'Age'])
print(df)

# Read with headerless data
df = pd.read_csv(io.StringIO(csv_data), header=None, names=['A', 'B', 'C'])
print(df)
```

---

### pd.to_csv

**Syntax:**
```python
DataFrame.to_csv(path_or_buf=None, sep=',', index=True, columns=None, header=True, mode='w', encoding=None, date_format=None, double_precision=10, line_terminator=None, quotechar='"', quoting=0, escapechar=None)
```

**Explanation:** Converts the DataFrame to CSV format with options for separators, indexing, headers, and writing mode.

**Examples:**
```python
df = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})

# Convert to CSV string
csv_string = df.to_csv(index=False)
print(csv_string)

# Without index
csv_string = df.to_csv(index=False, header=True)
print(csv_string)

# Custom separator
csv_string = df.to_csv(sep=';', index=False)
print(csv_string)
```

---

### pd.read_excel

**Syntax:**
```python
pd.read_excel(io, sheet_name=0, header=0, names=None, index_col=None, usecols=None, dtype=None, engine=None, skiprows=None, nrows=None, na_values=None)
```

**Explanation:** Reads an Excel file into DataFrame supporting both .xlsx and .xls formats.

**Examples:**
```python
# Note: Requires openpyxl or xlrd installed
# Read from file
df = pd.read_excel('file.xlsx', sheet_name='Sheet1')
print(df)

# Read specific sheet
df = pd.read_excel('file.xlsx', sheet_name=0)  # First sheet

# Read all sheets
# all_sheets = pd.read_excel('file.xlsx', sheet_name=None)
```

---

### pd.to_excel

**Syntax:**
```python
DataFrame.to_excel(excel_writer, sheet_name='Sheet1', index=True, columns=None, header=True, mode='w', encoding=None)
```

**Explanation:** Writes the DataFrame to an Excel spreadsheet.

**Examples:**
```python
df = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})

# Note: Requires openpyxl installed
# df.to_excel('output.xlsx', sheet_name='Sheet1', index=False)
```

---

### pd.read_json

**Syntax:**
```python
pd.read_json(path_or_buf=None, orient=None, typ='frame', dtype=None, convert_axes=None, convert_dates=True, keep_default_dates=True, numpy=False, precise_float=False, date_unit=None, encoding=None, encoding_errors='strict', lines=False, compression='infer', nrows=None)
```

**Explanation:** Converts a JSON string or file to a DataFrame.

**Examples:**
```python
# Read JSON
json_data = '{"Name": ["Alice", "Bob"], "Age": [25, 30]}'
df = pd.read_json(json_data)
print(df)

# Read JSON from file
# df = pd.read_json('file.json', orient='records')
```

---

### pd.to_json

**Syntax:**
```python
DataFrame.to_json(path_or_buf=None, orient=None, date_format=None, double_precision=10, force_ascii=True, date_unit='ms', default_handler=None, lines=False, compression='infer', index=True)
```

**Explanation:** Converts the DataFrame to JSON format.

**Examples:**
```python
df = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})

# Convert to JSON
json_string = df.to_json(orient='records')
print(json_string)

# To JSON file
# df.to_json('output.json', orient='records')
```

---

### pd.read_html

**Syntax:**
```python
pd.read_html(io, match='.+', flavor=None, header=None, index_col=None, skiprows=None, attrs=None, parse_dates=False, thousands=',', decimal='.', converters=None, na_values=None, keep_default_na=True)
```

**Explanation:** Reads HTML tables from a URL, file, or string into DataFrame.

**Examples:**
```python
# Read tables from URL
# dfs = pd.read_html('https://example.com/table.html')

# Read from string
# html_string = '<table><tr><td>1</td></tr></table>'
# dfs = pd.read_html(html_string)
```

---

### pd.read_sql

**Syntax:**
```python
pd.read_sql(sql, con, index_col=None, coerce_float=True, params=None, parse_dates=None, columns=None, chunksize=None)
```

**Explanation:** Reads a SQL query or database table into DataFrame.

**Examples:**
```python
# Using SQLAlchemy
# from sqlalchemy import create_engine
# engine = create_engine('sqlite:///database.db')
# df = pd.read_sql('SELECT * FROM table_name', engine)

# Or with connection
# df = pd.read_sql('SELECT * FROM table_name', con)
```

---

### pd.read_clipboard

**Syntax:**
```python
pd.read_clipboard(sep='\\s+', dtype=None, **kwargs)
```

**Explanation:** Reads text from clipboard and parses it into DataFrame.

**Examples:**
```python
# Copy data from Excel/CSV and run:
# df = pd.read_clipboard()
```

---

## 3. Data Selection and Indexing

### df.loc

**Syntax:**
```python
DataFrame.loc[rows, columns]
```

**Explanation:** Label-based indexing for selecting rows and columns by labels.

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],
    'Age': [25, 30, 35, 40],
    'Salary': [50000, 60000, 70000, 80000]
}, index=['a', 'b', 'c', 'd'])

# Select single row by label
print(df.loc['a'])

# Select multiple rows
print(df.loc[['a', 'b']])

# Select row with slice
print(df.loc['a':'c'])

# Select specific cell
print(df.loc['a', 'Name'])

# Select rows and columns
print(df.loc[['a', 'b'], ['Name', 'Salary']])
```

---

### df.iloc

**Syntax:**
```python
DataFrame.iloc[rows, cols]
```

**Explanation:** Integer-based indexing for selecting by position (0-based).

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],
    'Age': [25, 30, 35, 40],
    'Salary': [50000, 60000, 70000, 80000]
})

# Select single row by integer position
print(df.iloc[0])

# Select multiple rows
print(df.iloc[[0, 1]])

# Select rows with slice
print(df.iloc[0:3])

# Select specific cell
print(df.iloc[0, 0])

# Select rows and columns by position
print(df.iloc[[0, 1], [0, 2]])
```

---

### df.at

**Syntax:**
```python
DataFrame.at[label, column]
```

**Explanation:** Used for getting/setting a single value using label-based indexing (faster than loc for single values).

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob'],
    'Age': [25, 30]
}, index=['a', 'b'])

# Get single value
print(df.at['a', 'Name'])

# Set single value
df.at['a', 'Name'] = 'Alicia'
print(df)
```

---

### df.iat

**Syntax:**
```python
DataFrame.iat[row, col]
```

**Explanation:** Used for getting/setting a single value using integer position (faster than iloc for single values).

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob'],
    'Age': [25, 30]
})

# Get single value by position
print(df.iat[0, 0])

# Set single value by position
df.iat[0, 0] = 'Alicia'
print(df)
```

---

### df[column]

**Syntax:**
```python
DataFrame[column_name]
DataFrame[[column1, column2, ...]]
```

**Explanation:** Direct column selection using column names as keys.

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35]
})

# Select single column
print(df['Name'])

# Select multiple columns
print(df[['Name', 'Age']])
```

---

### df.query

**Syntax:**
```python
DataFrame.query(expr, inplace=False, **kwargs)
```

**Explanation:** Filters data using a boolean expression (similar to SQL WHERE clause).

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],
    'Age': [25, 30, 35, 40],
    'Salary': [50000, 60000, 70000, 80000]
})

# Query with condition
result = df.query('Age > 30')
print(result)

# Multiple conditions
result = df.query('Age > 25 and Salary > 60000')
print(result)

# With variables
threshold = 30
result = df.query('Age > @threshold')
print(result)
```

---

### df.filter

**Syntax:**
```python
DataFrame.filter(items=None, like=None, regex=None, axis=None)
```

**Explanation:** Filters columns or rows by names/items/regex patterns.

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob'],
    'Age': [25, 30],
    'Salary': [50000, 60000],
    'Department': ['HR', 'IT']
})

# Filter by items (column names)
print(df.filter(items=['Name', 'Age']))

# Filter by like (contains)
print(df.filter(like='a', axis=1))

# Filter by regex
print(df.filter(regex='^S', axis=1))
```

---

## 4. Data Manipulation

### df.drop

**Syntax:**
```python
DataFrame.drop(labels=None, axis=0, index=None, columns=None, level=None, inplace=False, errors='raise')
```

**Explanation:** Removes rows or columns by specifying labels and axis.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6],
    'C': [7, 8, 9]
})

# Drop columns
result = df.drop('A', axis=1)
print(result)

# Drop multiple columns
result = df.drop(['A', 'B'], axis=1)
print(result)

# Drop rows
result = df.drop([0, 1])
print(result)

# Inplace drop
df.drop('C', axis=1, inplace=True)
print(df)
```

---

### df.fillna

**Syntax:**
```python
DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None)
```

**Explanation:** Fills NA/NaN values with specified value or method.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, np.nan, 3],
    'B': [4, 5, np.nan],
    'C': [np.nan, 8, 9]
})

# Fill with scalar
result = df.fillna(0)
print(result)

# Fill with dictionary (different values per column)
result = df.fillna({'A': 10, 'B': 20, 'C': 30})
print(result)

# Forward fill (method='ffill')
result = df.fillna(method='ffill')
print(result)

# Backward fill (method='bfill')
result = df.fillna(method='bfill')
print(result)
```

---

### df.dropna

**Syntax:**
```python
DataFrame.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
```

**Explanation:** Removes rows/columns with missing values (NaN).

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, np.nan, 3],
    'B': [4, 5, np.nan],
    'C': [np.nan, 8, 9]
})

# Drop rows with any NaN
result = df.dropna()
print(result)

# Drop columns with any NaN
result = df.dropna(axis=1)
print(result)

# Drop rows with all NaN
result = df.dropna(how='all')
print(result)

# Drop with threshold (at least 2 non-NaN values)
result = df.dropna(thresh=2)
print(result)

# Drop based on specific column
result = df.dropna(subset=['A', 'B'])
print(result)
```

---

### df.replace

**Syntax:**
```python
DataFrame.replace(to_replace=None, value=None, inplace=False, limit=None, regex=False, method='pad', axis=None)
```

**Explanation:** Replaces values found in the DataFrame with another value.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6],
    'C': ['a', 'b', 'c']
})

# Replace single value
result = df.replace(1, 100)
print(result)

# Replace multiple values
result = df.replace([1, 2], [100, 200])
print(result)

# Replace with dictionary
result = df.replace({1: 100, 'a': 'x'})
print(result)

# Using regex
df2 = pd.DataFrame({'A': ['a1', 'a2', 'b1']})
result = df2.replace(regex=r'a', value='X')
print(result)
```

---

### df.rename

**Syntax:**
```python
DataFrame.rename(mapper=None, index=None, columns=None, axis=None, copy=False, inplace=False, level=None)
```

**Explanation:** Alters axes labels (index or columns) using a mapping.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2],
    'B': [3, 4]
})

# Rename columns
result = df.rename(columns={'A': 'X', 'B': 'Y'})
print(result)

# Rename index
result = df.rename(index={0: 'a', 1: 'b'})
print(result)

# Rename using function
result = df.rename(columns=str.lower)
print(result)
```

---

### df.insert

**Syntax:**
```python
DataFrame.insert(loc, column, value, allow_duplicates=False)
```

**Explanation:** Inserts a column at a specific location in the DataFrame.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

# Insert column at position 1
df.insert(1, 'C', [7, 8, 9])
print(df)

# Insert at the end
df.insert(len(df.columns), 'D', [10, 11, 12])
print(df)
```

---

### df.assign

**Syntax:**
```python
DataFrame.assign(**kwargs)
```

**Explanation:** Adds new columns to a DataFrame, returning a new object.

**Examples:**
```python
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})

# Assign new column
result = df.assign(C=[7, 8, 9])
print(result)

# Assign using lambda
result = df.assign(C=lambda x: x['A'] + x['B'])
print(result)

# Assign multiple columns
result = df.assign(C=[7, 8, 9], D=lambda x: x['A'] * 2)
print(result)
```

---

### df.sort_values

**Syntax:**
```python
DataFrame.sort_values(by, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last', ignore_index=False, key=None)
```

**Explanation:** Sorts the DataFrame by specified axis (rows or columns).

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Charlie', 'Alice', 'Bob'],
    'Age': [35, 25, 30],
    'Salary': [70000, 50000, 60000]
})

# Sort by single column
result = df.sort_values('Age')
print(result)

# Sort descending
result = df.sort_values('Age', ascending=False)
print(result)

# Sort by multiple columns
result = df.sort_values(['Age', 'Salary'])
print(result)

# Sort with different orders
result = df.sort_values(['Age', 'Salary'], ascending=[True, False])
print(result)
```

---

### df.sort_index

**Syntax:**
```python
DataFrame.sort_index(axis=0, level=None, ascending=True, inplace=False, kind='quicksort', na_position='last', sort_remaining=True, ignore_index=False, key=None)
```

**Explanation:** Sorts the DataFrame by index (row labels or column names).

**Examples:**
```python
df = pd.DataFrame({'B': [1, 2], 'A': [3, 4]}, index=['b', 'a'])

# Sort index (rows)
result = df.sort_index()
print(result)

# Sort columns
result = df.sort_index(axis=1)
print(result)

# Sort descending
result = df.sort_index(ascending=False)
print(result)
```

---

### df.reset_index

**Syntax:**
```python
DataFrame.reset_index(level=None, drop=False, inplace=False, col_level=0, col_fill='')
```

**Explanation:** Resets the index to default integer index.

**Examples:**
```python
df = pd.DataFrame({'A': [1, 2, 3]}, index=['a', 'b', 'c'])

# Reset index
result = df.reset_index()
print(result)

# Drop old index
result = df.reset_index(drop=True)
print(result)
```

---

### df.set_index

**Syntax:**
```python
DataFrame.set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False)
```

**Explanation:** Sets the DataFrame index using existing columns.

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie'],
    'Age': [25, 30, 35]
})

# Set index
result = df.set_index('Name')
print(result)

# Set multiple indexes
result = df.set_index(['Name', 'Age'])
print(result)
```

---

### df.transpose

**Syntax:**
```python
DataFrame.transpose(*args, **kwargs)
```

**Explanation:** Returns the transpose of the DataFrame (rows become columns and vice versa). Alias: `df.T`

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.T
print(result)
```

---

### df.melt

**Syntax:**
```python
DataFrame.melt(id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None, ignore_index=True)
```

**Explanation:** Unpivots a DataFrame from wide to long format.

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob'],
    'Age': [25, 30],
    'Salary': [50000, 60000]
})

result = df.melt(id_vars=['Name'], var_name='Attribute', value_name='Value')
print(result)
```

---

### df.pivot

**Syntax:**
```python
DataFrame.pivot(index=None, columns=None, values=None)
```

**Explanation:** Reshapes data from long to wide format.

**Examples:**
```python
df = pd.DataFrame({
    'Date': ['2023-01', '2023-01', '2023-02', '2023-02'],
    'Product': ['A', 'B', 'A', 'B'],
    'Sales': [100, 200, 150, 250]
})

result = df.pivot(index='Date', columns='Product', values='Sales')
print(result)
```

---

## 5. Data Transformation

### df.apply

**Syntax:**
```python
DataFrame.apply(func, axis=0, raw=False, result_type=None, args=(), **kwargs)
```

**Explanation:** Applies a function along an axis of the DataFrame.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

# Apply function to each column
result = df.apply(np.sum)
print(result)

# Apply function to each row
result = df.apply(np.sum, axis=1)
print(result)

# Apply custom function
result = df.apply(lambda x: x * 2)
print(result)
```

---

### df.applymap

**Syntax:**
```python
DataFrame.applymap(func, na_action=None)
```

**Explanation:** Applies a function element-wise (deprecated in newer pandas, use df.map() instead).

**Examples:**
```python
df = pd.DataFrame({
    'A': [1.5, 2.5, 3.5],
    'B': [4.5, 5.5, 6.5]
})

# Apply function to each element
result = df.applymap(lambda x: int(x))
print(result)

# Using map (newer pandas)
result = df.map(lambda x: int(x))
print(result)
```

---

### Series.map

**Syntax:**
```python
Series.map(arg, na_action=None)
```

**Explanation:** Maps values using input correspondence (dict, Series, or function).

**Examples:**
```python
s = pd.Series(['cat', 'dog', 'mouse'])

# Map with dictionary
result = s.map({'cat': 'kitten', 'dog': 'puppy'})
print(result)

# Map with function
result = s.map(len)
print(result)

# Map with lambda
result = s.map(lambda x: x.upper())
print(result)
```

---

### df.transform

**Syntax:**
```python
DataFrame.transform(func, axis=0, *args, **kwargs)
```

**Explanation:** Calls function on each element and produces a DataFrame with same shape.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

# Transform each element
result = df.transform(np.sqrt)
print(result)

# Multiple transformations
result = df.transform([np.sqrt, np.exp])
print(result)
```

---

### pd.get_dummies

**Syntax:**
```python
pd.get_dummies(data, prefix=None, prefix_sep='_', dummy_na=False, columns=None, sparse=False, drop_first=False, dtype=None)
```

**Explanation:** Converts categorical variables into dummy/indicator variables.

**Examples:**
```python
df = pd.DataFrame({
    'Color': ['Red', 'Green', 'Blue'],
    'Size': ['S', 'M', 'L']
})

# Get dummies for one column
result = pd.get_dummies(df['Color'])
print(result)

# Get dummies for all columns
result = pd.get_dummies(df)
print(result)

# With prefix
result = pd.get_dummies(df['Color'], prefix='color')
print(result)
```

---

### pd.cut

**Syntax:**
```python
pd.cut(x, bins, right=True, labels=None, retbins=False, precision=3, include_lowest=False)
```

**Explanation:** Bins values into discrete intervals.

**Examples:**
```python
# Bin values into categories
data = [1, 5, 10, 15, 20]
result = pd.cut(data, bins=[0, 5, 10, 15, 20])
print(result)

# With labels
result = pd.cut(data, bins=[0, 5, 10, 15, 20], labels=['low', 'medium', 'high', 'very high'])
print(result)
```

---

### pd.qcut

**Syntax:**
```python
pd.qcut(x, q, labels=None, retbins=False, precision=3, duplicates='drop')
```

**Explanation:** Bins values into quantile-based discrete intervals.

**Examples:**
```python
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
result = pd.qcut(data, q=4)
print(result)
```

---

## 6. Merge, Join, and Concatenate

### pd.merge

**Syntax:**
```python
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)
```

**Explanation:** Performs database-style join operations on DataFrame objects.

**Examples:**
```python
left = pd.DataFrame({
    'key': ['K0', 'K1', 'K2'],
    'A': ['A0', 'A1', 'A2']
})

right = pd.DataFrame({
    'key': ['K0', 'K1', 'K2'],
    'B': ['B0', 'B1', 'B2']
})

# Inner join
result = pd.merge(left, right, on='key', how='inner')
print(result)

# Outer join
result = pd.merge(left, right, on='key', how='outer')
print(result)

# Left join
result = pd.merge(left, right, on='key', how='left')
print(result)

# Right join
result = pd.merge(left, right, on='key', how='right')
print(result)
```

---

### DataFrame.join

**Syntax:**
```python
DataFrame.join(other, on=None, how='left', lsuffix='', rsuffix='', sort=False)
```

**Explanation:** Joins columns of another DataFrame on index.

**Examples:**
```python
left = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
}, index=['K0', 'K1', 'K2'])

right = pd.DataFrame({
    'C': [7, 8, 9],
    'D': [10, 11, 12]
}, index=['K0', 'K1', 'K2'])

# Join
result = left.join(right)
print(result)

# Outer join
result = left.join(right, how='outer')
print(result)
```

---

### pd.concat

**Syntax:**
```python
pd.concat(objs, axis=0, join='outer', ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=False, copy=True)
```

**Explanation:** Concatenates pandas objects along a particular axis.

**Examples:**
```python
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

# Concatenate rows
result = pd.concat([df1, df2])
print(result)

# Concatenate columns
df3 = pd.DataFrame({'C': [9, 10]})
result = pd.concat([df1, df3], axis=1)
print(result)

# Ignore index
result = pd.concat([df1, df2], ignore_index=True)
print(result)
```

---

### DataFrame.combine_first

**Syntax:**
```python
DataFrame.combine_first(other)
```

**Explanation:** Updates null values with values from another DataFrame.

**Examples:**
```python
df1 = pd.DataFrame({'A': [1, np.nan, 3], 'B': [4, 5, np.nan]})
df2 = pd.DataFrame({'A': [np.nan, 10, 20], 'B': [30, np.nan, 50]})

result = df1.combine_first(df2)
print(result)
```

---

## 7. GroupBy and Aggregation

### df.groupby

**Syntax:**
```python
DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True, group_keys=True, squeeze=False, observed=False, dropna=True)
```

**Explanation:** Groups DataFrame using a mapper or by a Series of columns.

**Examples:**
```python
df = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B', 'A', 'B'],
    'Value': [1, 2, 3, 4, 5, 6]
})

# Group by single column
grouped = df.groupby('Category')
for name, group in grouped:
    print(f"\n{name}:")
    print(group)

# Aggregate after groupby
result = df.groupby('Category').sum()
print(result)

# Group by multiple columns
df2 = pd.DataFrame({
    'Cat1': ['A', 'A', 'B', 'B'],
    'Cat2': ['X', 'Y', 'X', 'Y'],
    'Value': [1, 2, 3, 4]
})
result = df2.groupby(['Cat1', 'Cat2']).sum()
print(result)
```

---

### GroupBy.agg

**Syntax:**
```python
GroupBy.agg(func, *args, **kwargs)
```

**Explanation:** Aggregates using one or more operations over the specified axis.

**Examples:**
```python
df = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B'],
    'Value': [1, 2, 3, 4],
    'Count': [10, 20, 30, 40]
})

# Multiple aggregation functions
result = df.groupby('Category').agg({'Value': 'sum', 'Count': 'mean'})
print(result)

# Using list of functions
result = df.groupby('Category')['Value'].agg(['sum', 'mean', 'min', 'max'])
print(result)

# Using custom function
result = df.groupby('Category').agg({'Value': lambda x: x.max() - x.min()})
print(result)
```

---

### GroupBy.filter

**Syntax:**
```python
GroupBy.filter(func, dropna=True)
```

**Explanation:** Filters groups based on a condition.

**Examples:**
```python
df = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B'],
    'Value': [1, 100, 3, 4]
})

# Filter groups
result = df.groupby('Category').filter(lambda x: x['Value'].sum() > 10)
print(result)
```

---

### GroupBy.transform

**Syntax:**
```python
GroupBy.transform(func, *args, **kwargs)
```

**Explanation:** Transforms values using a function within groups.

**Examples:**
```python
df = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B'],
    'Value': [1, 2, 3, 4]
})

# Transform within groups
result = df.groupby('Category')['Value'].transform(lambda x: x - x.mean())
print(result)
```

---

### GroupBy.apply

**Syntax:**
```python
GroupBy.apply(func, *args, **kwargs)
```

**Explanation:** Applies function to each group.

**Examples:**
```python
df = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B'],
    'Value': [1, 2, 3, 4]
})

# Apply function to groups
result = df.groupby('Category').apply(lambda x: x * 2)
print(result)
```

---

### pd.crosstab

**Syntax:**
```python
pd.crosstab(index, columns, values=None, aggfunc=None, margins=False, margins_name='All', dropna=True, normalize=False)
```

**Explanation:** Computes a cross-tabulation of two (or more) factors.

**Examples:**
```python
df = pd.DataFrame({
    'Gender': ['M', 'F', 'M', 'F', 'M', 'F'],
    'Dept': ['IT', 'IT', 'HR', 'HR', 'IT', 'HR'],
    'Count': [1, 2, 3, 4, 5, 6]
})

# Create crosstab
result = pd.crosstab(df['Gender'], df['Dept'])
print(result)

# With values and aggfunc
result = pd.crosstab(df['Gender'], df['Dept'], values=df['Count'], aggfunc='sum')
print(result)
```

---

### pd.pivot_table

**Syntax:**
```python
pd.pivot_table(data, values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All', observed=False, sort=True)
```

**Explanation:** Creates a spreadsheet-style pivot table as a DataFrame.

**Examples:**
```python
df = pd.DataFrame({
    'Date': ['2023-01', '2023-01', '2023-02', '2023-02'],
    'Product': ['A', 'B', 'A', 'B'],
    'Sales': [100, 200, 150, 250]
})

# Simple pivot table
result = pd.pivot_table(df, values='Sales', index='Date', columns='Product')
print(result)

# With aggregation
result = pd.pivot_table(df, values='Sales', index='Date', columns='Product', aggfunc='sum')
print(result)
```

---

## 8. String Operations

### str.lower

**Syntax:**
```python
Series.str.lower()
```

**Explanation:** Converts strings to lowercase.

**Examples:**
```python
s = pd.Series(['ALICE', 'BOB', 'CHARLIE'])
result = s.str.lower()
print(result)
```

---

### str.upper

**Syntax:**
```python
Series.str.upper()
```

**Explanation:** Converts strings to uppercase.

**Examples:**
```python
s = pd.Series(['alice', 'bob', 'charlie'])
result = s.str.upper()
print(result)
```

---

### str.strip

**Syntax:**
```python
Series.str.strip(to_strip=None)
```

**Explanation:** Removes leading and trailing whitespace.

**Examples:**
```python
s = pd.Series(['  alice  ', 'bob', '  charlie'])
result = s.str.strip()
print(result)
```

---

### str.split

**Syntax:**
```python
Series.str.split(pat=None, n=-1, expand=False, regex=None)
```

**Explanation:** Splits strings by delimiter (or regex).

**Examples:**
```python
s = pd.Series(['a,b,c', 'd,e,f', 'g,h,i'])

# Split by delimiter
result = s.str.split(',')
print(result)

# Split and expand to DataFrame
result = s.str.split(',', expand=True)
print(result)
```

---

### str.replace

**Syntax:**
```python
Series.str.replace(pat, repl, n=-1, case=None, flags=0, regex=None)
```

**Explanation:** Replaces substring/regex pattern with another string.

**Examples:**
```python
s = pd.Series(['cat', 'dog', 'mouse'])

# Replace substring
result = s.str.replace('at', 'AT')
print(result)

# Using regex
result = s.str.replace(r'\w+', 'animal')
print(result)
```

---

### str.contains

**Syntax:**
```python
Series.str.contains(pat, case=True, flags=0, na=None, regex=True)
```

**Explanation:** Returns boolean array if pattern is contained in each string.

**Examples:**
```python
s = pd.Series(['cat', 'dog', 'mouse', 'catfish'])

# Check if contains substring
result = s.str.contains('cat')
print(result)

# Using regex
result = s.str.contains(r'^c.*t$')
print(result)
```

---

### str.startswith

**Syntax:**
```python
Series.str.startswith(prefix, na=None)
```

**Explanation:** Returns True if string starts with prefix.

**Examples:**
```python
s = pd.Series(['cat', 'dog', 'cow', 'catapult'])
result = s.str.startswith('c')
print(result)
```

---

### str.endswith

**Syntax:**
```python
Series.str.endswith(suffix, na=None)
```

**Explanation:** Returns True if string ends with suffix.

**Examples:**
```python
s = pd.Series(['cat', 'dog', 'log', 'fog'])
result = s.str.endswith('g')
print(result)
```

---

### str.len

**Syntax:**
```python
Series.str.len()
```

**Explanation:** Returns the length of each string.

**Examples:**
```python
s = pd.Series(['alice', 'bob', 'charlie'])
result = s.str.len()
print(result)
```

---

### str.find

**Syntax:**
```python
Series.str.find(sub, start=0, end=None)
```

**Explanation:** Returns the first position of substring (or -1 if not found).

**Examples:**
```python
s = pd.Series(['alice', 'bob', 'charlie'])
result = s.str.find('a')
print(result)
```

---

### str.count

**Syntax:**
```python
Series.str.count(pat, flags=0, **kwargs)
```

**Explanation:** Counts occurrences of pattern in each string.

**Examples:**
```python
s = pd.Series(['cat', 'cattle', 'catastrophe'])
result = s.str.count('cat')
print(result)
```

---

### str.concat

**Syntax:**
```python
Series.str.concat(others, join='left', na_rep=None)
```

**Explanation:** Concatenates strings element-wise.

**Examples:**
```python
s1 = pd.Series(['Hello', 'World'])
s2 = pd.Series([' ', '!'])
result = s1.str.concat(s2)
print(result)
```

---

### str.slice

**Syntax:**
```python
Series.str.slice(start=None, stop=None, step=None)
```

**Explanation:** Slice substrings from each element.

**Examples:**
```python
s = pd.Series(['abcdef', 'ghijkl'])
result = s.str.slice(0, 3)
print(result)
```

---

### str.join

**Syntax:**
```python
Series.str.join(sep)
```

**Explanation:** Join strings in each element with given separator.

**Examples:**
```python
s = pd.Series([['a', 'b', 'c'], ['d', 'e']])
result = s.str.join('-')
print(result)
```

---

### str.extract

**Syntax:**
```python
Series.str.extract(pat, flags=0, expand=True)
```

**Explanation:** Extracts groups from the first match of regular expression.

**Examples:**
```python
s = pd.Series(['a1', 'a2', 'b3'])
result = s.str.extract(r'([a-z])(\d)')
print(result)
```

---

### str.isalpha

**Syntax:**
```python
Series.str.isalpha()
```

**Explanation:** Returns True if all characters are alphabetic.

**Examples:**
```python
s = pd.Series(['abc', '123', 'a1'])
result = s.str.isalpha()
print(result)
```

---

### str.isnumeric

**Syntax:**
```python
Series.str.isnumeric()
```

**Explanation:** Returns True if all characters are numeric.

**Examples:**
```python
s = pd.Series(['123', 'abc', '12a'])
result = s.str.isnumeric()
print(result)
```

---

## 9. DateTime Operations

### pd.to_datetime

**Syntax:**
```python
pd.to_datetime(arg, errors='raise', dayfirst=False, yearfirst=False, utc=False, format=None, exact=True, unit='ns', origin='unix', cache=True)
```

**Explanation:** Converts argument to datetime.

**Examples:**
```python
# Convert string to datetime
dates = ['2023-01-01', '2023-01-02', '2023-01-03']
result = pd.to_datetime(dates)
print(result)

# Convert with format
result = pd.to_datetime('2023/01/01', format='%Y/%m/%d')
print(result)
```

---

### pd.date_range

**Syntax:**
```python
pd.date_range(start=None, end=None, periods=None, freq=None, tz=None, normalize=False, name=None, closed=None, **kwargs)
```

**Explanation:** Returns a fixed frequency DatetimeIndex.

**Examples:**
```python
# Date range with start and end
result = pd.date_range(start='2023-01-01', end='2023-01-10')
print(result)

# Date range with periods
result = pd.date_range(start='2023-01-01', periods=10, freq='D')
print(result)

# With frequency
result = pd.date_range(start='2023-01-01', periods=5, freq='M')
print(result)
```

---

### pd.timedelta_range

**Syntax:**
```python
pd.timedelta_range(start=None, end=None, periods=None, freq=None, name=None, closed=None)
```

**Explanation:** Returns a fixed frequency TimedeltaIndex.

**Examples:**
```python
# Timedelta range
result = pd.timedelta_range(start='1 day', periods=5, freq='D')
print(result)
```

---

### Series.dt.year

**Syntax:**
```python
Series.dt.year
```

**Explanation:** Returns the year of the datetime.

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01', periods=5, freq='M'))
result = s.dt.year
print(result)
```

---

### Series.dt.month

**Syntax:**
```python
Series.dt.month
```

**Explanation:** Returns the month of the datetime.

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01', periods=5, freq='M'))
result = s.dt.month
print(result)
```

---

### Series.dt.day

**Syntax:**
```python
Series.dt.day
```

**Explanation:** Returns the day of the datetime.

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01', periods=5, freq='D'))
result = s.dt.day
print(result)
```

---

### Series.dt.hour

**Syntax:**
```python
Series.dt.hour
```

**Explanation:** Returns the hour of the datetime.

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01', periods=5, freq='H'))
result = s.dt.hour
print(result)
```

---

### Series.dt.dayofweek

**Syntax:**
```python
Series.dt.dayofweek
```

**Explanation:** Returns the day of the week (0=Monday, 6=Sunday).

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01', periods=7, freq='D'))
result = s.dt.dayofweek
print(result)
```

---

### Series.dt.strftime

**Syntax:**
```python
Series.dt.strftime(date_format)
```

**Explanation:** Converts datetime to string format.

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01', periods=3, freq='D'))
result = s.dt.strftime('%Y-%m-%d')
print(result)
```

---

### Series.dt.floor

**Syntax:**
```python
Series.dt.floor(freq, ambiguous='raise', nonexistent='raise')
```

**Explanation:** Floors datetime to specified frequency.

**Examples:**
```python
s = pd.Series(pd.date_range('2023-01-01 12:30:00', periods=3, freq='H'))
result = s.dt.floor('D')
print(result)
```

---

## 10. Statistical Functions

### df.describe

**Syntax:**
```python
DataFrame.describe(percentiles=None, include=None, exclude=None, datetime_is_numeric=False)
```

**Explanation:** Generates descriptive statistics summary.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, 30, 40, 50]
})

result = df.describe()
print(result)
```

---

### df.mean

**Syntax:**
```python
DataFrame.mean(axis=None, skipna=None, level=None, numeric_only=None, **kwargs)
```

**Explanation:** Returns the mean of the values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.mean()
print(result)

# By axis
result = df.mean(axis=1)
print(result)
```

---

### df.median

**Syntax:**
```python
DataFrame.median(axis=None, skipna=None, level=None, numeric_only=None, **kwargs)
```

**Explanation:** Returns the median of the values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, 30, 40, 50]
})

result = df.median()
print(result)
```

---

### df.std

**Syntax:**
```python
DataFrame.std(axis=None, skipna=None, level=None, ddof=1, numeric_only=None, **kwargs)
```

**Explanation:** Returns the standard deviation.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, 30, 40, 50]
})

result = df.std()
print(result)
```

---

### df.min

**Syntax:**
```python
DataFrame.min(axis=None, skipna=None, level=None, numeric_only=None, **kwargs)
```

**Explanation:** Returns the minimum of the values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.min()
print(result)
```

---

### df.max

**Syntax:**
```python
DataFrame.max(axis=None, skipna=None, level=None, numeric_only=None, **kwargs)
```

**Explanation:** Returns the maximum of the values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.max()
print(result)
```

---

### df.sum

**Syntax:**
```python
DataFrame.sum(axis=None, skipna=None, level=None, numeric_only=None, min_count=0, **kwargs)
```

**Explanation:** Returns the sum of the values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.sum()
print(result)
```

---

### df.count

**Syntax:**
```python
DataFrame.count(axis=0, level=None, numeric_only=False)
```

**Explanation:** Returns the count of non-NA values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, np.nan, 3],
    'B': [4, 5, 6]
})

result = df.count()
print(result)
```

---

### df.corr

**Syntax:**
```python
DataFrame.corr(method='pearson', min_periods=1, numeric_only=False)
```

**Explanation:** Computes pairwise correlation.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [2, 4, 6, 8, 10],
    'C': [5, 4, 3, 2, 1]
})

result = df.corr()
print(result)
```

---

### df.cov

**Syntax:**
```python
DataFrame.cov(min_periods=None, ddof=1, numeric_only=False)
```

**Explanation:** Computes pairwise covariance.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [2, 4, 6, 8, 10]
})

result = df.cov()
print(result)
```

---

### df.cumsum

**Syntax:**
```python
DataFrame.cumsum(axis=None, skipna=True, *args, **kwargs)
```

**Explanation:** Returns cumulative sum.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.cumsum()
print(result)
```

---

### df.cumprod

**Syntax:**
```python
DataFrame.cumprod(axis=None, skipna=True, *args, **kwargs)
```

**Explanation:** Returns cumulative product.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.cumprod()
print(result)
```

---

### df.abs

**Syntax:**
```python
DataFrame.abs()
```

**Explanation:** Returns absolute value.

**Examples:**
```python
df = pd.DataFrame({
    'A': [-1, -2, 3],
    'B': [4, -5, 6]
})

result = df.abs()
print(result)
```

---

### df.round

**Syntax:**
```python
DataFrame.round(decimals=0, *args, **kwargs)
```

**Explanation:** Rounds to specified decimals.

**examples:**
```python
df = pd.DataFrame({
    'A': [1.234, 5.678, 9.012],
    'B': [3.456, 7.890, 1.234]
})

result = df.round(2)
print(result)
```

---

### df.var

**Syntax:**
```python
DataFrame.var(axis=None, skipna=None, level=None, ddof=1, numeric_only=None, **kwargs)
```

**Explanation:** Returns the variance.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, 30, 40, 50]
})

result = df.var()
print(result)
```

---

### df.quantile

**Syntax:**
```python
DataFrame.quantile(q=0.5, axis=0, numeric_only=True, interpolation='linear')
```

**Explanation:** Returns quantile.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    'B': [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
})

result = df.quantile(0.5)
print(result)

# Multiple quantiles
result = df.quantile([0.25, 0.5, 0.75])
print(result)
```

---

## 11. Window Functions

### df.rolling

**Syntax:**
```python
DataFrame.rolling(window, min_periods=None, center=False, win_type=None, on=None, axis=0, closed=None, step=None, method='single')
```

**Explanation:** Provides rolling window calculations.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
})

# Rolling sum
result = df.rolling(window=3).sum()
print(result)

# Rolling mean
result = df.rolling(window=3).mean()
print(result)
```

---

### df.expanding

**Syntax:**
```python
DataFrame.expanding(min_periods=1, center=False, axis=0, method='single')
```

**Explanation:** Provides expanding window calculations.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5]
})

# Expanding sum
result = df.expanding().sum()
print(result)

# Expanding mean
result = df.expanding().mean()
print(result)
```

---

### df.ewm

**Syntax:**
```python
DataFrame.ewm(com=None, span=None, halflife=None, alpha=None, min_periods=0, adjust=True, ignore_na=False, axis=0, times=None, method='single')
```

**Explanation:** Provides exponential weighted functions.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5]
})

# Exponential weighted moving average
result = df.ewm(span=3).mean()
print(result)
```

---

## 12. Plotting

### df.plot

**Syntax:**
```python
DataFrame.plot(x=None, y=None, kind='line', ax=None, subplots=False, sharex=None, sharey=False, layout=None, figsize=None, use_index=True, title=None, grid=None, legend=True, style=None, logx=False, logy=False, loglog=False, xticks=None, yticks=None, xlim=None, ylim=None, rot=None, xlabel=None, ylabel=None, fontdict=None, label=None, secondary_y=False)
```

**Explanation:** Creates various types of plots.

**Examples:**
```python
import matplotlib.pyplot as plt

df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [5, 4, 3, 2, 1]
})

# Line plot
df.plot()
plt.show()

# Bar plot
df.plot(kind='bar')
plt.show()

# Scatter plot
df.plot(kind='scatter', x='A', y='B')
plt.show()

# Histogram
df.plot(kind='hist')
plt.show()

# Box plot
df.plot(kind='box')
plt.show()

# Area plot
df.plot(kind='area')
plt.show()
```

---

### df.hist

**Syntax:**
```python
DataFrame.hist(column=None, by=None, grid=True, xlabelsize=None, xrot=None, ylabelsize=None, yrot=None, ax=None, sharex=False, sharey=False, figsize=None, layout=None, bins=10, backend=None, **kwargs)
```

**Explanation:** Creates histogram plot.

**Examples:**
```python
df = pd.DataFrame({
    'A': np.random.randn(100),
    'B': np.random.randn(100)
})

df.hist(bins=20)
plt.show()
```

---

### df.boxplot

**Syntax:**
```python
DataFrame.boxplot(column=None, by=None, ax=None, fontsize=None, rot=0, grid=True, figsize=None, layout=None, return_type=None, showmeans=False, patch_artist=False)
```

**Explanation:** Creates boxplot.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [2, 4, 6, 8, 10],
    'C': [5, 4, 3, 2, 1]
})

df.boxplot()
plt.show()
```

---

## 13. Utilities and Info

### df.info

**Syntax:**
```python
DataFrame.info(verbose=None, buf=None, max_cols=None, memory_usage=None, show_counts=None)
```

**Explanation:** Prints a concise summary of DataFrame.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': ['a', 'b', 'c']
})

df.info()
```

---

### df.dtypes

**Syntax:**
```python
DataFrame.dtypes
```

**Explanation:** Returns the data types of each column.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': ['a', 'b', 'c'],
    'C': [1.5, 2.5, 3.5]
})

print(df.dtypes)
```

---

### df.shape

**Syntax:**
```python
DataFrame.shape
```

**Explanation:** Returns the dimensions of the DataFrame (rows, columns).

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

print(df.shape)  # (3, 2)
```

---

### df.columns

**Syntax:**
```python
DataFrame.columns
```

**Explanation:** Returns the column labels.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

print(df.columns)
```

---

### df.index

**Syntax:**
```python
DataFrame.index
```

**Explanation:** Returns the index (row labels).

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3]
}, index=['a', 'b', 'c'])

print(df.index)
```

---

### df.head

**Syntax:**
```python
DataFrame.head(n=5)
```

**Explanation:** Returns the first n rows.

**Examples:**
```python
df = pd.DataFrame({
    'A': range(100)
})

print(df.head(10))
```

---

### df.tail

**Syntax:**
```python
DataFrame.tail(n=5)
```

**Explanation:** Returns the last n rows.

**Examples:**
```python
df = pd.DataFrame({
    'A': range(100)
})

print(df.tail(10))
```

---

### df.sample

**Syntax:**
```python
DataFrame.sample(n=None, frac=None, replace=False, weights=None, random_state=None, axis=None)
```

**Explanation:** Returns random sample of rows.

**Examples:**
```python
df = pd.DataFrame({
    'A': range(100)
})

# Sample 10 rows
print(df.sample(n=10))

# Sample 10% of data
print(df.sample(frac=0.1))
```

---

### df.isnull

**Syntax:**
```python
DataFrame.isnull()
```

**Explanation:** Returns boolean DataFrame showing missing values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, np.nan, 3],
    'B': [4, 5, np.nan]
})

print(df.isnull())
```

---

### df.notnull

**Syntax:**
```python
DataFrame.notnull()
```

**Explanation:** Returns boolean DataFrame showing non-missing values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, np.nan, 3],
    'B': [4, 5, np.nan]
})

print(df.notnull())
```

---

### df.duplicated

**Syntax:**
```python
DataFrame.duplicated(subset=None, keep='first')
```

**Explanation:** Returns boolean Series for duplicated rows.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 2, 3],
    'B': ['a', 'b', 'b', 'c']
})

print(df.duplicated())
```

---

### df.drop_duplicates

**Syntax:**
```python
DataFrame.drop_duplicates(subset=None, keep='first', inplace=False, ignore_index=False)
```

**Explanation:** Removes duplicate rows.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 2, 3],
    'B': ['a', 'b', 'b', 'c']
})

result = df.drop_duplicates()
print(result)
```

---

### df.memory_usage

**Syntax:**
```python
DataFrame.memory_usage(deep=False)
```

**Explanation:** Returns memory usage of each column.

**Examples:**
```python
df = pd.DataFrame({
    'A': range(1000)
})

print(df.memory_usage())
```

---

### df.nunique

**Syntax:**
```python
DataFrame.nunique(axis=0, dropna=True)
```

**Explanation:** Returns count of unique values.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 2, 3],
    'B': ['a', 'b', 'b', 'c']
})

print(df.nunique())
```

---

### df.value_counts

**Syntax:**
```python
Series.value_counts(normalize=False, sort=True, ascending=False, bins=None, dropna=True)
```

**Explanation:** Returns counts of unique values.

**Examples:**
```python
s = pd.Series([1, 2, 2, 3, 3, 3])
print(s.value_counts())
```

---

### pd.isna

**Syntax:**
```python
pd.isna(obj)
```

**Explanation:** Detects missing values.

**Examples:**
```python
print(pd.isna(np.nan))
print(pd.isna(None))
print(pd.isna(pd.NA))
```

---

### pd.notna

**Syntax:**
```python
pd.notna(obj)
```

**Explanation:** Detects non-missing values.

**Examples:**
```python
print(pd.notna(1))
print(pd.notna(np.nan))
```

---

### pd.unique

**Syntax:**
```python
pd.unique(values)
```

**Explanation:** Returns unique values from array-like.

**Examples:**
```python
arr = [1, 2, 2, 3, 3, 3]
print(pd.unique(arr))
```

---

### pd.isin

**Syntax:**
```python
pd.isin(values, values_set)
```

**Explanation:** Returns boolean array if each element is in values_set.

**Examples:**
```python
s = pd.Series([1, 2, 3, 4, 5])
result = pd.isin(s, [2, 4])
print(result)
```

---

### df.where

**Syntax:**
```python
DataFrame.where(cond, other=nan, inplace=False, axis=None, level=None, errors='raise', try_cast=False)
```

**Explanation:** Replace values where condition is False.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.where(df > 2, 0)
print(result)
```

---

### df.mask

**Syntax:**
```python
DataFrame.mask(cond, other=nan, inplace=False, axis=None, level=None, errors='raise', try_cast=False)
```

**Explanation:** Replace values where condition is True.

**Examples:**
```python
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

result = df.mask(df > 2, 0)
print(result)
```

---

### df.copy

**Syntax:**
```python
DataFrame.copy(deep=True)
```

**Explanation:** Creates a copy of DataFrame.

**Examples:**
```python
df = pd.DataFrame({'A': [1, 2, 3]})
df_copy = df.copy()
```

---

### pd.pivot

**Syntax:**
```python
pd.pivot(data, index, columns, values)
```

**Explanation:** Reshapes data from long to wide format.

**Examples:**
```python
df = pd.DataFrame({
    'Date': ['2023-01', '2023-01', '2023-02', '2023-02'],
    'Product': ['A', 'B', 'A', 'B'],
    'Sales': [100, 200, 150, 250]
})

result = df.pivot(index='Date', columns='Product', values='Sales')
print(result)
```

---

### pd.melt

**Syntax:**
```python
pd.melt(frame, id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None, ignore_index=True)
```

**Explanation:** Unpivots DataFrame from wide to long format.

**Examples:**
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob'],
    'Age': [25, 30],
    'Salary': [50000, 60000]
})

result = pd.melt(df, id_vars=['Name'], var_name='Attribute', value_name='Value')
print(result)
```

---

### df.add

**Syntax:**
```python
DataFrame.add(other, axis='columns', level=None, fill_value=None)
```

**Explanation:** Addition of DataFrame and other element-wise.

**Examples:**
```python
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

result = df1.add(df2)
print(result)
```

---

### df.sub

**Syntax:**
```python
DataFrame.sub(other, axis='columns', level=None, fill_value=None)
```

**Explanation:** Subtraction of DataFrame and other element-wise.

**Examples:**
```python
df1 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})
df2 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})

result = df1.sub(df2)
print(result)
```

---

### df.mul

**Syntax:**
```python
DataFrame.mul(other, axis='columns', level=None, fill_value=None)
```

**Explanation:** Multiplication of DataFrame and other element-wise.

**Examples:**
```python
df = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})

result = df.mul(2)
print(result)
```

---

### df.div

**Syntax:**
```python
DataFrame.div(other, axis='columns', level=None, fill_value=None)
```

**Explanation:** Division of DataFrame and other element-wise.

**Examples:**
```python
df = pd.DataFrame({'A': [2, 4], 'B': [6, 8]})

result = df.div(2)
print(result)
```

---

### df.mod

**Syntax:**
```python
DataFrame.mod(other, axis='columns', level=None, fill_value=None)
```

**Explanation:** Modulo operation on DataFrame.

**Examples:**
```python
df = pd.DataFrame({'A': [5, 7], 'B': [9, 11]})

result = df.mod(3)
print(result)
```

---

### df.pow

**Syntax:**
```python
DataFrame.pow(other, axis='columns', level=None, fill_value=None)
```

**Explanation:** Exponential power of DataFrame.

**Examples:**
```python
df = pd.DataFrame({'A': [2, 3], 'B': [4, 5]})

result = df.pow(2)
print(result)
```

---

## Additional Resources

For more information, visit the official pandas documentation: https://pandas.pydata.org/docs/

---

*This reference guide covers the most commonly used pandas functions. For a complete list of all functions and parameters, please refer to the official documentation.*

# Pandas_Library
