## What is Pandas?

Pandas is an open-source Python library used for:

* Data Analysis
* Data Cleaning
* Data Manipulation
* Data Engineering
* ETL Pipelines
* Reporting Systems
* Machine Learning Preprocessing
* Automation

Think of Pandas as:

* Excel on steroids
* SQL inside Python
* In-memory database
* Fast data manipulation engine

Official import:

```python
import pandas as pd
```

---

# Why Pandas Was Created?

Before Pandas:

### Problem 1: File Handling

Developers manually read files:

```python
file = open("data.csv")
```

Then split every line manually.

Very slow.

---

### Problem 2: Missing Values

Handling missing data required:

```python
if value == "":
```

everywhere.

Huge code.

---

### Problem 3: Aggregations

Finding:

* Sum
* Average
* Grouping

required loops.

```python
for row in data:
```

Time consuming.

---

### Problem 4: Data Transformation

Tasks like:

* filtering
* sorting
* joins
* cleaning

required hundreds of lines.

---

## Pandas Solution

Pandas provides:

### Fast Operations

```python
df["salary"].sum()
```

instead of loops.

---

### Vectorization

Operations happen on entire columns.

```python
df["salary"] * 2
```

No loop required.

---

### SQL-like Features

```python
merge()
groupby()
join()
```

similar to SQL.

---

### File Reading

Supports:

```python
CSV
Excel
JSON
HTML
Parquet
SQL
```

---

# Under The Hood: Pandas Architecture

Many freshers think:

```python
Pandas != NumPy
```

Wrong.

Actually:

```text
Python
  ↓
NumPy
  ↓
Pandas
```

Pandas is built on top of NumPy.

---

# Internal Structure

Pandas mainly has two objects:

```text
Series
DataFrame
```

---

# Series

One-dimensional labeled array.

Example:

```python
s = pd.Series([10,20,30])
```

Output:

```text
0    10
1    20
2    30
```

Contains:

```text
Index
Values
```

Internally:

```text
Index -> Labels

Values -> NumPy Array
```

Visualization:

```text
Index     Value

0         10
1         20
2         30
```

---

# DataFrame

Two-dimensional tabular structure.

Example:

```python
df = pd.DataFrame({
    "name":["Raj","Aman"],
    "age":[22,25]
})
```

Output:

```text
   name  age
0  Raj   22
1  Aman  25
```

Contains:

```text
Rows
Columns
Index
Data
```

---

# DataFrame Anatomy

A DataFrame has:

### Row Index

```text
0
1
2
3
```

---

### Column Index

```text
name
age
salary
```

---

### Values

Actual data.

```text
Raj
22
50000
```

---

### Dtypes

Data types of columns.

Example:

```python
df.dtypes
```

Output:

```text
name      object
age       int64
salary    int64
```

---

# Hidden Architecture: BlockManager

This is a favorite senior-level interview topic.

Internally Pandas does NOT store:

```text
Row by Row
```

Instead it stores:

```text
Column by Column
```

Example:

```python
df
```

```text
name age salary

Raj  22 50000
Aman 25 60000
```

Internally:

```text
name block
-------------
Raj
Aman

age block
-------------
22
25

salary block
-------------
50000
60000
```

Why?

Because column operations become extremely fast.

---

# Why Column Storage Is Powerful

Suppose:

```python
df["salary"] * 2
```

Pandas accesses one continuous memory block.

Fast.

CPU cache friendly.

---

# NumPy Array Behind Pandas

Example:

```python
s = pd.Series([10,20,30])

s.to_numpy()
```

Output:

```python
array([10,20,30])
```

Pandas ultimately stores values in NumPy arrays.

---

# Memory Layout

Python List:

```python
[10,20,30]
```

Stores:

```text
Pointer
Pointer
Pointer
```

Each integer is separate object.

Memory expensive.

---

NumPy Array:

```python
np.array([10,20,30])
```

Stores:

```text
10 20 30
```

contiguously.

Much faster.

---

# Why Pandas Is Fast

Three reasons:

### 1. NumPy Backend

Uses optimized C code.

---

### 2. Vectorization

Works on entire columns.

---

### 3. BlockManager

Stores similar datatype columns together.

---

# Important Interview Questions

### Q1. Is Pandas built on NumPy?

Yes.

Pandas uses NumPy arrays internally.

---

### Q2. Difference between Series and DataFrame?

Series:

```text
1D
Single Column
```

DataFrame:

```text
2D
Rows + Columns
```

---

### Q3. Why is Pandas faster than Python loops?

Because:

* Vectorization
* NumPy backend
* C implementation
* Continuous memory

---

### Q4. What is BlockManager?

Internal storage engine of Pandas that stores columns grouped by datatype.

---

### Q5. Does Pandas store data row-wise?

No.

Mostly column-wise.

---

### Q6. Why are column operations faster?

Because data is stored contiguously in memory.

---

### Hidden 5+ Years Experience Insight

Bad Pandas:

```python
for row in df.iterrows():
    row["salary"] *= 2
```

Very slow.

Good Pandas:

```python
df["salary"] *= 2
```

Vectorized.

100x–1000x faster on large datasets.

---

# Reading CSV Files in Pandas (Production Level Notes)

CSV Reading is one of the most important Pandas topics because nearly **80% of real-world data pipelines start by reading CSV files**. Almost every Data Engineer, Data Analyst, ML Engineer, and Python Developer uses `read_csv()`. 

---

# 1. Reading CSV Files

## What is CSV?

CSV stands for:

```text
Comma Separated Values
```

Example:

```csv
name,age,city
Raj,22,Delhi
Aman,25,Mumbai
```

CSV is popular because:

* Lightweight
* Human readable
* Universal format
* Easy to transfer
* Fast to process



---

## Basic Syntax

```python
import pandas as pd

df = pd.read_csv("students.csv")
```

Output:

```text
   name  age    city
0   Raj   22   Delhi
1  Aman   25  Mumbai
```

Notice:

```text
0
1
```

are indexes automatically created by Pandas.

---

# Internal Working of read_csv()

When you execute:

```python
df = pd.read_csv("data.csv")
```

Internally Pandas performs:

```text
Open File
    ↓
Read Bytes
    ↓
Decode Text
    ↓
Split Rows
    ↓
Split Columns
    ↓
Infer Datatypes
    ↓
Create NumPy Arrays
    ↓
Create DataFrame
```

This is exactly why CSV reading becomes expensive for huge datasets. 

---

# Important read_csv() Parameters

Interviewers frequently ask:

```text
What parameters of read_csv() have you used?
```

Expected answer:

```text
sep
header
names
usecols
nrows
skiprows
dtype
parse_dates
encoding
```

---

# 2. sep Parameter

## What is sep?

`sep` means:

```text
Separator
Delimiter
```

Default:

```python
sep=","
```

---

## Example 1

File:

```text
Raj|22|Delhi
Aman|25|Mumbai
```

Separator:

```text
|
```

Read:

```python
df = pd.read_csv(
    "data.txt",
    sep="|"
)
```

Output:

```text
   Name  Age   City
0  Raj   22   Delhi
1 Aman   25  Mumbai
```



---

## What Happens If sep Is Not Provided?

Pandas assumes:

```python
sep=","
```

Result:

```text
Raj|22|Delhi
```

becomes one column.

Output:

```text
      Raj|22|Delhi
0   Aman|25|Mumbai
```

Wrong parsing.

---

## Common Separators

### Comma

```python
sep=","
```

CSV files.

---

### Pipe

```python
sep="|"
```

Enterprise exports.

---

### Semicolon

```python
sep=";"
```

European datasets.

---

### Tab

```python
sep="\t"
```

TSV files.

---

# Senior-Level Insight

Always validate raw files.

Bad File:

```text
Raj|22|Delhi
Aman,25,Mumbai
```

Mixed separators.

Produces:

```text
ParserError
```

Production ETL jobs often fail because of this.

---

# Interview Questions

### Why use sep?

To tell Pandas how columns are separated.

---

### What is default separator?

```python
","
```

---

### What is TSV?

Tab Separated Values.

```python
sep="\t"
```

---

# 3. header Parameter

One of the most misunderstood parameters.

---

## What is header?

Header tells Pandas:

```text
Which row contains column names?
```

---

## Example

CSV:

```csv
name,age,city
Raj,22,Delhi
Aman,25,Mumbai
```

Read:

```python
df = pd.read_csv("data.csv")
```

Pandas automatically assumes:

```python
header=0
```

Meaning:

```text
First row contains column names
```



---

# header=0

Default behavior.

```python
df = pd.read_csv(
    "data.csv",
    header=0
)
```

Output:

```text
name age city
Raj 22 Delhi
```

---

# header=None

Used when file has NO column names.

CSV:

```text
Raj,22,Delhi
Aman,25,Mumbai
```

Read:

```python
df = pd.read_csv(
    "data.csv",
    header=None
)
```

Output:

```text
      0   1       2
0   Raj 22   Delhi
1 Aman 25  Mumbai
```

Pandas creates numeric columns.

---

# header=1

Means:

```text
Second row contains headers
```

Example:

```text
Report Generated 2026
name,age,city
Raj,22,Delhi
```

```python
df = pd.read_csv(
    "data.csv",
    header=1
)
```

Output:

```text
name age city
Raj 22 Delhi
```

---

# Hidden Production Problem

File:

```text
name,name,city
Raj,22,Delhi
```

Duplicate headers.

Pandas loads:

```text
name
name.1
city
```

to avoid conflicts.

---

# Interview Questions

### Default value of header?

```python
header=0
```

---

### header=None use case?

When file contains no column names.

---

### Difference between header=0 and header=None?

header=0:

```text
first row becomes columns
```

header=None:

```text
first row becomes data
```

---

# 4. names Parameter

Used to create custom column names.

---

## Why names Exists?

Real-world datasets may have:

* Missing headers
* Duplicate headers
* Broken exports
* Random names

Instead of fixing files manually:

```python
names=["Name","Age","City"]
```

---

## Example

CSV:

```text
Raj,22,Delhi
Aman,25,Mumbai
```

Read:

```python
df = pd.read_csv(
    "data.csv",
    names=["Name","Age","City"]
)
```

Output:

```text
   Name Age City
0 Raj 22 Delhi
1 Aman 25 Mumbai
```



---

# Important Hidden Rule

If file already contains headers:

```csv
name,age,city
Raj,22,Delhi
```

and you use:

```python
names=["Name","Age","City"]
```

Then Pandas treats original header as DATA.

Output:

```text
Name Age City
name age city
Raj 22 Delhi
```

Unexpected behavior.

---

## Correct Solution

```python
df = pd.read_csv(
    "data.csv",
    header=0,
    names=["Name","Age","City"]
)
```

Now original header is replaced.

---

# Production Best Practices

### Good

```python
STANDARD_COLUMNS = [
    "emp_id",
    "emp_name",
    "salary"
]
```

Use standard naming.

---

### Avoid

```python
names=["A","B","C"]
```

Not descriptive.

---

# Interview Questions

### Why use names?

To assign custom column names while reading data.

---

### Can names replace existing headers?

Yes.

When used with:

```python
header=0
```

---

### What happens if names length mismatches?

Pandas raises errors or creates unexpected structure depending on file.

---

# Quick Revision Table

| Parameter | Purpose             |
| --------- | ------------------- |
| sep       | Column separator    |
| header    | Header row position |
| names     | Custom column names |

---

# Hidden Senior Developer Tip

Many ETL failures happen because developers assume:

```python
pd.read_csv(file)
```

works for every file.

In reality, before loading a file, experienced engineers always verify:

```text
Separator
Encoding
Header Row
Bad Rows
Missing Values
Date Columns
Column Count
```

This validation step prevents most production pipeline failures.

---
# usecols, nrows, skiprows, dtype, parse_dates, encoding

These parameters are heavily used in real-world ETL pipelines because they help improve:

* Performance
* Memory Usage
* Data Quality
* Processing Speed
* Production Stability

Most senior engineers optimize `read_csv()` using these parameters. 

---

# 5. usecols Parameter

## What is usecols?

`usecols` allows Pandas to read only selected columns from a file.

Instead of reading the entire file:

```python
df = pd.read_csv("data.csv")
```

you can read only required columns.

---

## Example

CSV:

```text
Name,Age,Salary,City,Country
Raj,22,50000,Delhi,India
```

Read:

```python
df = pd.read_csv(
    "data.csv",
    usecols=["Name","Age"]
)
```

Output:

```text
Name  Age
Raj   22
```

Only required columns are loaded.

---

# Why usecols Is Important?

Imagine:

```text
1000 Columns
20 Million Rows
```

But your analysis needs:

```text
Name
Salary
Department
```

only.

Without usecols:

```text
Read all 1000 columns
```

Huge RAM waste.

With usecols:

```text
Read only 3 columns
```

Massive optimization.



---

# Internal Working

Without usecols:

```text
CSV
 ↓
Read all columns
 ↓
Create DataFrame
```

With usecols:

```text
CSV
 ↓
Ignore unnecessary columns
 ↓
Read selected columns
 ↓
Create smaller DataFrame
```

---

# Production Example

Bad:

```python
df = pd.read_csv("sales.csv")
```

Good:

```python
df = pd.read_csv(
    "sales.csv",
    usecols=[
        "customer_id",
        "amount"
    ]
)
```

---

# Interview Question

### Why use usecols?

To reduce:

* RAM usage
* Disk I/O
* CPU usage

while improving loading speed.

---

# 6. nrows Parameter

## What is nrows?

Reads only a limited number of rows.

---

## Example

```python
df = pd.read_csv(
    "data.csv",
    nrows=100
)
```

Meaning:

```text
Read first 100 rows
Stop
```



---

# Internal Workflow

Suppose file has:

```text
100000 rows
```

and:

```python
nrows=100
```

Pandas performs:

```text
Open File
 ↓
Read Row 1–100
 ↓
Stop Reading
 ↓
Create DataFrame
```

---

# Why nrows Is Useful?

### Debugging

Instead of loading:

```text
20 GB file
```

load:

```python
nrows=50
```

to inspect structure.

---

### Schema Validation

Check:

* Column Names
* Datatypes
* Missing Values

before reading entire dataset.

---

# Production Example

```python
sample = pd.read_csv(
    "transactions.csv",
    nrows=1000
)
```

Used during development.

---

# Interview Question

### Does nrows improve performance?

Yes.

Because Pandas stops reading early.

---

# 7. skiprows Parameter

## What is skiprows?

Skips unwanted rows while reading data.

Common in:

* ERP Reports
* Government Data
* Excel Exports
* Legacy Systems



---

# Example

File:

```text
Company Report 2026
Generated on 24-May-2026

Name,Age,City
Raj,22,Delhi
Aman,25,Mumbai
```

Problem:

Pandas may treat:

```text
Company Report 2026
```

as header.

---

Solution:

```python
df = pd.read_csv(
    "data.csv",
    skiprows=2
)
```

Output:

```text
Name Age City
Raj 22 Delhi
Aman 25 Mumbai
```

---

# Different Ways

## Skip First N Rows

```python
skiprows=5
```

---

## Skip Specific Rows

```python
skiprows=[1,4,7]
```

---

Example:

```text
Row 1 skipped
Row 4 skipped
Row 7 skipped
```

---

# Production Use Cases

### Audit Reports

```text
Generated By SAP
Generated Date
Version
Actual Data
```

Skip metadata rows.

---

### Government Data

Many files begin with:

```text
Notes
Disclaimer
Instructions
```

before actual table.

---

# Interview Question

### Difference between skiprows and nrows?

skiprows:

```text
Ignore rows
```

nrows:

```text
Limit rows read
```

---

# 8. dtype Parameter

One of the most important optimization techniques.

---

## What is dtype?

Datatype controls:

* Memory
* Performance
* Storage
* Allowed Operations



---

# Example

CSV:

```text
age,salary
22,50000
25,60000
```

Read:

```python
df = pd.read_csv(
    "data.csv",
    dtype={
        "age":"int32",
        "salary":"float32"
    }
)
```

---

# Why dtype Matters?

Without dtype:

Pandas guesses datatypes.

```text
Guessing = Risk
```

---

# Hidden Production Problem

Suppose:

```text
Customer_ID
```

contains:

```text
001
002
003
```

Pandas may infer:

```python
int64
```

Output:

```text
1
2
3
```

Leading zeros lost.

---

Solution:

```python
dtype={
    "Customer_ID":"str"
}
```

Output:

```text
001
002
003
```

---

# Memory Optimization Example

Default:

```python
int64
```

Memory:

```text
8 bytes
```

Use:

```python
int32
```

Memory:

```text
4 bytes
```

50% reduction.

---

# Category Datatype (Very Important)

Suppose:

```text
Delhi
Mumbai
Delhi
Delhi
Mumbai
```

Repeated values.

Instead of:

```python
object
```

Use:

```python
category
```

```python
df["city"] = (
    df["city"]
    .astype("category")
)
```

Benefits:

* Less Memory
* Faster Groupby
* Faster Filtering



---

# Interview Questions

### Why specify dtype manually?

To:

* Save memory
* Prevent wrong inference
* Improve performance

---

### When should category be used?

Low-cardinality repeated values:

```text
Gender
Country
State
Department
```

---

### Why object dtype is slow?

Because strings are Python objects.

More memory + slower operations.

---

# 9. parse_dates Parameter

One of the most important real-world parameters.

---

## What is parse_dates?

Automatically converts strings into datetime objects.

Without parse_dates:

```python
created_at
```

becomes:

```python
object
```

With parse_dates:

```python
datetime64[ns]
```



---

# Example

CSV:

```text
id,name,created_at
1,Raj,2025-01-10
2,Aman,2025-03-15
```

Read:

```python
df = pd.read_csv(
    "data.csv",
    parse_dates=["created_at"]
)
```

---

Check:

```python
df.dtypes
```

Output:

```text
id                     int64
name                  object
created_at    datetime64[ns]
```

---

# Why Is It Important?

Without datetime conversion:

Cannot efficiently do:

```python
Sorting
Filtering
Grouping
Rolling
Resampling
Trend Analysis
```

---

# Common Operations

### Extract Year

```python
df["created_at"].dt.year
```

---

### Extract Month

```python
df["created_at"].dt.month
```

---

### Day Name

```python
df["created_at"].dt.day_name()
```

---

### Date Difference

```python
df["end"] - df["start"]
```

returns:

```text
timedelta
```

---

# Hidden Production Problem

Date:

```text
01/02/2025
```

Could mean:

```text
1 Feb 2025
```

or

```text
2 Jan 2025
```

Ambiguous formats cause bugs.

---

# Interview Question

### Why use parse_dates during reading?

Because conversion happens once during loading and is more efficient than converting later.

---

# 10. encoding Parameter

Very important when handling international files.



---

# What is Encoding?

Computers store:

```text
Bytes
```

not characters.

Encoding defines:

```text
Character → Byte Mapping
```

---

# Common Encodings

### UTF-8

Most common.

```python
encoding="utf-8"
```

Supports:

* English
* Hindi
* Chinese
* Japanese
* Arabic

---

### Latin-1

Common in old systems.

```python
encoding="latin1"
```

---

### cp1252

Microsoft Windows exports.

```python
encoding="cp1252"
```

---

# Production Error

```python
UnicodeDecodeError
```

Example:

```python
df = pd.read_csv(
    "data.csv"
)
```

Error:

```text
UnicodeDecodeError
```

---

# Solution

Try:

```python
df = pd.read_csv(
    "data.csv",
    encoding="latin1"
)
```

or

```python
encoding="cp1252"
```

---

# Interview Question

### What causes UnicodeDecodeError?

Wrong file encoding.

---

### Most common encoding?

```python
UTF-8
```

---

# Quick Revision Table

| Parameter   | Purpose                     |
| ----------- | --------------------------- |
| usecols     | Read selected columns       |
| nrows       | Read limited rows           |
| skiprows    | Skip unwanted rows          |
| dtype       | Set datatypes manually      |
| parse_dates | Convert dates automatically |
| encoding    | Handle text encoding        |

---

# Hidden Senior Engineer Tips

Always optimize CSV loading using:

```python
pd.read_csv(
    file,
    usecols=[...],
    dtype={...},
    parse_dates=[...],
    encoding="utf-8"
)
```

For large files this can reduce:

```text
Memory Usage: 60-90%
Loading Time: 30-70%
```

and prevent many production issues.
```

# Handling Bad Rows, Logging Errors, Chunk Processing & Huge CSV Files

This section covers topics that are extremely important in real-world ETL pipelines, Data Engineering projects, and large-scale production systems. Most beginners know `read_csv()`, but experienced engineers know how to handle corrupted files, huge datasets, and memory issues. 

---

# 11. What Are Bad Rows?

A **Bad Row** (Malformed Record) is a row that does not follow the expected file structure.

Example:

Expected CSV:

```csv
Name,Age,City
Raj,22,Delhi
Aman,25,Mumbai
```

All rows contain:

```text
3 columns
```

---

## Bad CSV Example

```csv
Name,Age,City
Raj,22,Delhi
Aman,25,Mumbai,India
```

Problem:

```text
Row 1 → 3 Columns
Row 2 → 4 Columns
```

Structure broken.

This row becomes:

```text
Bad Row
Malformed Record
Corrupted Record
```



---

# Why Bad Rows Occur?

In production data comes from:

* Users
* Excel exports
* Third-party vendors
* APIs
* Legacy systems
* Automated scripts

Any of these sources can generate invalid records. 

---

# Common Causes of Bad Rows

## 1. Extra Delimiters

Expected:

```csv
Raj,22,Delhi
```

Actual:

```csv
Raj,22,Delhi,India
```

Result:

```text
Expected 3 fields
Found 4
```

---

## 2. Missing Values

Expected:

```csv
Raj,22,Delhi
```

Actual:

```csv
Raj,,Delhi
```

or

```csv
Raj,22
```

---

## 3. Broken Quotes

Expected:

```csv
Raj,"Software Engineer",Delhi
```

Actual:

```csv
Raj,"Software Engineer,Delhi
```

Missing closing quote.

Parser becomes confused.

---

## 4. Human Editing

Users manually edit CSV in:

```text
Excel
Notepad
Google Sheets
```

and accidentally break structure.

---

## 5. Corrupted Exports

ERP systems sometimes insert:

```text
Special Characters
Unexpected Symbols
Control Characters
```

creating malformed rows.

---

# Error Produced by Pandas

Common error:

```python
ParserError
```

Example:

```text
ParserError:
Expected 3 fields in line 4,
saw 4
```

This is one of the most common Pandas production errors.

---

# Handling Bad Rows

## Option 1: Raise Error (Default)

```python
df = pd.read_csv(
    "data.csv"
)
```

Behavior:

```text
Bad Row Found
↓
Stop Execution
↓
Raise Error
```

Good when data quality is critical.

---

## Option 2: Skip Bad Rows

```python
df = pd.read_csv(
    "data.csv",
    on_bad_lines="skip"
)
```

Behavior:

```text
Bad Row
↓
Ignore Row
↓
Continue Processing
```

Useful for huge datasets.



---

## Option 3: Warn

```python
df = pd.read_csv(
    "data.csv",
    on_bad_lines="warn"
)
```

Behavior:

```text
Show Warning
Continue Reading
```

Good during testing.

---

# Custom Handling

Advanced approach:

```python
def handle_bad_row(row):
    print(row)

df = pd.read_csv(
    "data.csv",
    on_bad_lines="skip"
)
```

Used when you need custom validation.

---

# Logging Bad Rows

Senior engineers rarely just skip rows.

They log them.

Example:

```python
import logging

logging.basicConfig(
    filename="bad_rows.log",
    level=logging.ERROR
)
```

When bad row found:

```python
logging.error(
    f"Invalid row: {row}"
)
```

Benefits:

* Easier debugging
* Audit trail
* Compliance requirements



---

# Interview Questions

### What is a bad row?

A row whose structure differs from expected schema.

---

### How do you skip bad rows?

```python
on_bad_lines="skip"
```

---

### How do you show warnings?

```python
on_bad_lines="warn"
```

---

### Why log bad rows?

To maintain auditability and troubleshoot data quality issues.

---

# 12. Reading Huge CSV Files

This is where many beginners crash systems.

---

## Problem

File:

```text
5 GB CSV
```

Code:

```python
df = pd.read_csv(
    "large_file.csv"
)
```

What happens?

```text
Read Entire File
↓
RAM Full
↓
System Slow
↓
MemoryError
```



---

# What Is Chunk Processing?

Instead of loading:

```text
Entire File
```

load:

```text
Small Pieces
```

called:

```text
Chunks
```

---

# Basic Syntax

```python
chunk = pd.read_csv(
    "large.csv",
    chunksize=10000
)
```

Meaning:

```text
Read 10,000 rows
at a time
```

---

# Internal Working

```text
CSV File
     ↓
Read 10,000 Rows
     ↓
Create DataFrame
     ↓
Process
     ↓
Remove From Memory
     ↓
Read Next 10,000
```

Memory remains stable.



---

# Using Chunks

```python
chunks = pd.read_csv(
    "large.csv",
    chunksize=10000
)

for chunk in chunks:
    print(chunk.head())
```

---

# What Does Pandas Return?

Without chunksize:

```python
type(df)
```

Output:

```python
DataFrame
```

---

With chunksize:

```python
type(chunks)
```

Output:

```python
TextFileReader
```

Iterator object.

---

# Real Production Example

## Counting Rows

```python
total_rows = 0

for chunk in pd.read_csv(
    "large.csv",
    chunksize=10000
):
    total_rows += len(chunk)

print(total_rows)
```

---

## Sum of Column

```python
total_sales = 0

for chunk in pd.read_csv(
    "sales.csv",
    chunksize=50000
):
    total_sales += chunk[
        "sales"
    ].sum()
```

Memory efficient.

---

# Filtering Large Files

Instead of:

```python
df = pd.read_csv(
    "large.csv"
)
```

Use:

```python
result = []

for chunk in pd.read_csv(
    "large.csv",
    chunksize=50000
):
    filtered = chunk[
        chunk["city"]=="Delhi"
    ]
    result.append(filtered)

final_df = pd.concat(result)
```

Works on files much larger than RAM.

---

# Hidden Production Problems

## Problem 1: Duplicate Detection

Suppose:

```text
Customer 101
```

appears in:

```text
Chunk 1
Chunk 10
```

Simple chunk processing may miss duplicates.

---

## Problem 2: GroupBy Issues

Grouping across chunks:

```python
groupby()
```

can produce incomplete results if not aggregated correctly.

---

## Problem 3: Sorting

Global sorting impossible unless all chunks combined.

---

## Problem 4: Memory Fragmentation

Too many chunk objects can increase memory usage.

Always delete unnecessary chunks.

---

# Performance Optimization

Best Practice:

```python
pd.read_csv(
    file,
    chunksize=50000,
    usecols=[...],
    dtype={...}
)
```

Combining:

* chunksize
* usecols
* dtype

gives massive performance improvement.

---

# Chunk Processing vs Full Load

| Feature     | Full Load          | Chunk Processing |
| ----------- | ------------------ | ---------------- |
| Memory      | High               | Low              |
| Speed       | Fast (small files) | Good             |
| Large Files | Risky              | Excellent        |
| Scalability | Poor               | Excellent        |
| Production  | Limited            | Preferred        |

---

# Real-World Use Cases

### Log Processing

```text
Application Logs
Server Logs
Security Logs
```

---

### Financial Data

```text
Transactions
Payments
Invoices
```

---

### Telemetry Data

```text
IoT Devices
Sensor Data
Machine Data
```

---

### User Activity Tracking

```text
Clicks
Page Views
Events
```

---

# Hidden Senior Engineer Tips

For files:

```text
< 100 MB
```

Normal reading is fine.

---

For files:

```text
100 MB – 1 GB
```

Consider:

```python
usecols
dtype
parse_dates
```

---

For files:

```text
> 1 GB
```

Use:

```python
chunksize
```

almost always.

---

# Interview Questions

### What is chunk processing?

Reading large files in smaller pieces instead of loading everything into memory.

---

### What does chunksize return?

```python
TextFileReader
```

Iterator object.

---

### Why use chunk processing?

To avoid:

```text
MemoryError
RAM exhaustion
System crashes
```

---

### Can chunk processing replace Spark?

No.

Chunk processing helps a single machine.

Spark is distributed computing across multiple machines.

---

## Quick Revision

```python
# Skip bad rows
pd.read_csv(
    file,
    on_bad_lines="skip"
)

# Warn
pd.read_csv(
    file,
    on_bad_lines="warn"
)

# Chunk processing
pd.read_csv(
    file,
    chunksize=10000
)
```
# Reading Excel Files in Pandas 

Excel files are everywhere in industry:

* HR Reports
* Finance Reports
* Banking Data
* ERP Exports
* SAP Reports
* Government Data
* Business Dashboards

A large percentage of business data still arrives as `.xlsx` files. Pandas provides `read_excel()` to work with Excel efficiently. 

---

# Why Excel Needs a Special Engine?

CSV is plain text.

Excel is a binary structured format.

Pandas cannot read Excel directly.

It needs an engine.

Most common:

```bash
pip install openpyxl
```



---

# Basic Excel Reading

## Syntax

```python
import pandas as pd

df = pd.read_excel(
    "employees.xlsx"
)
```

Output:

```text
   Name    Department Salary
0  Raj     IT         50000
1  Aman    HR         40000
```

---

# Internal Working of read_excel()

When you execute:

```python
df = pd.read_excel(
    "employees.xlsx"
)
```

Pandas performs:

```text
Open Excel File
      ↓
openpyxl Engine
      ↓
Read Workbook
      ↓
Read Sheet
      ↓
Parse Cells
      ↓
Infer Datatypes
      ↓
Create DataFrame
```

Unlike CSV:

```text
CSV → Line Parsing

Excel → Workbook Parsing
```

which makes Excel slower.



---

# Reading Specific Sheet

Most Excel files contain:

```text
Sheet1
Sheet2
Sheet3
```

Example:

```python
df = pd.read_excel(
    "employees.xlsx",
    sheet_name="Sales"
)
```

Output:

```text
Only Sales sheet loaded
```



---

# Why Use sheet_name?

Suppose workbook contains:

```text
Sales
Finance
HR
Operations
```

Loading everything wastes memory.

Read only required sheet.

---

# Reading Multiple Sheets

## Read All Sheets

```python
all_sheets = pd.read_excel(
    "employees.xlsx",
    sheet_name=None
)
```

Output:

```python
{
 "Sales": DataFrame,
 "HR": DataFrame,
 "Finance": DataFrame
}
```

Important:

Returns:

```python
dict
```

not DataFrame.



---

# Reading Sheet By Index

Instead of name:

```python
df = pd.read_excel(
    "employees.xlsx",
    sheet_name=0
)
```

Meaning:

```text
First Sheet
```

---

```python
sheet_name=1
```

Meaning:

```text
Second Sheet
```

---

# Interview Question

### Difference between sheet_name="Sales" and sheet_name=0 ?

```text
"Sales" → Sheet name

0 → Sheet position
```

---

# Reading Selected Columns

Exactly like CSV.

---

## Example

Excel:

```text
Name Age Salary City
```

Read only:

```python
df = pd.read_excel(
    "employees.xlsx",
    usecols=["Name","Salary"]
)
```

Output:

```text
Name Salary
Raj 50000
Aman 60000
```



---

# Alternative Column Selection

Excel Columns:

```text
A B C D E
```

Read:

```python
df = pd.read_excel(
    "employees.xlsx",
    usecols="A:C"
)
```

Output:

```text
Only A,B,C columns
```

---

# Why use usecols?

Suppose:

```text
500 columns
```

Need only:

```text
3 columns
```

Use:

```python
usecols=
```

to save memory.

---

# Reading Specific Rows

## nrows

```python
df = pd.read_excel(
    "employees.xlsx",
    nrows=10
)
```

Reads:

```text
First 10 rows
```

Only.

---

# skiprows

Excel often contains:

```text
Company Name
Generated Date
Prepared By
```

before actual table.

---

Example:

```python
df = pd.read_excel(
    "report.xlsx",
    skiprows=3
)
```

Meaning:

```text
Ignore first 3 rows
```



---

# Custom Column Names

Suppose Excel file contains:

```text
Emp Name
Age
Dept
```

You want:

```text
employee_name
employee_age
department
```

Use:

```python
df = pd.read_excel(
    "file.xlsx",
    names=[
      "employee_name",
      "employee_age",
      "department"
    ]
)
```

---

# Setting Index Column

## Default

Pandas creates:

```text
0
1
2
3
```

as index.

---

## Custom Index

```python
df = pd.read_excel(
    "employees.xlsx",
    index_col="EmployeeID"
)
```

Output:

```text
EmployeeID becomes index
```



---

# Reading Date Columns

Very common interview topic.

Excel internally stores dates as:

```text
Serial Numbers
```

Example:

```text
45678
```

represents a date.

Pandas automatically converts them.

---

Example:

```python
df = pd.read_excel(
    "sales.xlsx",
    parse_dates=["JoiningDate"]
)
```

Output:

```text
datetime64[ns]
```



---

# Reading With dtype

Production optimization.

```python
df = pd.read_excel(
    "employees.xlsx",
    dtype={
        "Age":"int32",
        "Salary":"float32"
    }
)
```

Benefits:

* Lower Memory
* Faster Processing
* Better Consistency

---

# Handling Missing Values

Excel:

```text
Name   Salary
Raj    50000
Aman
```

Output:

```text
NaN
```

automatically generated.

---

Check:

```python
df.isnull().sum()
```

Fill:

```python
df.fillna(0)
```



---

# Formula Handling (Very Important)

Consider Excel:

```excel
A1 = 10
A2 = 20

A3 = A1+A2
```

Displayed:

```text
30
```

---

## What Pandas Reads

Most people think:

```text
Pandas reads formulas
```

Wrong.

Pandas usually reads:

```text
Calculated Value
```

not formula.

Example:

```python
df = pd.read_excel(
    "sales.xlsx"
)
```

Output:

```text
30
```

instead of:

```excel
=A1+A2
```



---

# Why?

Excel stores:

```text
Formula
+
Cached Result
```

Pandas generally reads:

```text
Cached Result
```

---

# Hidden Production Problem

Suppose:

```excel
=A1+A2
```

exists.

Workbook was never recalculated.

Cached value:

```text
100
```

Actual value:

```text
110
```

Pandas reads:

```text
100
```

Old value.

Very common finance bug.

---

# Enterprise Excel Problems

## 1. Hidden Sheets

Workbook:

```text
Sheet1
Sheet2
Secret_Data
```

Some sheets are hidden.

Pandas can still access them if specified.

---

## 2. Merged Cells

Excel:

```text
Region
     North
     South
```

Merged cells can create:

```text
NaN
```

issues.



---

## 3. Duplicate Columns

Excel:

```text
Salary Salary Salary
```

Pandas automatically converts:

```text
Salary
Salary.1
Salary.2
```

---

## 4. Mixed Datatypes

Column:

```text
1000
2000
N/A
3000
```

Pandas may infer:

```python
object
```

instead of:

```python
int64
```

leading to performance problems.

---

# Performance Problems With Excel

Excel:

```text
100 MB
```

is much slower than:

```text
100 MB CSV
```

Why?

Because:

```text
Workbook Parsing
Cell Parsing
Sheet Metadata
Formatting
```

must be processed.

---

# Production Best Practices

### Always Read Required Sheet

```python
sheet_name="Sales"
```

---

### Read Required Columns

```python
usecols=
```

---

### Specify Datatypes

```python
dtype=
```

---

### Parse Dates During Reading

```python
parse_dates=
```

---

### Convert Excel To CSV For Huge Files

Many Data Engineers first do:

```text
Excel
 ↓
CSV
 ↓
Pandas
```

because CSV processing is much faster.

---

# Frequently Asked Interview Questions

### Which package is required for read_excel()?

```text
openpyxl
```

---

### What does sheet_name=None return?

```python
Dictionary of DataFrames
```

---

### Can Pandas read formulas?

Usually it reads cached values, not formulas.

---

### Why is Excel slower than CSV?

Because Excel requires workbook parsing and metadata processing.

---

### How to read only the HR sheet?

```python
pd.read_excel(
    "file.xlsx",
    sheet_name="HR"
)
```

---

### How to read columns A to D?

```python
pd.read_excel(
    "file.xlsx",
    usecols="A:D"
)
```

---

# Quick Revision

```python
# Basic
pd.read_excel("file.xlsx")

# Specific sheet
pd.read_excel(
    "file.xlsx",
    sheet_name="Sales"
)

# All sheets
pd.read_excel(
    "file.xlsx",
    sheet_name=None
)

# Specific columns
pd.read_excel(
    "file.xlsx",
    usecols="A:C"
)

# Skip rows
pd.read_excel(
    "file.xlsx",
    skiprows=3
)

# Parse dates
pd.read_excel(
    "file.xlsx",
    parse_dates=["date"]
)
```
# Reading HTML Tables in Pandas (`read_html()`)

One of the most underrated features in Pandas is **`read_html()`**.

Many websites publish data inside HTML tables:

* Stock Market Data
* Sports Statistics
* Government Reports
* Election Results
* Financial Statements
* Economic Indicators

Instead of manually copying data into Excel, Pandas can directly convert HTML tables into DataFrames. 

---

# What is HTML?

HTML stands for:

```text
HyperText Markup Language
```

Web pages are built using HTML.

Example:

```html
<table>
<tr>
<th>Name</th>
<th>Age</th>
</tr>

<tr>
<td>Raj</td>
<td>22</td>
</tr>
</table>
```

This is a simple HTML table.

---

# Why read_html() Exists?

Before Pandas:

Developers had to:

```text
Open Website
↓
Copy Table
↓
Paste Into Excel
↓
Save CSV
↓
Read CSV
```

Very slow.

---

Pandas provides:

```python
pd.read_html()
```

which performs:

```text
Website
↓
Extract Table
↓
Convert To DataFrame
```

Automatically.



---

# Installation Requirement

Usually required:

```bash
pip install lxml
```

Sometimes:

```bash
pip install html5lib
```

These libraries help Pandas parse HTML.

---

# Basic Syntax

## Read Table From URL

```python
import pandas as pd

tables = pd.read_html(
    "https://example.com"
)
```

---

# Important Observation

Most beginners expect:

```python
DataFrame
```

Wrong.

Output:

```python
list
```

because a webpage can contain multiple tables.

---

Example:

```python
type(tables)
```

Output:

```python
list
```

---

# Access First Table

```python
df = tables[0]
```

Now:

```python
type(df)
```

Output:

```python
DataFrame
```

---

# Internal Working

When Pandas executes:

```python
pd.read_html(url)
```

It performs:

```text
Download HTML
        ↓
Parse HTML Tags
        ↓
Locate <table>
        ↓
Extract Rows
        ↓
Extract Columns
        ↓
Create DataFrames
        ↓
Return List
```



---

# Example

Suppose webpage contains:

```html
<table>

Name   Age
Raj    22
Aman   25

</table>
```

Read:

```python
tables = pd.read_html(url)
```

Output:

```python
[
  DataFrame
]
```

Access:

```python
df = tables[0]
```

Result:

```text
Name  Age
Raj   22
Aman  25
```

---

# Reading Local HTML File

Instead of URL:

```python
pd.read_html(
    "report.html"
)
```

Works with local files.

---

# Multiple Tables

Most websites contain many tables.

Example:

```text
Table 1 → Sales
Table 2 → Employees
Table 3 → Finance
```

Read:

```python
tables = pd.read_html(url)
```

Output:

```python
[
 df1,
 df2,
 df3
]
```

---

# Loop Through Tables

```python
tables = pd.read_html(url)

for i, table in enumerate(tables):
    print(i)
    print(table.head())
```

Useful for discovering table positions.

---

# Selecting Specific Table

```python
sales_df = tables[2]
```

Reads third table.

---

# Reading Specific Table Using match

One of the most important interview topics.

Suppose webpage contains:

```text
Sales Report
Employee Report
Finance Report
```

You only need:

```text
Employee Report
```

Use:

```python
pd.read_html(
    url,
    match="Employee"
)
```

Pandas searches table text.

---

# Why match Is Important?

Without:

```python
pd.read_html(url)
```

All tables loaded.

---

With:

```python
match=
```

Only required table extracted.

Faster.

Less memory.

---

# Reading Specific Attributes

HTML:

```html
<table id="sales">
```

You can use:

```python
attrs={"id":"sales"}
```

Example:

```python
pd.read_html(
    url,
    attrs={
      "id":"sales"
    }
)
```

Useful in production scraping.

---

# Handling Headers

Suppose HTML:

```html
<table>

<tr>
<th>Name</th>
<th>Age</th>
</tr>

</table>
```

Pandas automatically creates:

```text
Name
Age
```

columns.

---

If headers are on second row:

```python
pd.read_html(
    url,
    header=1
)
```

Same concept as CSV.

---

# Handling Missing Values

HTML table:

```text
Name   Salary
Raj    50000
Aman
```

Output:

```text
NaN
```

automatically generated.

Check:

```python
df.isnull().sum()
```

---

# Converting Datatypes

HTML often loads numbers as:

```python
object
```

Convert:

```python
df["salary"] = (
    df["salary"]
    .astype("float")
)
```

Production best practice.

---

# Real-World Example

## Stock Market Tables

Many financial websites display:

```text
Company
Price
Market Cap
P/E Ratio
```

inside HTML tables.

Use:

```python
pd.read_html(url)
```

instead of manual copying.

---

## Government Reports

Examples:

```text
Population Data
GDP Reports
Election Results
```

Often available as tables.

---

## Sports Statistics

Examples:

```text
IPL
Cricket
Football
Olympics
```

Statistics frequently exist in HTML tables.

---

# Production Problems

## Problem 1: JavaScript Tables

Most modern websites load tables dynamically.

HTML initially:

```html
<div id="table"></div>
```

Actual table loaded later using JavaScript.

---

Pandas sees:

```text
No Table Found
```

because JavaScript hasn't executed.

---

Error:

```python
ValueError:
No tables found
```

---

# Why?

Pandas downloads:

```text
Raw HTML
```

not rendered browser content.

---

# Solution

Use:

```text
Selenium
Playwright
BeautifulSoup
```

before Pandas.

---

# Problem 2: Login Required

Website:

```text
Login Required
```

Pandas cannot automatically authenticate.

Need session handling.

---

# Problem 3: Anti-Bot Protection

Many websites block scraping.

Examples:

```text
Cloudflare
Rate Limiting
Captcha
```

---

# Problem 4: Nested Tables

HTML:

```html
<table>

<table>

</table>

</table>
```

Nested tables confuse parsing.

---

# Problem 5: Broken HTML

Many websites contain:

```html
Missing Tags
Broken Structure
Invalid HTML
```

Parser may fail.

---

# Performance Considerations

Good:

```python
pd.read_html(
    url,
    match="Sales"
)
```

---

Bad:

```python
pd.read_html(
    huge_page
)
```

extracting 100 tables.

---

# read_html() vs BeautifulSoup

| Feature               | read_html | BeautifulSoup |
| --------------------- | --------- | ------------- |
| Easy                  | ✅         | ❌             |
| Direct DataFrame      | ✅         | ❌             |
| Flexible              | ❌         | ✅             |
| Complex Parsing       | ❌         | ✅             |
| Fast Table Extraction | ✅         | ❌             |

---

# Internal Return Type

Very common interview question.

```python
tables = pd.read_html(url)
```

Returns:

```python
list[DataFrame]
```

NOT:

```python
DataFrame
```

---

# Interview Questions

### What does read_html() return?

```python
list of DataFrames
```

---

### Why does it return a list?

A webpage may contain multiple tables.

---

### How do you access first table?

```python
tables[0]
```

---

### What does match do?

Filters tables containing specific text.

---

### Can read_html read JavaScript tables?

No.

Because JavaScript is not executed.

---

### Which libraries are commonly required?

```text
lxml
html5lib
```

---

# Production Best Practices

Always:

```python
tables = pd.read_html(
    url,
    match="Required Table"
)
```

instead of loading everything.

Validate:

```python
len(tables)
```

before using:

```python
tables[0]
```

to avoid:

```python
IndexError
```

---

# Quick Revision

```python
# Basic
tables = pd.read_html(url)

# First table
df = tables[0]

# Match text
pd.read_html(
    url,
    match="Employee"
)

# Specific attributes
pd.read_html(
    url,
    attrs={"id":"sales"}
)

# Header row
pd.read_html(
    url,
    header=1
)
```
# Pandas Series 

A **Series** is the most fundamental data structure in Pandas.

Many people directly learn DataFrames and ignore Series, but internally:

```text
DataFrame = Collection of Series
```

Understanding Series properly makes DataFrames, GroupBy, Merge, and advanced Pandas much easier. 

---

# What is a Series?

A Series is:

> A one-dimensional labeled array capable of holding any datatype.

Think of it as:

* Excel Column
* SQL Column
* NumPy Array + Labels
* Python Dictionary with ordering



---

# Visual Representation

```text
Index    Value

0         10
1         20
2         30
3         40
```

Here:

```text
Index = Labels

Values = Actual Data
```

---

# Internal Structure of Series

A Series contains two major components:

```text
Series
 │
 ├── Index
 │
 └── Values (NumPy Array)
```

Example:

```python
s = pd.Series([10,20,30])
```

Internally:

```text
Index
------
0
1
2

Values
------
10
20
30
```

---

# Creating a Series

## Method 1: From List

```python
import pandas as pd

s = pd.Series([10,20,30,40])
print(s)
```

Output:

```text
0    10
1    20
2    30
3    40
dtype: int64
```

Most common method.

---

# Automatic Index Creation

When index isn't specified:

```python
pd.Series([10,20,30])
```

Pandas creates:

```text
0
1
2
```

automatically.

---

# Creating Series With Custom Index

```python
s = pd.Series(
    [10,20,30],
    index=["a","b","c"]
)
```

Output:

```text
a    10
b    20
c    30
```

---

# Why Custom Index?

Real-world examples:

```text
Employee IDs
Customer IDs
Product Codes
Country Names
```

Instead of:

```text
0
1
2
```

we use meaningful labels.

---

# Creating Series From Dictionary

Very common interview question.

```python
data = {
    "Raj":100,
    "Aman":200,
    "Rohit":300
}

s = pd.Series(data)
```

Output:

```text
Raj      100
Aman     200
Rohit    300
```

---

# What Happens Internally?

Dictionary:

```python
{
 "Raj":100
}
```

becomes:

```text
Index      Value

Raj         100
```

Keys become Index.

Values become Data.

---

# Creating Empty Series

```python
s = pd.Series(dtype="float64")
```

Output:

```text
Series([], dtype=float64)
```

Useful for dynamic operations.

---

# Series Datatypes

Series can store:

### Integers

```python
pd.Series([1,2,3])
```

dtype:

```text
int64
```

---

### Float

```python
pd.Series([1.1,2.2,3.3])
```

dtype:

```text
float64
```

---

### String

```python
pd.Series(["A","B","C"])
```

dtype:

```text
object
```

---

### Boolean

```python
pd.Series([True,False])
```

dtype:

```text
bool
```

---

# Checking Datatype

```python
s.dtype
```

Output:

```text
int64
```

Interview favorite.

---

# Important Attributes

## values

Returns underlying NumPy array.

```python
s.values
```

Output:

```python
array([10,20,30])
```

---

## index

Returns labels.

```python
s.index
```

Output:

```python
RangeIndex(start=0, stop=3)
```

---

## shape

```python
s.shape
```

Output:

```python
(3,)
```

Notice:

```text
One Dimension
```

---

## size

```python
s.size
```

Output:

```python
3
```

---

## ndim

Number of dimensions.

```python
s.ndim
```

Output:

```python
1
```

---

# Series vs NumPy Array

## NumPy

```python
arr = np.array([10,20,30])
```

Access:

```python
arr[0]
```

Output:

```text
10
```

Only position-based.

---

## Series

```python
s = pd.Series(
    [10,20,30],
    index=["a","b","c"]
)
```

Access:

```python
s["a"]
```

Output:

```text
10
```

Label-based.

---

# Accessing Elements

## By Position

```python
s[0]
```

Output:

```text
10
```

---

## By Label

```python
s["a"]
```

Output:

```text
10
```

---

# Slicing Series

```python
s[0:3]
```

Output:

```text
0 10
1 20
2 30
```

---

# Fancy Indexing

```python
s[[0,2]]
```

Output:

```text
0 10
2 30
```

---

# Boolean Indexing

Extremely important.

```python
s = pd.Series(
    [10,20,30,40]
)

s[s > 20]
```

Output:

```text
2 30
3 40
```

---

# Why Boolean Indexing Matters?

Used everywhere:

```python
Filtering
Data Cleaning
Feature Engineering
```

---

# Series is Vectorized

Huge interview topic.

Instead of:

```python
result = []

for i in s:
    result.append(i*2)
```

Use:

```python
s * 2
```

Output:

```text
20
40
60
80
```

Much faster.

---

# Arithmetic Operations

```python
s + 10
```

Output:

```text
20
30
40
50
```

---

```python
s * 2
```

Output:

```text
20
40
60
80
```

---

```python
s / 2
```

Output:

```text
5
10
15
20
```

---

# Internal Reason Why Fast?

Because operations happen on:

```text
NumPy Arrays
```

not Python loops.

---

# Series Alignment (MOST IMPORTANT)

This is one of Pandas' superpowers.

Example:

```python
s1 = pd.Series(
 [10,20,30],
 index=["a","b","c"]
)

s2 = pd.Series(
 [1,2,3],
 index=["a","b","c"]
)
```

Add:

```python
s1 + s2
```

Output:

```text
a 11
b 22
c 33
```

Works normally.

---

# Different Indexes

```python
s1
```

```text
a 10
b 20
c 30
```

---

```python
s2
```

```text
b 1
c 2
d 3
```

---

Addition:

```python
s1 + s2
```

Output:

```text
a NaN
b 21
c 32
d NaN
```

---

# Why?

Pandas aligns by:

```text
INDEX
```

not position.

This is a very common interview question.

---

# Hidden Production Bug

Many beginners assume:

```text
Row 1 + Row 1
Row 2 + Row 2
```

Wrong.

Pandas matches:

```text
Index Names
```

first.

---

# Handling Missing Alignment

Use:

```python
s1.add(
    s2,
    fill_value=0
)
```

Output:

```text
a 10
b 21
c 32
d 3
```

---

# Series Functions

## head()

```python
s.head()
```

First 5 records.

---

## tail()

```python
s.tail()
```

Last 5 records.

---

## sample()

```python
s.sample(3)
```

Random values.

---

## unique()

```python
s.unique()
```

Unique elements.

---

## nunique()

```python
s.nunique()
```

Count unique values.

---

## value_counts()

Very important.

```python
s.value_counts()
```

Example:

```python
s = pd.Series(
 ["A","A","B","C"]
)
```

Output:

```text
A 2
B 1
C 1
```

Frequently asked in interviews.

---

# Memory Usage

Check memory:

```python
s.memory_usage()
```

Output:

```text
Bytes consumed
```

Useful for optimization.

---

# Hidden Senior-Level Concepts

## Series Is Immutable Size-Wise

Values can change:

```python
s[0] = 100
```

Allowed.

---

Structure modifications create new objects internally.

---

## Strings Are Expensive

```python
object
```

dtype consumes significant memory.

For repeated values use:

```python
category
```

---

Example:

```python
s = s.astype("category")
```

Can reduce memory by:

```text
50%-95%
```

---

# Frequently Asked Interview Questions

### What is Series?

A one-dimensional labeled array.

---

### Difference Between List and Series?

List:

```text
No Labels
```

Series:

```text
Labels + NumPy Backend
```

---

### Difference Between NumPy Array and Series?

NumPy:

```text
Position Based
```

Series:

```text
Label Based + Position Based
```

---

### Why is Series Faster Than List?

Because:

```text
Vectorization
NumPy Backend
C Implementation
```

---

### What is Alignment in Pandas?

Operations match by index labels rather than positions.

---

### What is value_counts()?

Returns frequency of each unique value.

---

### Why use category dtype?

Reduces memory usage for repeated string values.

---

# Quick Revision

```python
# Create
s = pd.Series([10,20,30])

# Custom index
s = pd.Series(
 [10,20,30],
 index=["a","b","c"]
)

# Access
s["a"]

# Filter
s[s > 20]

# Vectorized
s * 2

# Unique
s.unique()

# Frequency
s.value_counts()

# Memory
s.memory_usage()
```
# Advanced Series Topics (`loc`, `iloc`, Reindexing, Sorting, Ranking, Missing Values & Statistical Operations)

This section covers the Series concepts that are heavily asked in interviews and used daily in Data Analysis, Data Engineering, and Machine Learning projects.

Most freshers know:

```python
s[0]
```

But experienced developers know:

```python
loc
iloc
reindex
rank
sort_values
sort_index
fillna
dropna
```

These are critical for production-grade Pandas development.

---

# 1. loc vs iloc

One of the most frequently asked Pandas interview questions.

---

## loc (Label-Based Indexing)

`loc` accesses data using:

```text
Index Labels
```

Example:

```python
s = pd.Series(
    [100,200,300],
    index=["a","b","c"]
)

s.loc["b"]
```

Output:

```python
200
```

---

### Internal Logic

```text
Find label "b"
↓
Return corresponding value
```

---

### Multiple Labels

```python
s.loc[["a","c"]]
```

Output:

```text
a 100
c 300
```

---

### Slicing with loc

```python
s.loc["a":"c"]
```

Output:

```text
a 100
b 200
c 300
```

---

## Important Rule

Unlike Python slicing:

```python
s.loc["a":"c"]
```

includes:

```text
c
```

This is extremely important.

---

# iloc (Position-Based Indexing)

`iloc` uses:

```text
Integer Position
```

Example:

```python
s.iloc[1]
```

Output:

```python
200
```

---

### Multiple Positions

```python
s.iloc[[0,2]]
```

Output:

```text
a 100
c 300
```

---

### Slicing

```python
s.iloc[0:2]
```

Output:

```text
a 100
b 200
```

Notice:

```text
Position 2 excluded
```

Same as Python slicing.

---

# loc vs iloc Comparison

| Feature                | loc        | iloc      |
| ---------------------- | ---------- | --------- |
| Uses                   | Labels     | Positions |
| End Included           | Yes        | No        |
| Error on Missing Label | Yes        | N/A       |
| Similar To             | Dictionary | List      |

---

# Common Interview Question

Given:

```python
s = pd.Series(
 [100,200,300],
 index=[10,20,30]
)
```

What is:

```python
s.loc[20]
```

Output:

```python
200
```

---

What is:

```python
s.iloc[1]
```

Output:

```python
200
```

Different reason.

One uses:

```text
Label
```

other uses:

```text
Position
```

---

# Hidden Production Bug

Many beginners write:

```python
s[0]
```

Thinking:

```text
First Row
```

But if custom indexes exist:

```python
index=[100,101,102]
```

unexpected behavior may occur.

Use:

```python
iloc
```

for positions.

---

# 2. Reindexing

Reindexing means:

```text
Changing Index Structure
```

without changing original data.

---

## Example

```python
s = pd.Series(
 [10,20,30],
 index=["a","b","c"]
)
```

---

Reindex:

```python
s.reindex(
 ["a","b","c","d"]
)
```

Output:

```text
a 10
b 20
c 30
d NaN
```

---

# Why NaN Appears?

New label:

```text
d
```

has no data.

Pandas inserts:

```python
NaN
```

---

# Reindex with Fill Value

```python
s.reindex(
 ["a","b","c","d"],
 fill_value=0
)
```

Output:

```text
a 10
b 20
c 30
d 0
```

---

# Production Use Case

Suppose monthly reports contain:

```text
Jan
Feb
Mar
```

Some months missing.

Reindex ensures:

```text
Jan
Feb
Mar
Apr
May
Jun
```

always appear.

---

# 3. Renaming Index

Original:

```text
a 10
b 20
```

---

Use:

```python
s.rename(
 {
   "a":"Math",
   "b":"Science"
 }
)
```

Output:

```text
Math      10
Science   20
```

---

# 4. Sorting Series

Very important.

---

## sort_values()

Sort by values.

```python
s = pd.Series(
 [30,10,20]
)
```

```python
s.sort_values()
```

Output:

```text
10
20
30
```

---

Descending:

```python
s.sort_values(
 ascending=False
)
```

Output:

```text
30
20
10
```

---

# sort_index()

Sort labels.

```python
s = pd.Series(
 [10,20,30],
 index=["c","a","b"]
)
```

```python
s.sort_index()
```

Output:

```text
a 20
b 30
c 10
```

---

# Difference

```python
sort_values()
```

Sorts:

```text
Data
```

---

```python
sort_index()
```

Sorts:

```text
Labels
```

---

# Interview Question

Difference between:

```python
sort_values()
```

and

```python
sort_index()
```

Very common.

---

# 5. Ranking

Used heavily in:

* Finance
* Analytics
* Leaderboards
* Reporting

---

Example

```python
s = pd.Series(
 [100,300,200]
)
```

```python
s.rank()
```

Output:

```text
1
3
2
```

Meaning:

```text
100 → Rank 1
200 → Rank 2
300 → Rank 3
```

---

# Why Ranking Matters?

Examples:

```text
Top Customers
Top Employees
Top Products
Top Students
```

---

# Different Ranking Methods

## Average

Default.

```python
s.rank()
```

---

## Dense Rank

```python
s.rank(
 method="dense"
)
```

SQL-like dense ranking.

---

# Missing Values in Series

One of the most important topics.

---

# Creating Missing Values

```python
import numpy as np

s = pd.Series(
 [10,np.nan,30]
)
```

Output:

```text
0 10
1 NaN
2 30
```

---

# Detect Missing Values

## isna()

```python
s.isna()
```

Output:

```text
False
True
False
```

---

## isnull()

Same as:

```python
isna()
```

---

# Count Missing Values

```python
s.isna().sum()
```

Output:

```python
1
```

---

# Removing Missing Values

## dropna()

```python
s.dropna()
```

Output:

```text
10
30
```

---

# Filling Missing Values

## fillna()

```python
s.fillna(0)
```

Output:

```text
10
0
30
```

---

# Forward Fill

```python
s.fillna(
 method="ffill"
)
```

Example:

```text
10
NaN
30
```

becomes:

```text
10
10
30
```

---

# Backward Fill

```python
s.fillna(
 method="bfill"
)
```

becomes:

```text
10
30
30
```

---

# Production Example

Stock Data:

```text
Day1 100
Day2 NaN
Day3 120
```

Forward fill:

```text
Day2 = 100
```

Useful in time-series data.

---

# Statistical Functions

Series comes with built-in statistics.

---

## Sum

```python
s.sum()
```

---

## Mean

```python
s.mean()
```

---

## Median

```python
s.median()
```

---

## Mode

```python
s.mode()
```

---

## Minimum

```python
s.min()
```

---

## Maximum

```python
s.max()
```

---

## Standard Deviation

```python
s.std()
```

---

## Variance

```python
s.var()
```

---

# describe()

One of the most important methods.

```python
s.describe()
```

Output:

```text
count
mean
std
min
25%
50%
75%
max
```

Quick summary of data.

---

# Why Interviewers Love describe()

Single command gives:

```text
Data Overview
Distribution
Spread
Outliers Clues
```

---

# Mathematical Operations

```python
s + 10
```

---

```python
s * 2
```

---

```python
s ** 2
```

---

```python
np.sqrt(s)
```

---

All operations are:

```text
Vectorized
```

No explicit loops required.

---

# Hidden Senior-Level Concepts

## Series Alignment Still Applies

```python
s1 + s2
```

matches:

```text
Index Labels
```

not positions.

Many production bugs occur because developers forget this.

---

## Avoid Python Loops

Bad:

```python
for i in s:
    ...
```

Good:

```python
s * 2
```

Vectorized operations are significantly faster.

---

## Repeated Strings

Instead of:

```python
object
```

use:

```python
category
```

for memory optimization.

---

# Frequently Asked Interview Questions

### Difference Between loc and iloc?

`loc` uses labels.

`iloc` uses positions.

---

### Does loc include ending index?

Yes.

---

### Does iloc include ending index?

No.

---

### What does reindex do?

Creates a new index structure and aligns existing data.

---

### Difference Between sort_values and sort_index?

`sort_values()` sorts data.

`sort_index()` sorts labels.

---

### How do you detect missing values?

```python
isna()
```

or

```python
isnull()
```

---

### Difference Between fillna and dropna?

`fillna()` replaces missing values.

`dropna()` removes missing values.

---

### What does describe() return?

Statistical summary of data.

---

# Quick Revision

```python
# Label-based
s.loc["a"]

# Position-based
s.iloc[0]

# Reindex
s.reindex(["a","b","c"])

# Sort values
s.sort_values()

# Sort index
s.sort_index()

# Missing values
s.isna()

# Remove missing
s.dropna()

# Fill missing
s.fillna(0)

# Statistics
s.describe()

# Rank
s.rank()
```

# DataFrame Deep Dive 

If Pandas has a heart, it is the **DataFrame**.

Almost every real-world Pandas operation eventually works on a DataFrame.

Examples:

* Customer Data
* Employee Data
* Sales Reports
* Banking Transactions
* Machine Learning Datasets
* ETL Pipelines

All are represented using DataFrames. 

---

# What is a DataFrame?

A DataFrame is:

> A two-dimensional labeled tabular data structure with rows and columns.

Think of it as:

* Excel Sheet
* SQL Table
* Collection of Series
* In-memory Database Table



---

# Visual Representation

```text
        Name     Age     Salary

0       Raj      22      50000
1       Aman     25      60000
2       Rohit    30      70000
```

Here:

```text
Rows    → Records

Columns → Features

Index   → Row Labels
```

---

# Internal Architecture of DataFrame

Most beginners think:

```text
DataFrame
     ↓
Rows
```

Wrong.

Internally Pandas stores data:

```text
Column-wise
```

---

Example:

```python
df
```

```text
Name   Age   Salary

Raj    22    50000
Aman   25    60000
```

Internally:

```text
Name Block

Raj
Aman
```

---

```text
Age Block

22
25
```

---

```text
Salary Block

50000
60000
```

This architecture is one reason Pandas is fast.

---

# Why Column-Based Storage?

Suppose:

```python
df["Salary"] * 2
```

Pandas accesses one continuous memory block.

Fast.

CPU-cache friendly.

---

# DataFrame = Collection of Series

Example:

```python
df
```

```text
Name Age Salary
```

Internally:

```text
DataFrame

 ├── Series(Name)
 ├── Series(Age)
 └── Series(Salary)
```

Very important interview question.

---

# Creating DataFrames

---

# Method 1: Dictionary

Most common.

```python
import pandas as pd

df = pd.DataFrame({
    "Name":["Raj","Aman"],
    "Age":[22,25]
})
```

Output:

```text
   Name  Age
0  Raj   22
1 Aman   25
```

---

# Internal Conversion

Dictionary:

```python
{
 "Name":[...],
 "Age":[...]
}
```

becomes:

```text
Column Name
Column Age
```

---

# Method 2: List of Lists

```python
df = pd.DataFrame([
    ["Raj",22],
    ["Aman",25]
],
columns=["Name","Age"])
```

Output:

```text
   Name  Age
0  Raj   22
1 Aman   25
```

---

# Method 3: NumPy Array

```python
import numpy as np

arr = np.array([
 [1,2],
 [3,4]
])

df = pd.DataFrame(
 arr,
 columns=["A","B"]
)
```

---

# Method 4: List of Dictionaries

Very common in APIs.

```python
data = [
 {"Name":"Raj","Age":22},
 {"Name":"Aman","Age":25}
]

df = pd.DataFrame(data)
```

---

# DataFrame Attributes

---

## shape

Returns:

```python
df.shape
```

Output:

```python
(3,2)
```

Meaning:

```text
3 Rows
2 Columns
```

---

## ndim

```python
df.ndim
```

Output:

```python
2
```

DataFrame is two-dimensional.

---

## size

```python
df.size
```

Output:

```python
6
```

Formula:

```text
Rows × Columns
```

---

## columns

```python
df.columns
```

Output:

```python
Index(['Name','Age'])
```

---

## index

```python
df.index
```

Output:

```python
RangeIndex(start=0, stop=3)
```

---

## dtypes

Most important attribute.

```python
df.dtypes
```

Output:

```text
Name    object
Age     int64
```

---

## info()

Very frequently used.

```python
df.info()
```

Output:

```text
Rows
Columns
Datatype
Memory Usage
Null Count
```

---

# Why info() Is Important?

Before cleaning data always run:

```python
df.info()
```

to understand:

```text
Structure
Missing Values
Memory
Datatypes
```

---

# head()

Shows first rows.

```python
df.head()
```

Default:

```text
First 5 rows
```

---

# head(10)

```python
df.head(10)
```

First 10 rows.

---

# tail()

Last rows.

```python
df.tail()
```

Output:

```text
Last 5 rows
```

---

# sample()

Random rows.

```python
df.sample(3)
```

Useful for data inspection.

---

# Selecting Columns

One of the most used operations.

---

## Single Column

```python
df["Name"]
```

Output:

```text
Raj
Aman
```

Returns:

```python
Series
```

Interview favorite.

---

# Verify

```python
type(df["Name"])
```

Output:

```python
pandas.Series
```

---

# Multiple Columns

```python
df[
 ["Name","Age"]
]
```

Output:

```text
Name Age
Raj 22
```

Returns:

```python
DataFrame
```

---

# Important Interview Question

### Why Double Brackets?

Single:

```python
df["Name"]
```

returns:

```python
Series
```

---

Double:

```python
df[["Name"]]
```

returns:

```python
DataFrame
```

---

# Adding New Column

```python
df["Bonus"] = 5000
```

Output:

```text
Name Age Bonus
Raj 22 5000
```

Column added dynamically.

---

# Creating Derived Columns

```python
df["DoubleSalary"] = (
    df["Salary"] * 2
)
```

Vectorized operation.

---

# Renaming Columns

```python
df.rename(
 columns={
   "Name":"EmployeeName"
 }
)
```

Output:

```text
EmployeeName
```

---

# Changing All Column Names

```python
df.columns = [
 "emp_name",
 "emp_age"
]
```

---

# Removing Columns

```python
df.drop(
 columns=["Age"]
)
```

Output:

```text
Age removed
```

---

# Important Rule

By default:

```python
drop()
```

does NOT modify original DataFrame.

---

Need:

```python
df = df.drop(...)
```

or

```python
inplace=True
```

---

# Selecting Rows

---

## By Position

```python
df.iloc[0]
```

First row.

Returns:

```python
Series
```

---

## Multiple Rows

```python
df.iloc[0:3]
```

Returns DataFrame.

---

# By Label

```python
df.loc[0]
```

Access row label 0.

---

# loc vs iloc (DataFrame)

Exactly same concept as Series.

| Method | Uses      |
| ------ | --------- |
| loc    | Labels    |
| iloc   | Positions |

---

# Selecting Specific Cells

Example:

```python
df.loc[0,"Name"]
```

Output:

```python
Raj
```

---

Position Based:

```python
df.iloc[0,1]
```

Output:

```python
22
```

---

# Boolean Filtering

Most important DataFrame operation.

---

Example:

```python
df[
 df["Age"] > 23
]
```

Output:

```text
Aman
Rohit
```

---

# Internal Process

```text
Age > 23
     ↓
True False True
     ↓
Filter Rows
```

---

# Multiple Conditions

```python
df[
 (df["Age"] > 23)
 &
 (df["Salary"] > 50000)
]
```

---

# OR Condition

```python
df[
 (df["Age"] > 23)
 |
 (df["Salary"] > 50000)
]
```

---

# Common Filtering Methods

## isin()

```python
df[
 df["City"].isin(
  ["Delhi","Mumbai"]
 )
]
```

---

## between()

```python
df[
 df["Salary"]
 .between(50000,100000)
]
```

---

## str.contains()

```python
df[
 df["Name"]
 .str.contains("Raj")
]
```

Very common.

---

# Hidden Production Tips

Never do:

```python
for row in df.iterrows():
```

unless absolutely necessary.

Slow.

---

Prefer:

```python
Vectorized Operations
```

Example:

Bad:

```python
for i in range(len(df)):
    df["Salary"][i] *= 2
```

---

Good:

```python
df["Salary"] *= 2
```

Much faster.

---

# Frequently Asked Interview Questions

### What is a DataFrame?

A two-dimensional labeled tabular data structure.

---

### Is DataFrame built on Series?

Yes.

A DataFrame is a collection of Series.

---

### What does df.shape return?

```text
(rows, columns)
```

---

### Difference between:

```python
df["Name"]
```

and

```python
df[["Name"]]
```

First returns:

```python
Series
```

Second returns:

```python
DataFrame
```

---

### Difference between loc and iloc?

loc → Labels

iloc → Positions

---

### Why is Pandas fast?

Because:

* NumPy Backend
* Vectorization
* Column-Based Storage
* C Implementation

---

# Quick Revision

```python
# Create
df = pd.DataFrame(...)

# Info
df.info()

# Shape
df.shape

# Columns
df.columns

# Single Column
df["Name"]

# Multiple Columns
df[["Name","Age"]]

# Add Column
df["Bonus"] = 1000

# Drop Column
df.drop(columns=["Bonus"])

# Row
df.iloc[0]

# Cell
df.loc[0,"Name"]

# Filter
df[df["Age"] > 25]
```
 # Advanced DataFrame Operations (`set_index()`, `reset_index()`, `drop()`, `rename()`, `insert()`, `assign()`, Sorting, Duplicates & Memory Optimization)

These operations are used in almost every real-world Pandas project.

Typical workflow:

```text
Read Data
   ↓
Clean Data
   ↓
Modify Columns
   ↓
Handle Duplicates
   ↓
Set Index
   ↓
Transform Data
   ↓
Export Data
```

Understanding these methods is essential for Data Analysts, Data Engineers, ML Engineers, and Python Developers.

---

# 1. Understanding Index in DataFrame

Every DataFrame has:

```text
Rows
Columns
Index
```

Example:

```python
df
```

```text
    Name    Age

0   Raj      22
1   Aman     25
2   Rohit    30
```

Here:

```text
0
1
2
```

is the index.

---

# Why Index Exists?

Index helps Pandas:

```text
Locate Data
Align Data
Filter Data
Join Data
```

efficiently.

---

# 2. set_index()

One of the most important Pandas methods.

Used to convert a column into the DataFrame index.

---

## Example

```python
df
```

```text
EmpID Name Age

101   Raj  22
102   Aman 25
103   Rohit 30
```

---

Use:

```python
df.set_index("EmpID")
```

Output:

```text
       Name Age

EmpID
101    Raj  22
102    Aman 25
103    Rohit 30
```

---

# Why Use set_index()?

Instead of:

```python
df[df["EmpID"]==101]
```

you can do:

```python
df.loc[101]
```

Faster and cleaner.

---

# Important Rule

```python
df.set_index("EmpID")
```

does NOT modify original DataFrame.

Need:

```python
df = df.set_index("EmpID")
```

or

```python
inplace=True
```

---

# Multiple Indexes

Advanced concept.

```python
df.set_index(
 ["State","City"]
)
```

Creates:

```text
MultiIndex
```

Example:

```text
State    City

UP       Noida
UP       Lucknow
MP       Bhopal
```

---

# Interview Question

### What is MultiIndex?

Multiple levels of indexing in a DataFrame.

---

# 3. reset_index()

Opposite of:

```python
set_index()
```

---

Example

Current:

```text
       Name Age

EmpID
101    Raj 22
102    Aman 25
```

---

Use:

```python
df.reset_index()
```

Output:

```text
EmpID Name Age

101   Raj 22
102   Aman 25
```

Index becomes normal column.

---

# Why Use reset_index()?

Required after:

```python
groupby()
merge()
pivot()
```

operations.

---

# Remove Index Completely

```python
df.reset_index(
 drop=True
)
```

Output:

```text
0
1
2
```

new default index.

---

# Interview Question

Difference between:

```python
reset_index()
```

and

```python
reset_index(drop=True)
```

---

Without drop:

Old index becomes column.

---

With drop:

Old index removed.

---

# 4. drop()

One of the most used methods.

Used to remove:

```text
Rows
Columns
```

---

# Drop Column

```python
df.drop(
 columns=["Age"]
)
```

Output:

```text
Age removed
```

---

# Drop Multiple Columns

```python
df.drop(
 columns=[
   "Age",
   "Salary"
 ]
)
```

---

# Drop Row

```python
df.drop(
 index=0
)
```

Output:

```text
First row removed
```

---

# Drop Multiple Rows

```python
df.drop(
 index=[0,2]
)
```

---

# Internal Working

Pandas creates:

```text
New DataFrame
```

by default.

Original remains unchanged.

---

# inplace=True

```python
df.drop(
 columns=["Age"],
 inplace=True
)
```

Modifies original DataFrame.

---

# Hidden Interview Question

Why avoid excessive:

```python
inplace=True
```

?

Modern Pandas often creates copies internally anyway.

Many companies prefer:

```python
df = df.drop(...)
```

for readability.

---

# 5. rename()

Used to rename:

```text
Columns
Indexes
```

---

# Rename Columns

```python
df.rename(
 columns={
   "Name":"Employee_Name"
 }
)
```

Output:

```text
Employee_Name
```

---

# Rename Multiple Columns

```python
df.rename(
 columns={
  "Name":"EmpName",
  "Age":"EmpAge"
 }
)
```

---

# Rename Index

```python
df.rename(
 index={
  0:"A",
  1:"B"
 }
)
```

---

# Production Best Practice

Standardize names:

Bad:

```text
Employee Name
EMP_NAME
employeeName
```

Good:

```text
employee_name
```

---

# 6. insert()

Adds column at a specific position.

---

# Normal Column Creation

```python
df["Salary"] = 50000
```

Always adds at end.

---

# insert()

```python
df.insert(
 1,
 "Salary",
 50000
)
```

Output:

```text
Name Salary Age
```

---

Parameters:

```python
Position
Column Name
Value
```

---

# Why Use insert()?

When column order matters.

Example:

```text
Financial Reports
CSV Exports
Client Deliverables
```

---

# 7. assign()

Very popular in modern Pandas.

Creates new columns while keeping code clean.

---

Example

```python
df.assign(
 Bonus=5000
)
```

Output:

```text
New Bonus column
```

---

# Multiple Columns

```python
df.assign(
 Bonus=5000,
 Tax=2000
)
```

---

# Derived Columns

```python
df.assign(
 DoubleSalary=
 df["Salary"]*2
)
```

---

# Why assign()?

Method chaining.

Example:

```python
(
 df
 .dropna()
 .assign(
     Bonus=5000
 )
 .sort_values("Age")
)
```

Clean and readable.

---

# 8. Sorting DataFrames

Very important.

---

# sort_values()

Sort by column values.

---

Example

```python
df
```

```text
Name Age

Raj 30
Aman 22
Rohit 25
```

---

```python
df.sort_values(
 "Age"
)
```

Output:

```text
Aman 22
Rohit 25
Raj 30
```

---

# Descending Order

```python
df.sort_values(
 "Age",
 ascending=False
)
```

Output:

```text
Raj 30
Rohit 25
Aman 22
```

---

# Multiple Columns

```python
df.sort_values(
 ["Department","Salary"]
)
```

Sort hierarchy:

```text
Department
     ↓
Salary
```

---

# sort_index()

Sorts row labels.

---

Example

```text
3 Raj
1 Aman
2 Rohit
```

---

```python
df.sort_index()
```

Output:

```text
1 Aman
2 Rohit
3 Raj
```

---

# Interview Question

Difference between:

```python
sort_values()
```

and

```python
sort_index()
```

---

sort_values:

```text
Sort data
```

sort_index:

```text
Sort labels
```

---

# 9. Duplicate Handling

Extremely important in real-world projects.

---

# Detect Duplicates

```python
df.duplicated()
```

Output:

```text
False
True
False
```

---

Meaning:

```text
Row already exists
```

---

# Example

```text
Raj 22
Raj 22
Aman 25
```

Second Raj row:

```text
Duplicate
```

---

# Count Duplicates

```python
df.duplicated().sum()
```

---

# Remove Duplicates

```python
df.drop_duplicates()
```

Output:

```text
Unique rows only
```

---

# Specific Columns

```python
df.drop_duplicates(
 subset=["Name"]
)
```

Duplicate check only on Name.

---

# Keep First

Default:

```python
keep="first"
```

---

# Keep Last

```python
df.drop_duplicates(
 keep="last"
)
```

---

# Remove All Duplicates

```python
df.drop_duplicates(
 keep=False
)
```

---

# Production Example

Customer Data:

```text
CustomerID
101
101
102
103
```

Duplicates create:

```text
Wrong Reports
Wrong Revenue
Wrong Analytics
```

Always check duplicates.

---

# Memory Optimization

One of the most important senior-level topics.

---

# Check Memory Usage

```python
df.memory_usage()
```

---

Detailed:

```python
df.memory_usage(
 deep=True
)
```

---

# Why deep=True?

Includes:

```text
String Memory
Object Memory
```

which normal memory usage ignores.

---

# Convert Object → Category

Suppose:

```text
Delhi
Delhi
Delhi
Mumbai
Mumbai
```

Repeated many times.

---

Current:

```python
object
```

---

Convert:

```python
df["City"] = (
 df["City"]
 .astype("category")
)
```

Memory reduction:

```text
50% - 95%
```

possible.

---

# Numeric Optimization

Default:

```python
int64
```

Memory:

```text
8 bytes
```

---

Use:

```python
int32
```

Memory:

```text
4 bytes
```

---

Use:

```python
int16
```

Memory:

```text
2 bytes
```

---

# Hidden Production Tips

### Always Run

```python
df.info()
```

first.

---

### Check Duplicates

```python
df.duplicated().sum()
```

before analysis.

---

### Optimize Strings

```python
category
```

for repeated values.

---

### Avoid Loops

Bad:

```python
for row in df.iterrows():
```

Good:

```python
Vectorized Operations
```

---

# Frequently Asked Interview Questions

### What is set_index()?

Converts a column into the DataFrame index.

---

### Difference between set_index() and reset_index()?

`set_index()` creates index from columns.

`reset_index()` converts index back into column.

---

### What does drop() do?

Removes rows or columns.

---

### Difference between rename() and columns assignment?

`rename()` changes selected columns.

`df.columns = [...]` changes all columns.

---

### Difference between sort_values() and sort_index()?

`sort_values()` sorts data.

`sort_index()` sorts labels.

---

### How do you find duplicates?

```python
df.duplicated()
```

---

### How do you remove duplicates?

```python
df.drop_duplicates()
```

---

### Why use category datatype?

To reduce memory usage and improve performance for repeated values.

---

# Quick Revision

```python
# Set index
df.set_index("EmpID")

# Reset index
df.reset_index()

# Drop column
df.drop(columns=["Age"])

# Rename
df.rename(
 columns={"Name":"EmpName"}
)

# Insert
df.insert(1,"Salary",50000)

# Assign
df.assign(Bonus=5000)

# Sort
df.sort_values("Age")

# Duplicates
df.drop_duplicates()

# Memory
df.memory_usage(deep=True)
```
# Missing Values Deep Dive (`isnull()`, `notnull()`, `dropna()`, `fillna()`, Interpolation & Real-World Data Cleaning)

Missing values are one of the biggest challenges in Data Analysis, Data Engineering, Data Science, and Machine Learning.

In real-world datasets:

```text
Customer Data
Sales Data
Healthcare Data
Banking Data
IoT Data
Logs
```

missing values are everywhere.

A large portion of a Data Analyst's work involves handling null values correctly.

---

# What Are Missing Values?

A missing value means:

```text
Data Exists
But Value Is Missing
```

Example:

| Name  | Age | Salary |
| ----- | --- | ------ |
| Raj   | 22  | 50000  |
| Aman  | NaN | 60000  |
| Rohit | 30  | NaN    |

Here:

```text
Age → Missing
Salary → Missing
```

---

# How Pandas Represents Missing Values?

Pandas uses:

```python
NaN
```

Meaning:

```text
Not a Number
```

Internally:

```python
numpy.nan
```

---

# Creating Missing Values

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "Name":["Raj","Aman","Rohit"],
    "Age":[22,np.nan,30]
})
```

Output:

```text
Name   Age

Raj    22
Aman   NaN
Rohit  30
```

---

# Why Missing Values Occur?

## User Input

User leaves field blank.

```text
Age:
```

No value entered.

---

## System Failure

Application crashes while saving.

---

## Sensor Failure

IoT devices stop transmitting.

---

## Data Merge Issues

Data available in one source but not another.

---

## Corrupted Files

CSV or Excel contains empty cells.

---

# Important Note

Missing value is NOT:

```python
0
```

Missing value is NOT:

```python
""
```

Missing value is NOT:

```python
False
```

Missing value is:

```python
NaN
```

---

# 1. isnull()

Most used null-checking function.

---

## Syntax

```python
df.isnull()
```

Output:

```text
Name    Age

False   False
False   True
False   False
```

Meaning:

```text
True  → Missing

False → Present
```

---

# Internal Working

Pandas scans every cell:

```text
Value Exists?
      ↓

Yes → False

No → True
```

---

# Example

```python
df["Age"].isnull()
```

Output:

```text
0 False
1 True
2 False
```

---

# Interview Question

### Why does isnull return True for missing values?

Because it is detecting:

```text
Null Presence
```

not valid values.

---

# 2. isna()

Exactly same as:

```python
isnull()
```

Example:

```python
df.isna()
```

Output identical.

---

# Why Two Names?

Historical reasons.

Use either.

---

# Interview Question

Difference between:

```python
isnull()
```

and

```python
isna()
```

Answer:

```text
No Difference
```

---

# 3. notnull()

Opposite of:

```python
isnull()
```

---

Example

```python
df.notnull()
```

Output:

```text
True
False
True
```

Meaning:

```text
True → Value Exists

False → Missing
```

---

# notna()

Equivalent to:

```python
notnull()
```

---

# Counting Missing Values

Most important operation.

---

## Per Column

```python
df.isnull().sum()
```

Example:

```text
Name      0
Age       1
Salary    2
```

Meaning:

```text
Age → 1 missing

Salary → 2 missing
```

---

# Why sum() Works?

Internally:

```text
True = 1

False = 0
```

Example:

```text
False
True
False
```

becomes:

```text
0
1
0
```

Sum:

```text
1
```

---

# Total Missing Values

```python
df.isnull().sum().sum()
```

Output:

```python
3
```

---

# Percentage Missing

Very important in real projects.

```python
(
 df.isnull()
 .mean()
 * 100
)
```

Output:

```text
Age      20%
Salary   40%
```

---

# Why Percentage Matters?

Suppose:

```text
Salary Column
95% Missing
```

Usually:

```text
Drop Column
```

rather than fill it.

---

# Filtering Missing Records

Example:

```python
df[
 df["Age"].isnull()
]
```

Output:

```text
Only rows with missing Age
```

---

# Filtering Non-Missing Records

```python
df[
 df["Age"].notnull()
]
```

Output:

```text
Rows having Age values
```

---

# 4. dropna()

One of the most important cleaning methods.

Used to remove missing values.

---

# Example

```text
Name Age

Raj 22
Aman NaN
Rohit 30
```

---

Use:

```python
df.dropna()
```

Output:

```text
Raj 22
Rohit 30
```

Row containing NaN removed.

---

# Internal Logic

```text
Row Contains NaN?
      ↓

Yes → Remove

No → Keep
```

---

# Default Behavior

```python
df.dropna()
```

removes:

```text
Rows
```

---

# axis Parameter

## Remove Rows

```python
df.dropna(axis=0)
```

Default.

---

## Remove Columns

```python
df.dropna(axis=1)
```

Output:

```text
Entire column removed
```

if it contains NaN.

---

# Hidden Production Danger

Suppose:

```text
Salary
```

has one missing value.

```python
df.dropna(axis=1)
```

may remove the entire Salary column.

Always verify before dropping columns.

---

# how Parameter

## how="any"

Default.

```python
df.dropna(
 how="any"
)
```

If even one NaN exists:

```text
Drop Row
```

---

## how="all"

```python
df.dropna(
 how="all"
)
```

Remove row only when:

```text
All Values Missing
```

---

Example

```text
Name Age

NaN NaN
```

removed.

---

# thresh Parameter

Advanced interview topic.

```python
df.dropna(
 thresh=2
)
```

Meaning:

```text
Keep rows having at least 2 non-null values
```

---

# subset Parameter

Check only selected columns.

```python
df.dropna(
 subset=["Salary"]
)
```

Remove rows where Salary is missing.

Ignore other columns.

---

# 5. fillna()

Instead of removing data:

```text
Replace Missing Values
```

---

Example

```python
df.fillna(0)
```

Output:

```text
NaN → 0
```

---

# Fill String Columns

```python
df.fillna(
 "Unknown"
)
```

Output:

```text
NaN → Unknown
```

---

# Fill Specific Columns

```python
df.fillna({
    "Age":0,
    "City":"Unknown"
})
```

---

# Why fillna Is Better Than dropna?

Suppose:

```text
10 Million Rows
```

Dropping rows may remove huge amounts of data.

Sometimes replacement is better.

---

# Statistical Imputation

Very important for Machine Learning.

---

## Mean

```python
df["Age"].fillna(
 df["Age"].mean()
)
```

Example:

```text
20
30
40
NaN
```

Mean:

```text
30
```

Fill:

```text
20
```

# `fillna()` Advanced, Interpolation, Real-World Missing Value Strategies & Interview Questions

This continues from Part 11.

---

# Statistical Imputation (Continuation)

Instead of:

```python
df.fillna(0)
```

which may distort data, we often use statistical values.

---

# 1. Mean Imputation

Most common for numerical data.

```python
df["Age"] = df["Age"].fillna(
    df["Age"].mean()
)
```

Example:

```text
20
30
40
NaN
```

Mean:

```text
30
```

Result:

```text
20
30
40
30
```

---

## When To Use?

Good when data distribution is fairly normal.

Example:

```text
Student Marks
Temperature
Height
Weight
```

---

## Problem

Outliers affect mean heavily.

Example:

```text
20
25
30
1000
```

Mean becomes:

```text
268.75
```

Wrong representation.

---

# 2. Median Imputation

Very important interview topic.

```python
df["Salary"] = df["Salary"].fillna(
    df["Salary"].median()
)
```

Example:

```text
10000
15000
20000
1000000
NaN
```

Median:

```text
20000
```

Fill:

```text
NaN → 20000
```

---

## Why Median?

Less affected by outliers.

---

## Real World Usage

Used in:

```text
Banking
Healthcare
Insurance
Finance
```

where extreme values are common.

---

# 3. Mode Imputation

For categorical columns.

```python
df["City"] = df["City"].fillna(
    df["City"].mode()[0]
)
```

Example:

```text
Delhi
Delhi
Mumbai
NaN
```

Mode:

```text
Delhi
```

Fill:

```text
NaN → Delhi
```

---

# Forward Fill (ffill)

Very common in time-series data.

---

## Example

```text
Date    Price

1 Jan   100
2 Jan   NaN
3 Jan   120
```

Apply:

```python
df.fillna(method="ffill")
```

Result:

```text
100
100
120
```

---

## Internal Logic

```text
Take Previous Valid Value
```

---

# Production Use Cases

```text
Stock Prices
Sensor Data
IoT Data
Weather Data
```

---

# Backward Fill (bfill)

Uses next valid value.

```python
df.fillna(method="bfill")
```

---

Example

```text
100
NaN
120
```

Result:

```text
100
120
120
```

---

# Difference

| Method | Uses           |
| ------ | -------------- |
| ffill  | Previous Value |
| bfill  | Next Value     |

---

# Limit Parameter

Advanced interview topic.

---

Example

```python
df.fillna(
 method="ffill",
 limit=1
)
```

Meaning:

```text
Fill only 1 consecutive NaN
```

---

Example

Before:

```text
10
NaN
NaN
30
```

After:

```text
10
10
NaN
30
```

Only first NaN filled.

---

# Interpolation

One of the most powerful Pandas features.

---

# What Is Interpolation?

Estimate missing values mathematically.

---

Example

```text
10
NaN
30
```

Use:

```python
df.interpolate()
```

Output:

```text
10
20
30
```

---

# How Did Pandas Calculate 20?

It observed:

```text
10 → 30
```

Missing value lies in middle.

Linear estimate:

```text
20
```

---

# Why Interpolation Is Better?

Instead of:

```text
10
10
30
```

or

```text
10
30
30
```

it preserves trend.

---

# Real World Usage

```text
Sensor Data
Temperature Data
Financial Data
Telemetry Data
Scientific Measurements
```

---

# Types of Interpolation

## Linear

Default.

```python
df.interpolate(
 method="linear"
)
```

Most common.

---

## Time Interpolation

Used for date indexes.

```python
df.interpolate(
 method="time"
)
```

---

## Polynomial

Advanced.

```python
df.interpolate(
 method="polynomial",
 order=2
)
```

Used in scientific datasets.

---

# Missing Value Strategy (Senior-Level Concept)

Never blindly use:

```python
fillna(0)
```

Interviewers love this discussion.

---

# Strategy 1: Understand Why Data Is Missing

Ask:

```text
Why is value missing?
```

---

Example:

```text
Customer Salary Missing
```

Possibilities:

```text
Unknown
Not Applicable
System Error
User Refused
```

Each requires different handling.

---

# Strategy 2: Missing Completely At Random (MCAR)

Missing unrelated to anything.

Example:

```text
Network failure caused data loss
```

Safe to use:

```text
Mean
Median
Drop
```

---

# Strategy 3: Missing At Random (MAR)

Missing depends on another variable.

Example:

```text
Income missing mostly for students
```

Need smarter handling.

---

# Strategy 4: Missing Not At Random (MNAR)

Most dangerous.

Example:

```text
High earners hide salary
```

Missing itself contains information.

Dropping values can bias results.

---

# When Should You Drop Rows?

Good if:

```text
Less than 5% missing
```

and dataset is huge.

---

Example:

```text
10 Million Rows
50 Missing Rows
```

Safe to drop.

---

# When Should You Drop Columns?

If:

```text
80%-95% missing
```

and column provides little value.

---

Example:

```text
Secondary Phone Number
```

missing for most users.

---

# Machine Learning Perspective

Many ML algorithms cannot handle NaN.

Example:

```text
Linear Regression
Logistic Regression
SVM
```

Need imputation first.

---

# Checking Missing Value Percentage

Very common interview question.

```python
(
 df.isnull()
 .sum()
 /
 len(df)
) * 100
```

Output:

```text
Age      5%
Salary   20%
```

---

# Visualizing Missing Data

Useful in EDA.

```python
import seaborn as sns

sns.heatmap(
    df.isnull()
)
```

Shows missing-value patterns.

---

# Hidden Production Problems

## Problem 1

```python
fillna(0)
```

for salary.

Example:

```text
Salary Missing
```

becomes:

```text
0
```

which may be interpreted as:

```text
Employee earns nothing
```

Wrong business meaning.

---

## Problem 2

Mean imputation on skewed data.

Can distort distributions.

---

## Problem 3

Dropping rows aggressively.

Example:

```python
dropna()
```

may remove:

```text
40%-50%
```

of dataset accidentally.

Always check:

```python
df.shape
```

before and after.

---

# Frequently Asked Interview Questions

### Difference Between isnull() and notnull()?

```python
isnull()
```

detects missing values.

```python
notnull()
```

detects available values.

---

### Difference Between dropna() and fillna()?

dropna:

```text
Removes data
```

fillna:

```text
Replaces missing data
```

---

### Mean vs Median Imputation?

Mean:

```text
Affected by outliers
```

Median:

```text
Robust against outliers
```

---

### When Use Mode?

Categorical variables.

Example:

```text
Gender
City
Department
```

---

### What Is Interpolation?

Estimating missing values mathematically using nearby observations.

---

### Difference Between ffill and bfill?

ffill:

```text
Previous value
```

bfill:

```text
Next value
```

---

# Real Interview Scenario

**Question:**

You have:

```text
1 Million Rows

Age → 2% Missing

Salary → 60% Missing
```

What would you do?

**Expected Answer:**

```text
Age:
Use median/mean imputation

Salary:
Investigate business importance.
Possibly drop column if not useful.
```

Not:

```text
fillna(0) everywhere
```

---

# Quick Revision

```python
# Detect
df.isnull()

# Count
df.isnull().sum()

# Drop
df.dropna()

# Fill
df.fillna(0)

# Mean
df["Age"].fillna(
    df["Age"].mean()
)

# Median
df["Salary"].fillna(
    df["Salary"].median()
)

# Mode
df["City"].fillna(
    df["City"].mode()[0]
)

# Forward Fill
df.ffill()

# Backward Fill
df.bfill()

# Interpolation
df.interpolate()
```
# `concat()` in Pandas

`concat()` is one of the most frequently used Pandas functions in ETL pipelines, Data Engineering, Data Analysis, and Machine Learning.

Real-world examples:

```text
Monthly Sales Files
Customer Data From Multiple Sources
Log Files
CSV Chunks
API Responses
```

All often need to be combined together.

Instead of SQL `UNION`, Pandas provides:

```python
pd.concat()
```

---

# What is concat()?

`concat()` means:

```text
Concatenate
Combine
Stack
Join
```

multiple Pandas objects together.

Can combine:

```text
Series
DataFrames
```

---

# Syntax

```python
pd.concat(
    objs,
    axis=0,
    ignore_index=False
)
```

Where:

```text
objs         → Objects to combine
axis         → Direction
ignore_index → Reset index or not
```

---

# Internal Working

```text
DataFrame 1
     ↓
DataFrame 2
     ↓
DataFrame 3
     ↓
pd.concat()
     ↓
Single DataFrame
```

---

# 1. Vertical Concatenation (axis=0)

Most common use case.

---

## Example

### df1

```text
Name

Raj
Aman
```

### df2

```text
Name

Rohit
Karan
```

---

```python
df = pd.concat([df1, df2])
```

Output:

```text
Name

Raj
Aman
Rohit
Karan
```

---

# Visualization

```text
df1

Raj
Aman

   +
df2

Rohit
Karan

   =
Result

Raj
Aman
Rohit
Karan
```

---

# Why axis=0?

Axis meaning:

```text
axis=0 → Rows

axis=1 → Columns
```

When stacking rows:

```text
Vertical Direction
```

Hence:

```python
axis=0
```

---

# Default Behavior

```python
pd.concat([df1,df2])
```

is equivalent to:

```python
pd.concat(
    [df1,df2],
    axis=0
)
```

because:

```python
axis=0
```

is default.

---

# Real World Example

January Sales:

```text
Customer Sales

A 100
B 200
```

February Sales:

```text
Customer Sales

C 300
D 400
```

---

Combine:

```python
sales = pd.concat(
    [jan,feb]
)
```

Result:

```text
A 100
B 200
C 300
D 400
```

---

# 2. Horizontal Concatenation (axis=1)

Combines columns.

---

### df1

```text
Name

Raj
Aman
```

---

### df2

```text
Salary

50000
60000
```

---

```python
pd.concat(
    [df1,df2],
    axis=1
)
```

Output:

```text
Name Salary

Raj 50000
Aman 60000
```

---

# Visualization

```text
Name

Raj
Aman

     +

Salary

50000
60000

     =

Name Salary

Raj 50000
Aman 60000
```

---

# Why axis=1?

Columns are added side by side.

```text
Horizontal Direction
```

---

# Important Interview Question

Difference between:

```python
axis=0
```

and

```python
axis=1
```

| Axis   | Meaning         |
| ------ | --------------- |
| axis=0 | Combine Rows    |
| axis=1 | Combine Columns |

---

# Index Alignment in concat()

Very important interview topic.

---

### df1

```text
Index Name

0 Raj
1 Aman
```

---

### df2

```text
Index Salary

1 50000
2 60000
```

---

```python
pd.concat(
 [df1,df2],
 axis=1
)
```

Output:

```text
Index Name Salary

0 Raj   NaN
1 Aman 50000
2 NaN  60000
```

---

# Why?

Pandas aligns by:

```text
Index
```

not row position.

---

# Hidden Production Bug

Many beginners think:

```text
Row 1 + Row 1
```

Wrong.

Pandas performs:

```text
Index Matching
```

first.

---

# ignore_index=True

One of the most important parameters.

---

## Example

### df1

```text
0 Raj
1 Aman
```

---

### df2

```text
0 Rohit
1 Karan
```

---

```python
pd.concat(
 [df1,df2]
)
```

Output:

```text
0 Raj
1 Aman
0 Rohit
1 Karan
```

Duplicate indexes.

---

# Fix

```python
pd.concat(
 [df1,df2],
 ignore_index=True
)
```

Output:

```text
0 Raj
1 Aman
2 Rohit
3 Karan
```

---

# Why Use ignore_index?

After combining datasets:

```text
Clean Sequential Index
```

---

# Real ETL Usage

Combining:

```text
January.csv
February.csv
March.csv
```

Usually:

```python
pd.concat(
 files,
 ignore_index=True
)
```

---

# keys Parameter

Advanced interview topic.

Creates hierarchical index.

---

Example

```python
pd.concat(
 [df1,df2],
 keys=["Jan","Feb"]
)
```

Output:

```text
Jan 0 Raj
Jan 1 Aman

Feb 0 Rohit
Feb 1 Karan
```

---

# Why Use keys?

Keeps source information.

---

Example

```text
Which month did record come from?
```

Answer:

```text
Index Level
```

stores it.

---

# MultiIndex (Hierarchical Index)

Result of keys:

```text
Level 1 → Month

Level 2 → Original Index
```

Example:

```text
Jan 0
Jan 1
Feb 0
Feb 1
```

---

# names Parameter

Label MultiIndex levels.

```python
pd.concat(
 [df1,df2],
 keys=["Jan","Feb"],
 names=["Month"]
)
```

Output:

```text
Month

Jan
Feb
```

appears as index label.

---

# verify_integrity=True

Advanced feature.

---

Suppose:

```text
0 Raj
1 Aman

0 Rohit
1 Karan
```

Duplicate indexes exist.

---

```python
pd.concat(
 [df1,df2],
 verify_integrity=True
)
```

Output:

```text
ValueError
```

---

# Why Use?

Prevents accidental duplicate indexes.

Useful in production pipelines.

---

# join Parameter

Important.

Controls handling of unmatched columns.

---

## Outer Join (Default)

```python
pd.concat(
 [df1,df2],
 join="outer"
)
```

Keeps all columns.

---

Example

### df1

```text
Name Age
```

### df2

```text
Name Salary
```

Result:

```text
Name Age Salary
```

Missing values become:

```python
NaN
```

---

## Inner Join

```python
pd.concat(
 [df1,df2],
 join="inner"
)
```

Only common columns retained.

---

Output:

```text
Name
```

---

# Visual Understanding

Outer:

```text
All Columns
```

Inner:

```text
Intersection Only
```

---

# concat() with Series

---

### s1

```text
10
20
```

### s2

```text
30
40
```

---

```python
pd.concat(
 [s1,s2]
)
```

Output:

```text
10
20
30
40
```

---

# Convert Series to DataFrame

```python
pd.concat(
 [s1,s2],
 axis=1
)
```

Output:

```text
0  1

10 30
20 40
```

---

# concat vs append

Very common interview question.

---

Old:

```python
df1.append(df2)
```

---

Modern:

```python
pd.concat(
 [df1,df2]
)
```

---

# Important Update

```python
append()
```

has been removed in modern Pandas.

Use:

```python
concat()
```

instead.

---

# concat vs merge

Huge interview question.

---

## concat

Simply stacks data.

```text
Rows
or
Columns
```

---

## merge

Uses common key.

Like SQL JOIN.

Example:

```python
merge(
 on="CustomerID"
)
```

---

| concat              | merge           |
| ------------------- | --------------- |
| Stack data          | Join data       |
| No key required     | Key required    |
| Vertical/Horizontal | Relational join |

---

# Real World ETL Example

Suppose:

```text
sales_jan.csv
sales_feb.csv
sales_mar.csv
```

Read:

```python
jan = pd.read_csv(...)
feb = pd.read_csv(...)
mar = pd.read_csv(...)
```

Combine:

```python
sales = pd.concat(
 [jan,feb,mar],
 ignore_index=True
)
```

Most common ETL pattern.

---

# Performance Tips

Bad:

```python
result = pd.DataFrame()

for df in files:
    result = pd.concat(
        [result,df]
    )
```

Very slow.

---

Good:

```python
all_dfs = []

for df in files:
    all_dfs.append(df)

result = pd.concat(
    all_dfs
)
```

Much faster.

---

# Why?

Bad approach:

```text
Repeated Memory Allocation
Repeated Copying
```

---

Good approach:

```text
Single Concatenation
```

---

# Frequently Asked Interview Questions

### What is concat()?

Combines multiple Series/DataFrames.

---

### Difference Between axis=0 and axis=1?

axis=0:

```text
Combine Rows
```

axis=1:

```text
Combine Columns
```

---

### Why use ignore_index=True?

Creates new sequential indexes.

---

### What does keys do?

Creates hierarchical index.

---

### Difference Between concat and merge?

concat:

```text
Stack Data
```

merge:

```text
Join Data Using Keys
```

---

### Is append() recommended?

No.

Removed from modern Pandas.

Use:

```python
pd.concat()
```

---

# Quick Revision

```python
# Vertical
pd.concat(
 [df1,df2]
)

# Horizontal
pd.concat(
 [df1,df2],
 axis=1
)

# Reset Index
pd.concat(
 [df1,df2],
 ignore_index=True
)

# Hierarchical Index
pd.concat(
 [df1,df2],
 keys=["Jan","Feb"]
)

# Inner Join
pd.concat(
 [df1,df2],
 join="inner"
)
```
#  `merge()` in Pandas (SQL Joins in Pandas)

If `concat()` is used to **stack data**, then `merge()` is used to **join related data**.

This is one of the most important Pandas topics because it directly maps to SQL JOIN operations.

Real-world examples:

```text
Customers Table
Orders Table

Employees Table
Departments Table

Students Table
Marks Table
```

These datasets are usually stored separately and combined using:

```python
pd.merge()
```

---

# What is merge()?

`merge()` combines DataFrames based on common columns (keys).

Think:

```text
SQL JOIN
      =
Pandas merge()
```

---

# Why merge Exists?

Suppose:

### Customer Table

| CustomerID | Name  |
| ---------- | ----- |
| 101        | Raj   |
| 102        | Aman  |
| 103        | Rohit |

---

### Orders Table

| CustomerID | Amount |
| ---------- | ------ |
| 101        | 500    |
| 102        | 1000   |
| 104        | 2000   |

---

Need:

| CustomerID | Name | Amount |
| ---------- | ---- | ------ |
| 101        | Raj  | 500    |
| 102        | Aman | 1000   |

Use:

```python
pd.merge()
```

---

# Basic Syntax

```python
pd.merge(
    left_df,
    right_df,
    on="CustomerID"
)
```

---

# Internal Working

```text
Left Table
      ↓
Find Matching Keys
      ↓
Right Table
      ↓
Combine Matching Rows
      ↓
New DataFrame
```

---

# Important Parameters

```python
pd.merge(
    left,
    right,
    on=,
    how=,
    left_on=,
    right_on=
)
```

---

| Parameter | Purpose                   |
| --------- | ------------------------- |
| on        | Common column             |
| how       | Join type                 |
| left_on   | Left key                  |
| right_on  | Right key                 |
| suffixes  | Duplicate column handling |

---

# INNER JOIN

Most important join.

Also default join type.

---

## Example

### Customers

| CustomerID | Name  |
| ---------- | ----- |
| 101        | Raj   |
| 102        | Aman  |
| 103        | Rohit |

---

### Orders

| CustomerID | Amount |
| ---------- | ------ |
| 101        | 500    |
| 102        | 1000   |
| 104        | 2000   |

---

```python
pd.merge(
    customers,
    orders,
    on="CustomerID",
    how="inner"
)
```

Output:

| CustomerID | Name | Amount |
| ---------- | ---- | ------ |
| 101        | Raj  | 500    |
| 102        | Aman | 1000   |

---

# Rule

```text
Only Matching Keys
```

are kept.

---

# Visualization

```text
Customer Table

101
102
103

       ∩

Order Table

101
102
104

       =

101
102
```

---

# Interview Question

### Default join type?

```python
inner
```

---

# LEFT JOIN

Extremely common in production.

---

```python
pd.merge(
    customers,
    orders,
    on="CustomerID",
    how="left"
)
```

Output:

| CustomerID | Name  | Amount |
| ---------- | ----- | ------ |
| 101        | Raj   | 500    |
| 102        | Aman  | 1000   |
| 103        | Rohit | NaN    |

---

# Rule

```text
Keep ALL rows from Left Table
```

---

# Visualization

```text
LEFT TABLE

101
102
103

Keep Everything

101
102
103
```

---

# Why NaN Appears?

Customer:

```text
103
```

has no matching order.

Pandas fills:

```python
NaN
```

---

# Real World Usage

Customer Master Table:

```text
1 Million Customers
```

Orders:

```text
100K Customers Ordered
```

Need:

```text
All Customers
```

Use:

```python
how="left"
```

---

# RIGHT JOIN

Opposite of left join.

---

```python
pd.merge(
    customers,
    orders,
    on="CustomerID",
    how="right"
)
```

Output:

| CustomerID | Name | Amount |
| ---------- | ---- | ------ |
| 101        | Raj  | 500    |
| 102        | Aman | 1000   |
| 104        | NaN  | 2000   |

---

# Rule

```text
Keep ALL rows from Right Table
```

---

# OUTER JOIN

Very important interview topic.

---

```python
pd.merge(
    customers,
    orders,
    on="CustomerID",
    how="outer"
)
```

Output:

| CustomerID | Name  | Amount |
| ---------- | ----- | ------ |
| 101        | Raj   | 500    |
| 102        | Aman  | 1000   |
| 103        | Rohit | NaN    |
| 104        | NaN   | 2000   |

---

# Rule

```text
Keep Everything
```

---

# Visualization

```text
Customer

101
102
103

   ∪

Orders

101
102
104

   =

101
102
103
104
```

---

# Interview Question

Difference between:

```python
inner
```

and

```python
outer
```

Inner:

```text
Intersection
```

Outer:

```text
Union
```

---

# CROSS JOIN

Introduced in modern Pandas.

---

```python
pd.merge(
    df1,
    df2,
    how="cross"
)
```

---

Example

### df1

```text
A
B
```

### df2

```text
1
2
```

---

Output:

```text
A 1
A 2
B 1
B 2
```

---

# Formula

```text
Rows(df1) × Rows(df2)
```

---

Example

```text
1000 rows
1000 rows
```

Output:

```text
1,000,000 rows
```

Be careful.

---

# Merge on Different Column Names

Very common.

---

### Customers

| Cust_ID | Name |
| ------- | ---- |
| 101     | Raj  |

---

### Orders

| CustomerID | Amount |
| ---------- | ------ |
| 101        | 500    |

---

Cannot use:

```python
on=
```

because names differ.

---

Use:

```python
pd.merge(
    customers,
    orders,
    left_on="Cust_ID",
    right_on="CustomerID"
)
```

---

# Why left_on and right_on?

Allows joining columns with different names.

---

# Multiple Keys

Very common in enterprise projects.

---

Example

```text
CustomerID
OrderDate
```

together form unique record.

---

```python
pd.merge(
    df1,
    df2,
    on=[
      "CustomerID",
      "OrderDate"
    ]
)
```

---

# Duplicate Keys Problem

One of the most important production issues.

---

### Customers

| ID  | Name |
| --- | ---- |
| 101 | Raj  |
| 101 | Raj  |

---

### Orders

| ID  | Amount |
| --- | ------ |
| 101 | 500    |
| 101 | 700    |

---

Merge:

```python
pd.merge(
    customers,
    orders,
    on="ID"
)
```

Output:

```text
Raj 500
Raj 700
Raj 500
Raj 700
```

---

# Why?

Pandas creates:

```text
Cartesian Matching
```

---

Formula:

```text
2 × 2 = 4 rows
```

---

# Hidden Production Bug

Duplicate keys can explode dataset size.

Always check:

```python
df["ID"].duplicated().sum()
```

before merge.

---

# suffixes Parameter

Suppose:

### df1

```text
ID Name Salary
```

### df2

```text
ID Salary Bonus
```

---

Merge:

```python
pd.merge(
    df1,
    df2,
    on="ID"
)
```

Output:

```text
Salary_x
Salary_y
```

---

Custom names:

```python
pd.merge(
    df1,
    df2,
    on="ID",
    suffixes=(
      "_old",
      "_new"
    )
)
```

Output:

```text
Salary_old
Salary_new
```

---

# Indicator Parameter

Advanced interview topic.

---

```python
pd.merge(
    df1,
    df2,
    on="ID",
    how="outer",
    indicator=True
)
```

Output:

| ID  | _merge     |
| --- | ---------- |
| 101 | both       |
| 102 | left_only  |
| 103 | right_only |

---

# Why Useful?

Data reconciliation.

Examples:

```text
Customer Exists Here?
Missing There?
```

---

# Validate Parameter

Senior-level topic.

---

Example:

```python
pd.merge(
    df1,
    df2,
    on="ID",
    validate="one_to_one"
)
```

Checks relationship.

---

Available:

```text
one_to_one
one_to_many
many_to_one
many_to_many
```

---

# Why Important?

Detects accidental duplicate keys.

Prevents data corruption.

---

# merge vs concat

Huge interview question.

---

## concat

```text
Stack Data
```

Example:

```text
January
February
March
```

combine rows.

---

## merge

```text
Join Related Data
```

Example:

```text
Customers
Orders
```

join using key.

---

| concat              | merge      |
| ------------------- | ---------- |
| Stack               | Join       |
| No key needed       | Key needed |
| Vertical/Horizontal | Relational |
| Like UNION          | Like JOIN  |

---

# Performance Tips

---

## Optimize Datatypes

Bad:

```python
object
```

Good:

```python
category
```

for repeated values.

---

## Merge on Indexed Columns

Faster:

```python
df.set_index("ID")
```

then join.

---

## Remove Duplicates Before Merge

```python
drop_duplicates()
```

reduces explosion risk.

---

# Real ETL Example

### Customer Table

```text
CustomerID
Name
```

---

### Orders Table

```text
CustomerID
Amount
```

---

```python
final_df = pd.merge(
    customers,
    orders,
    on="CustomerID",
    how="left"
)
```

Most common production merge pattern.

---

# Frequently Asked Interview Questions

### Difference Between Inner and Left Join?

Inner:

```text
Matching rows only
```

Left:

```text
All rows from left table
```

---

### Default Join Type?

```python
inner
```

---

### Why Use left_on and right_on?

When join column names differ.

---

### What Causes Dataset Explosion?

Duplicate keys on both sides.

---

### Difference Between concat and merge?

concat stacks data.

merge joins data using keys.

---

### What Does indicator=True Do?

Adds source-tracking column.

---

### Why Use validate?

To verify relationship assumptions.

---

# Quick Revision

```python
# Inner
pd.merge(df1,df2,on="ID")

# Left
pd.merge(
 df1,df2,
 on="ID",
 how="left"
)

# Right
pd.merge(
 df1,df2,
 on="ID",
 how="right"
)

# Outer
pd.merge(
 df1,df2,
 on="ID",
 how="outer"
)

# Different names
pd.merge(
 df1,df2,
 left_on="Cust_ID",
 right_on="CustomerID"
)

# Multiple keys
pd.merge(
 df1,df2,
 on=["ID","Date"]
)

# Indicator
pd.merge(
 df1,df2,
 indicator=True
)
```
# `groupby()` in Pandas (Most Important Data Analysis Operation)

If `merge()` is the SQL JOIN of Pandas, then `groupby()` is the SQL **GROUP BY**.

This is one of the most important topics in Data Analysis, Business Intelligence, Data Engineering, and Machine Learning feature engineering.

Almost every reporting dashboard uses some form of:

```python
groupby()
```

---

# What is groupby()?

`groupby()` is used to:

```text
Split Data
    ↓
Apply Operation
    ↓
Combine Result
```

This is known as:

```text
Split → Apply → Combine
```

and is one of the most famous Pandas concepts.

---

# Why groupby Exists?

Suppose you have:

| Name  | Department | Salary |
| ----- | ---------- | ------ |
| Raj   | IT         | 50000  |
| Aman  | HR         | 40000  |
| Rohit | IT         | 60000  |
| Karan | HR         | 45000  |

Question:

```text
Average salary by department?
```

Without groupby:

Complicated loops.

With groupby:

```python
df.groupby("Department")["Salary"].mean()
```

Simple.

---

# Internal Working (Split-Apply-Combine)

Suppose:

```text
Department

IT
HR
IT
HR
```

---

## Step 1: Split

```text
IT Group

50000
60000
```

---

```text
HR Group

40000
45000
```

---

## Step 2: Apply

Example:

```python
mean()
```

IT:

```text
55000
```

HR:

```text
42500
```

---

## Step 3: Combine

Result:

| Department | Salary |
| ---------- | ------ |
| HR         | 42500  |
| IT         | 55000  |

---

# Basic Syntax

```python
df.groupby("Department")
```

Returns:

```python
DataFrameGroupBy
```

object.

Not actual data yet.

---

# Why?

Because Pandas is waiting for:

```text
Aggregation
Transformation
Filtering
```

operation.

---

# First GroupBy Example

```python
df.groupby(
    "Department"
)["Salary"].sum()
```

Output:

| Department | Salary |
| ---------- | ------ |
| HR         | 85000  |
| IT         | 110000 |

---

# Common Aggregations

---

## Sum

```python
df.groupby(
 "Department"
)["Salary"].sum()
```

---

## Mean

```python
df.groupby(
 "Department"
)["Salary"].mean()
```

---

## Min

```python
df.groupby(
 "Department"
)["Salary"].min()
```

---

## Max

```python
df.groupby(
 "Department"
)["Salary"].max()
```

---

## Count

```python
df.groupby(
 "Department"
)["Salary"].count()
```

Counts non-null values.

---

## Size

```python
df.groupby(
 "Department"
).size()
```

Counts rows.

---

# Interview Question

Difference between:

```python
count()
```

and

```python
size()
```

---

### count()

Ignores:

```python
NaN
```

---

### size()

Counts every row.

---

Example:

| Dept | Salary |
| ---- | ------ |
| IT   | 50000  |
| IT   | NaN    |

---

```python
count()
```

Output:

```text
1
```

---

```python
size()
```

Output:

```text
2
```

---

# Multiple Columns Grouping

Very common.

---

Example

```python
df.groupby(
 ["Department","City"]
)
["Salary"].sum()
```

Output:

```text
Department City

HR Delhi     40000
HR Mumbai    45000

IT Delhi     50000
IT Mumbai    60000
```

---

# MultiIndex Result

Notice:

```text
Department
     ↓
City
```

Multiple levels created.

Called:

```text
MultiIndex
```

---

# Converting MultiIndex to Normal DataFrame

Use:

```python
.reset_index()
```

Example:

```python
df.groupby(
 ["Department","City"]
)["Salary"].sum().reset_index()
```

Output:

| Department | City   | Salary |
| ---------- | ------ | ------ |
| HR         | Delhi  | 40000  |
| IT         | Mumbai | 60000  |

---

# Multiple Aggregations

Extremely important interview topic.

---

Example:

```python
df.groupby(
 "Department"
)["Salary"].agg(
 [
   "sum",
   "mean",
   "min",
   "max"
 ]
)
```

Output:

| Department | Sum    | Mean  | Min   | Max   |
| ---------- | ------ | ----- | ----- | ----- |
| HR         | 85000  | 42500 | 40000 | 45000 |
| IT         | 110000 | 55000 | 50000 | 60000 |

---

# Why agg()?

Allows multiple aggregations simultaneously.

---

# Named Aggregations

Modern Pandas approach.

```python
df.groupby(
 "Department"
).agg(
 TotalSalary=("Salary","sum"),
 AvgSalary=("Salary","mean"),
 MaxSalary=("Salary","max")
)
```

Output:

```text
Custom Column Names
```

Very clean.

---

# Grouping Multiple Columns

Example:

```python
df.groupby(
 "Department"
)[
 ["Salary","Bonus"]
].sum()
```

Output:

| Department | Salary | Bonus |
| ---------- | ------ | ----- |
| HR         | 85000  | 8000  |
| IT         | 110000 | 10000 |

---

# as_index=False

One of the most important interview topics.

---

Default:

```python
df.groupby(
 "Department"
).sum()
```

Output:

Department becomes index.

---

Use:

```python
df.groupby(
 "Department",
 as_index=False
).sum()
```

Output:

| Department | Salary |
| ---------- | ------ |
| HR         | 85000  |
| IT         | 110000 |

Department remains normal column.

---

# Why Use as_index=False?

Avoids:

```python
reset_index()
```

later.

---

# get_group()

Retrieve individual group.

---

Example:

```python
grp = df.groupby("Department")

grp.get_group("IT")
```

Output:

```text
Only IT employees
```

---

# Iterating Groups

```python
for name, group in df.groupby(
    "Department"
):
    print(name)
    print(group)
```

Output:

```text
HR Group
IT Group
```

---

# transform()

One of the most important advanced topics.

---

## Problem

Need department average salary for every employee.

---

Bad approach:

```text
Group
Merge
Join Back
```

---

Good approach:

```python
df["DeptAvg"] = (
 df.groupby(
   "Department"
 )["Salary"]
 .transform("mean")
)
```

Output:

| Name  | Department | Salary | DeptAvg |
| ----- | ---------- | ------ | ------- |
| Raj   | IT         | 50000  | 55000   |
| Rohit | IT         | 60000  | 55000   |

---

# Difference Between agg() and transform()

Huge interview question.

---

## agg()

Reduces rows.

Example:

```python
groupby().mean()
```

Result:

```text
1 row per group
```

---

## transform()

Keeps original rows.

Example:

```text
Same number of rows
```

---

# filter()

Advanced but important.

Used to keep/remove entire groups.

---

Example:

```python
df.groupby(
 "Department"
).filter(
 lambda x:
 len(x) > 2
)
```

Meaning:

```text
Keep departments
having more than 2 employees
```

---

# nunique()

Count unique values.

---

Example:

```python
df.groupby(
 "Department"
)["EmployeeID"]
.nunique()
```

Output:

```text
Unique employees per department
```

---

# value_counts()

Very useful.

---

Example:

```python
df["Department"]
.value_counts()
```

Output:

```text
IT 10
HR 5
Sales 3
```

Quick frequency table.

---

# GroupBy with Dates

Very common in business reporting.

---

Example:

```python
df.groupby(
 df["OrderDate"].dt.month
)["Sales"].sum()
```

Output:

```text
Monthly Sales
```

---

# Real-World Examples

---

## Total Sales By Month

```python
df.groupby(
 "Month"
)["Sales"].sum()
```

---

## Average Salary By Department

```python
df.groupby(
 "Department"
)["Salary"].mean()
```

---

## Orders Per Customer

```python
df.groupby(
 "CustomerID"
).size()
```

---

## Maximum Transaction By User

```python
df.groupby(
 "UserID"
)["Amount"].max()
```

---

# Hidden Production Problems

---

## Problem 1

Using:

```python
groupby().apply()
```

for simple aggregations.

Very slow.

---

Bad:

```python
groupby().apply(
 lambda x: x.sum()
)
```

---

Good:

```python
groupby().sum()
```

Much faster.

---

## Problem 2

Forgetting:

```python
reset_index()
```

after multi-column grouping.

Leads to confusing MultiIndexes.

---

## Problem 3

Grouping high-cardinality columns.

Example:

```text
CustomerID
```

with:

```text
10 Million Unique Values
```

Consumes huge memory.

---

# Frequently Asked Interview Questions

### What is Split-Apply-Combine?

Core concept behind groupby.

---

### Difference Between count() and size()?

count():

```text
Ignores NaN
```

size():

```text
Counts rows
```

---

### Difference Between agg() and transform()?

agg():

```text
Reduces rows
```

transform():

```text
Keeps original rows
```

---

### Why use as_index=False?

Keeps grouped columns as normal columns.

---

### What is MultiIndex?

Multiple index levels created after grouping by multiple columns.

---

### How do you remove MultiIndex?

```python
reset_index()
```

---

### What does filter() do?

Removes entire groups based on conditions.

---

### What does get_group() do?

Returns a specific group.

---

# Quick Revision

```python
# Basic
df.groupby("Dept")

# Sum
df.groupby("Dept")["Salary"].sum()

# Mean
df.groupby("Dept")["Salary"].mean()

# Multiple Aggregations
df.groupby("Dept")["Salary"].agg(
 ["sum","mean","max"]
)

# Multiple Columns
df.groupby(
 ["Dept","City"]
)["Salary"].sum()

# Reset Index
.reset_index()

# Transform
df.groupby(
 "Dept"
)["Salary"].transform("mean")

# Filter
df.groupby(
 "Dept"
).filter(
 lambda x: len(x) > 2
)

# Specific Group
grp.get_group("IT")
```
#  `apply()`, `map()`, `applymap()`, Vectorization vs Loops, Lambda Functions & Performance Optimization

This is one of the most misunderstood Pandas topics.

Many beginners use:

```python
for loop
```

for every transformation.

Experienced Pandas developers prefer:

```python
map()
apply()
vectorized operations
```

because they are cleaner, faster, and more scalable.

---

# Why Do We Need apply(), map(), applymap()?

Suppose:

```python
Name      Salary

Raj       50000
Aman      60000
Rohit     70000
```

Need:

```text
Add 10% bonus
Convert names to uppercase
Calculate tax
Create custom columns
```

Instead of loops, Pandas provides transformation functions.

---

# Transformation Hierarchy

```text
map()
    ↓
Series Only

apply()
    ↓
Series + DataFrame

applymap()
    ↓
Entire DataFrame
(Element Wise)
```

---

# 1. map()

Most commonly used on a Series.

---

## What is map()?

Applies transformation to each element of a Series.

---

### Example

```python
s = pd.Series([1,2,3,4])

s.map(lambda x: x*2)
```

Output:

```text
0 2
1 4
2 6
3 8
```

---

# Internal Working

```text
1 → 2
2 → 4
3 → 6
4 → 8
```

Function applied to every value.

---

# Mapping Using Dictionary

Very important interview topic.

---

Example

```python
df["Gender"]
```

```text
M
F
M
F
```

Convert:

```python
gender_map = {
    "M":"Male",
    "F":"Female"
}

df["Gender"].map(gender_map)
```

Output:

```text
Male
Female
Male
Female
```

---

# Why Dictionary Mapping?

Fast.

Readable.

Common in ETL.

---

# Hidden Production Example

Source System:

```text
1
2
3
```

Need:

```text
Pending
Approved
Rejected
```

Use:

```python
status_map = {
    1:"Pending",
    2:"Approved",
    3:"Rejected"
}

df["Status"].map(status_map)
```

---

# Missing Mapping Values

Example

```python
s = pd.Series([1,2,3,4])

mapping = {
    1:"A",
    2:"B"
}
```

---

```python
s.map(mapping)
```

Output:

```text
A
B
NaN
NaN
```

---

# Why?

Keys:

```text
3
4
```

not found.

Pandas inserts:

```python
NaN
```

---

# Interview Question

### map() works on?

```text
Series Only
```

---

# 2. apply()

Most powerful transformation function.

Works on:

```text
Series
DataFrame
```

---

# Series apply()

Example

```python
s = pd.Series([1,2,3])

s.apply(
 lambda x: x**2
)
```

Output:

```text
1
4
9
```

---

# Why Use apply Instead of map?

`apply()` is more flexible.

Can execute complex functions.

---

# Example

```python
def classify_age(age):

    if age < 18:
        return "Minor"

    return "Adult"
```

---

```python
df["Age"].apply(
 classify_age
)
```

Output:

```text
Adult
Adult
Minor
```

---

# DataFrame apply()

Important interview topic.

---

Example

```python
df
```

```text
Math Science

90   80
70   60
```

---

## Column-Wise Apply

```python
df.apply(np.sum)
```

Output:

```text
Math       160
Science    140
```

---

# Why?

Default:

```python
axis=0
```

means:

```text
Column Wise
```

---

# Row-Wise Apply

```python
df.apply(
 np.sum,
 axis=1
)
```

Output:

```text
170
130
```

---

# Why?

```python
axis=1
```

means:

```text
Row Wise
```

---

# Interview Question

Difference Between:

```python
axis=0
```

and

```python
axis=1
```

---

axis=0:

```text
Columns
```

---

axis=1:

```text
Rows
```

---

# Creating New Columns Using apply()

Very common.

---

Example

```python
df["FullName"] = (
    df["FirstName"]
    +
    " "
    +
    df["LastName"]
)
```

Vectorized.

---

Using apply:

```python
df["FullName"] = (
 df.apply(
  lambda row:
  row["FirstName"]
  + " "
  + row["LastName"],
  axis=1
 )
)
```

Output:

```text
Raj Sharma
Aman Gupta
```

---

# When Row-Wise Apply Is Needed?

When multiple columns participate.

---

Example

```python
Tax depends on:
Salary
Age
Department
```

Need row context.

---

# 3. applymap()

Applies function to every cell.

---

Example

```python
df
```

```text
1 2
3 4
```

---

```python
df.applymap(
 lambda x: x*10
)
```

Output:

```text
10 20
30 40
```

---

# Internal Working

```text
1 → 10
2 → 20
3 → 30
4 → 40
```

Every cell processed.

---

# Difference Between apply and applymap

---

## apply()

Works on:

```text
Row
Column
Series
```

---

## applymap()

Works on:

```text
Individual Cells
```

---

# Visual Comparison

```text
apply()

Column A
Column B

Process whole column
```

---

```text
applymap()

Cell by Cell
```

---

# Vectorization vs Apply vs Loop

MOST IMPORTANT INTERVIEW TOPIC.

---

# Method 1: Loop

```python
result = []

for x in df["Salary"]:
    result.append(x*2)
```

Slow.

---

# Method 2: apply()

```python
df["Salary"].apply(
 lambda x:x*2
)
```

Better.

---

# Method 3: Vectorization

```python
df["Salary"] * 2
```

Best.

---

# Performance Ranking

```text
Vectorization
      ↓
Fastest

map()
      ↓

apply()
      ↓

for loop
      ↓

iterrows()
      ↓

Slowest
```

---

# Why Vectorization Is Fast?

Because operations happen on:

```text
NumPy Arrays
```

using optimized C code.

---

# Hidden Production Rule

Always ask:

```text
Can this be vectorized?
```

before using apply.

---

Bad:

```python
df["Salary"].apply(
 lambda x:x*2
)
```

---

Good:

```python
df["Salary"] * 2
```

---

# Lambda Functions

Frequently used with apply.

---

## What is Lambda?

Anonymous function.

---

Example

Traditional:

```python
def square(x):
    return x*x
```

---

Lambda:

```python
lambda x:x*x
```

Same output.

---

# Examples

```python
df["Age"].apply(
 lambda x:x+1
)
```

---

```python
df["Name"].apply(
 lambda x:x.upper()
)
```

---

```python
df["Salary"].apply(
 lambda x:x*1.1
)
```

---

# String Operations (Avoid apply)

Bad:

```python
df["Name"].apply(
 lambda x:x.upper()
)
```

---

Good:

```python
df["Name"].str.upper()
```

Faster.

---

# Datetime Operations (Avoid apply)

Bad:

```python
df["Date"].apply(
 lambda x:x.year
)
```

---

Good:

```python
df["Date"].dt.year
```

---

# Numerical Operations (Avoid apply)

Bad:

```python
df["Salary"].apply(
 lambda x:x*2
)
```

---

Good:

```python
df["Salary"] * 2
```

---

# Hidden Production Problems

## Problem 1

Using:

```python
iterrows()
```

for millions of rows.

Very slow.

---

Bad:

```python
for idx,row in df.iterrows():
    ...
```

---

## Problem 2

Using apply() when vectorization exists.

---

Bad:

```python
apply(lambda x:x+1)
```

---

Good:

```python
+1
```

---

## Problem 3

Using applymap() on huge DataFrames.

Processes every cell individually.

Can be expensive.

---

# Frequently Asked Interview Questions

### Difference Between map() and apply()?

map():

```text
Series only
```

apply():

```text
Series + DataFrame
```

---

### Difference Between apply() and applymap()?

apply():

```text
Row/Column
```

applymap():

```text
Cell by Cell
```

---

### Which Is Faster?

```text
Vectorization
```

---

### Why Avoid iterrows()?

Very slow for large datasets.

---

### What Is Lambda?

Anonymous one-line function.

---

### When Use axis=1?

Row-wise operations.

---

### When Use axis=0?

Column-wise operations.

---

# Real Interview Scenario

**Question:**

Multiply Salary by 2.

Options:

```python
for loop
apply()
vectorization
```

Best Answer:

```python
df["Salary"] * 2
```

Because:

```text
Fastest
Most Readable
Most Memory Efficient
```

---

# Quick Revision

```python
# map
s.map(
 {"M":"Male","F":"Female"}
)

# apply
df["Age"].apply(
 lambda x:x+1
)

# DataFrame apply
df.apply(
 np.sum
)

# Row-wise
df.apply(
 func,
 axis=1
)

# applymap
df.applymap(
 lambda x:x*10
)

# Vectorized
df["Salary"] * 2
```

# `apply()`, `map()`, `applymap()`, Vectorization vs Loops, Lambda Functions & Performance Optimization

This is one of the most misunderstood Pandas topics.

Many beginners use:

```python
for loop
```

for every transformation.

Experienced Pandas developers prefer:

```python
map()
apply()
vectorized operations
```

because they are cleaner, faster, and more scalable.

---

# Why Do We Need apply(), map(), applymap()?

Suppose:

```python
Name      Salary

Raj       50000
Aman      60000
Rohit     70000
```

Need:

```text
Add 10% bonus
Convert names to uppercase
Calculate tax
Create custom columns
```

Instead of loops, Pandas provides transformation functions.

---

# Transformation Hierarchy

```text
map()
    ↓
Series Only

apply()
    ↓
Series + DataFrame

applymap()
    ↓
Entire DataFrame
(Element Wise)
```

---

# 1. map()

Most commonly used on a Series.

---

## What is map()?

Applies transformation to each element of a Series.

---

### Example

```python
s = pd.Series([1,2,3,4])

s.map(lambda x: x*2)
```

Output:

```text
0 2
1 4
2 6
3 8
```

---

# Internal Working

```text
1 → 2
2 → 4
3 → 6
4 → 8
```

Function applied to every value.

---

# Mapping Using Dictionary

Very important interview topic.

---

Example

```python
df["Gender"]
```

```text
M
F
M
F
```

Convert:

```python
gender_map = {
    "M":"Male",
    "F":"Female"
}

df["Gender"].map(gender_map)
```

Output:

```text
Male
Female
Male
Female
```

---

# Why Dictionary Mapping?

Fast.

Readable.

Common in ETL.

---

# Hidden Production Example

Source System:

```text
1
2
3
```

Need:

```text
Pending
Approved
Rejected
```

Use:

```python
status_map = {
    1:"Pending",
    2:"Approved",
    3:"Rejected"
}

df["Status"].map(status_map)
```

---

# Missing Mapping Values

Example

```python
s = pd.Series([1,2,3,4])

mapping = {
    1:"A",
    2:"B"
}
```

---

```python
s.map(mapping)
```

Output:

```text
A
B
NaN
NaN
```

---

# Why?

Keys:

```text
3
4
```

not found.

Pandas inserts:

```python
NaN
```

---

# Interview Question

### map() works on?

```text
Series Only
```

---

# 2. apply()

Most powerful transformation function.

Works on:

```text
Series
DataFrame
```

---

# Series apply()

Example

```python
s = pd.Series([1,2,3])

s.apply(
 lambda x: x**2
)
```

Output:

```text
1
4
9
```

---

# Why Use apply Instead of map?

`apply()` is more flexible.

Can execute complex functions.

---

# Example

```python
def classify_age(age):

    if age < 18:
        return "Minor"

    return "Adult"
```

---

```python
df["Age"].apply(
 classify_age
)
```

Output:

```text
Adult
Adult
Minor
```

---

# DataFrame apply()

Important interview topic.

---

Example

```python
df
```

```text
Math Science

90   80
70   60
```

---

## Column-Wise Apply

```python
df.apply(np.sum)
```

Output:

```text
Math       160
Science    140
```

---

# Why?

Default:

```python
axis=0
```

means:

```text
Column Wise
```

---

# Row-Wise Apply

```python
df.apply(
 np.sum,
 axis=1
)
```

Output:

```text
170
130
```

---

# Why?

```python
axis=1
```

means:

```text
Row Wise
```

---

# Interview Question

Difference Between:

```python
axis=0
```

and

```python
axis=1
```

---

axis=0:

```text
Columns
```

---

axis=1:

```text
Rows
```

---

# Creating New Columns Using apply()

Very common.

---

Example

```python
df["FullName"] = (
    df["FirstName"]
    +
    " "
    +
    df["LastName"]
)
```

Vectorized.

---

Using apply:

```python
df["FullName"] = (
 df.apply(
  lambda row:
  row["FirstName"]
  + " "
  + row["LastName"],
  axis=1
 )
)
```

Output:

```text
Raj Sharma
Aman Gupta
```

---

# When Row-Wise Apply Is Needed?

When multiple columns participate.

---

Example

```python
Tax depends on:
Salary
Age
Department
```

Need row context.

---

# 3. applymap()

Applies function to every cell.

---

Example

```python
df
```

```text
1 2
3 4
```

---

```python
df.applymap(
 lambda x: x*10
)
```

Output:

```text
10 20
30 40
```

---

# Internal Working

```text
1 → 10
2 → 20
3 → 30
4 → 40
```

Every cell processed.

---

# Difference Between apply and applymap

---

## apply()

Works on:

```text
Row
Column
Series
```

---

## applymap()

Works on:

```text
Individual Cells
```

---

# Visual Comparison

```text
apply()

Column A
Column B

Process whole column
```

---

```text
applymap()

Cell by Cell
```

---

# Vectorization vs Apply vs Loop

MOST IMPORTANT INTERVIEW TOPIC.

---

# Method 1: Loop

```python
result = []

for x in df["Salary"]:
    result.append(x*2)
```

Slow.

---

# Method 2: apply()

```python
df["Salary"].apply(
 lambda x:x*2
)
```

Better.

---

# Method 3: Vectorization

```python
df["Salary"] * 2
```

Best.

---

# Performance Ranking

```text
Vectorization
      ↓
Fastest

map()
      ↓

apply()
      ↓

for loop
      ↓

iterrows()
      ↓

Slowest
```

---

# Why Vectorization Is Fast?

Because operations happen on:

```text
NumPy Arrays
```

using optimized C code.

---

# Hidden Production Rule

Always ask:

```text
Can this be vectorized?
```

before using apply.

---

Bad:

```python
df["Salary"].apply(
 lambda x:x*2
)
```

---

Good:

```python
df["Salary"] * 2
```

---

# Lambda Functions

Frequently used with apply.

---

## What is Lambda?

Anonymous function.

---

Example

Traditional:

```python
def square(x):
    return x*x
```

---

Lambda:

```python
lambda x:x*x
```

Same output.

---

# Examples

```python
df["Age"].apply(
 lambda x:x+1
)
```

---

```python
df["Name"].apply(
 lambda x:x.upper()
)
```

---

```python
df["Salary"].apply(
 lambda x:x*1.1
)
```

---

# String Operations (Avoid apply)

Bad:

```python
df["Name"].apply(
 lambda x:x.upper()
)
```

---

Good:

```python
df["Name"].str.upper()
```

Faster.

---

# Datetime Operations (Avoid apply)

Bad:

```python
df["Date"].apply(
 lambda x:x.year
)
```

---

Good:

```python
df["Date"].dt.year
```

---

# Numerical Operations (Avoid apply)

Bad:

```python
df["Salary"].apply(
 lambda x:x*2
)
```

---

Good:

```python
df["Salary"] * 2
```

---

# Hidden Production Problems

## Problem 1

Using:

```python
iterrows()
```

for millions of rows.

Very slow.

---

Bad:

```python
for idx,row in df.iterrows():
    ...
```

---

## Problem 2

Using apply() when vectorization exists.

---

Bad:

```python
apply(lambda x:x+1)
```

---

Good:

```python
+1
```

---

## Problem 3

Using applymap() on huge DataFrames.

Processes every cell individually.

Can be expensive.

---

# Frequently Asked Interview Questions

### Difference Between map() and apply()?

map():

```text
Series only
```

apply():

```text
Series + DataFrame
```

---

### Difference Between apply() and applymap()?

apply():

```text
Row/Column
```

applymap():

```text
Cell by Cell
```

---

### Which Is Faster?

```text
Vectorization
```

---

### Why Avoid iterrows()?

Very slow for large datasets.

---

### What Is Lambda?

Anonymous one-line function.

---

### When Use axis=1?

Row-wise operations.

---

### When Use axis=0?

Column-wise operations.

---

# Real Interview Scenario

**Question:**

Multiply Salary by 2.

Options:

```python
for loop
apply()
vectorization
```

Best Answer:

```python
df["Salary"] * 2
```

Because:

```text
Fastest
Most Readable
Most Memory Efficient
```

---

# Quick Revision

```python
# map
s.map(
 {"M":"Male","F":"Female"}
)

# apply
df["Age"].apply(
 lambda x:x+1
)

# DataFrame apply
df.apply(
 np.sum
)

# Row-wise
df.apply(
 func,
 axis=1
)

# applymap
df.applymap(
 lambda x:x*10
)

# Vectorized
df["Salary"] * 2
```
# Pandas Window Functions Deep Dive (`rolling()`, `expanding()`, `ewm()`, Moving Averages, Time-Series Analytics & Financial Calculations)

Window Functions are among the most powerful features of Pandas.

They are heavily used in:

```text
Stock Market Analysis
Financial Modeling
Forecasting
Time Series Analysis
IoT Analytics
Sales Trends
Machine Learning Feature Engineering
```

Many interviewers ask about:

```python
rolling()
expanding()
ewm()
```

because they demonstrate real-world analytical skills.

---

# What Are Window Functions?

Window functions perform calculations on a group of neighboring rows.

Unlike:

```python
sum()
mean()
```

which aggregate the entire column,

Window Functions calculate values over a moving subset (window) of data.

---

# Example Dataset

```text
Day   Sales

1     100
2     120
3     140
4     160
5     180
```

Question:

```text
Average sales of last 3 days?
```

This is a window calculation.

---

# Types of Window Functions

Pandas provides:

```text
Rolling Window
Expanding Window
Exponentially Weighted Window
```

---

# 1. Rolling Window

Most important window function.

---

## What is Rolling?

A fixed-size moving window.

Example:

```python
s.rolling(3)
```

means:

```text
Take 3 rows
Calculate
Move Forward
Repeat
```

---

# Visual Understanding

Data:

```text
10
20
30
40
50
```

Window Size:

```python
rolling(3)
```

Windows formed:

```text
[10,20,30]

[20,30,40]

[30,40,50]
```

---

# Rolling Mean

Most common use case.

```python
s.rolling(3).mean()
```

Output:

```text
NaN
NaN
20
30
40
```

---

# Why NaN?

Need:

```text
3 observations
```

before calculating mean.

---

# Internal Calculation

For:

```text
10
20
30
```

Mean:

```text
(10+20+30)/3

= 20
```

---

For:

```text
20
30
40
```

Mean:

```text
30
```

---

# Rolling Sum

```python
s.rolling(3).sum()
```

Output:

```text
NaN
NaN
60
90
120
```

---

# Rolling Max

```python
s.rolling(3).max()
```

Output:

```text
NaN
NaN
30
40
50
```

---

# Rolling Min

```python
s.rolling(3).min()
```

Output:

```text
NaN
NaN
10
20
30
```

---

# Rolling Standard Deviation

Very important in finance.

```python
s.rolling(3).std()
```

Used for:

```text
Volatility
Risk Analysis
Anomaly Detection
```

---

# min_periods Parameter

Advanced interview topic.

Default:

```text
Need full window
```

---

Example:

```python
s.rolling(
    window=3,
    min_periods=1
).mean()
```

Output:

```text
10
15
20
30
40
```

---

# Why?

Calculations begin immediately.

---

# center=True

By default:

```text
Window aligned to right
```

---

Use:

```python
s.rolling(
 3,
 center=True
)
```

Window centered around current row.

Useful for signal processing.

---

# Time-Based Rolling Window

Very important.

---

Dataset:

```text
Date       Sales

2024-01-01 100
2024-01-02 120
2024-01-05 130
```

---

Use:

```python
df.rolling("7D").mean()
```

Meaning:

```text
Last 7 Days
```

not:

```text
Last 7 Rows
```

---

# Production Example

```python
stock["Price"].rolling(20).mean()
```

Calculates:

```text
20-Day Moving Average
```

Used by traders worldwide.

---

# Moving Average

One of the most common analytical techniques.

---

# What Is Moving Average?

Average of recent observations.

Reduces noise.

Highlights trend.

---

Example

Sales:

```text
100
200
50
300
150
```

Raw data fluctuates heavily.

---

Moving average:

```python
rolling(3).mean()
```

creates smoother trend.

---

# Why Businesses Use It?

```text
Revenue Trends
Demand Forecasting
Stock Prices
Weather Analysis
```

---

# 2. Expanding Window

Second major window function.

---

# What Is Expanding?

Uses:

```text
Current Row
+
All Previous Rows
```

---

Example

```python
s.expanding()
```

---

# Visual

Data:

```text
10
20
30
40
```

Windows:

```text
[10]

[10,20]

[10,20,30]

[10,20,30,40]
```

---

# Expanding Mean

```python
s.expanding().mean()
```

Output:

```text
10
15
20
25
```

---

# Internal Calculation

Row 1:

```text
10
```

Mean:

```text
10
```

---

Row 2:

```text
10+20

=30

30/2

=15
```

---

Row 3:

```text
10+20+30

=60

60/3

=20
```

---

# Expanding Sum

```python
s.expanding().sum()
```

Output:

```text
10
30
60
100
```

---

# Difference Between Rolling and Expanding

| Feature     | Rolling    | Expanding     |
| ----------- | ---------- | ------------- |
| Window Size | Fixed      | Growing       |
| Past Data   | Limited    | All Previous  |
| Memory      | Small      | Increasing    |
| Common Use  | Moving Avg | Running Total |

---

# Interview Question

Difference between:

```python
rolling(3)
```

and

```python
expanding()
```

---

Rolling:

```text
Fixed Window
```

Expanding:

```text
Growing Window
```

---

# 3. Exponentially Weighted Moving Window (EWM)

Most advanced window function.

Frequently used in:

```text
Finance
Trading
Forecasting
Signal Processing
```

---

# Problem With Rolling Mean

Rolling gives equal importance.

Example:

```text
100
200
300
```

Weights:

```text
33%
33%
33%
```

---

But recent values are usually more important.

---

# EWM Solution

Recent observations receive higher weight.

Older observations receive lower weight.

---

# Syntax

```python
s.ewm(
 span=3
).mean()
```

---

# Internal Concept

Weights:

```text
Newest Value → Highest Weight

Older Value → Lower Weight

Oldest Value → Lowest Weight
```

---

# Why Important?

Suppose stock prices:

```text
100
110
120
500
```

Recent change matters more.

EWM reacts faster.

---

# Example

```python
s = pd.Series(
 [100,120,140,160]
)

s.ewm(
 span=2
).mean()
```

Produces exponentially weighted averages.

---

# Key Parameters

---

## span

Most common.

```python
ewm(span=5)
```

Controls smoothing.

---

Small span:

```text
React Faster
```

---

Large span:

```text
Smoother Curve
```

---

## alpha

Direct smoothing factor.

```python
ewm(alpha=0.3)
```

---

Formula:

```text
0 < alpha ≤ 1
```

---

Higher alpha:

```text
More Weight To Recent Data
```

---

# Financial Indicators

---

## EMA (Exponential Moving Average)

Very common.

```python
price.ewm(
 span=20
).mean()
```

20-day EMA.

---

## MACD

Built using EMAs.

Common stock trading indicator.

---

## Volatility Measures

Financial risk calculations often use:

```python
rolling().std()
```

and

```python
ewm()
```

---

# Cumulative Functions

Often grouped with window functions.

---

# cumsum()

Running total.

```python
s.cumsum()
```

---

Example:

```text
10
20
30
```

Output:

```text
10
30
60
```

---

# cumprod()

Running multiplication.

```python
s.cumprod()
```

---

Example:

```text
2
3
4
```

Output:

```text
2
6
24
```

---

# cummax()

Running maximum.

```python
s.cummax()
```

---

Example:

```text
10
5
20
15
```

Output:

```text
10
10
20
20
```

---

# cummin()

Running minimum.

```python
s.cummin()
```

---

Output:

```text
10
5
5
5
```

---

# Real World Analytics Examples

---

# Running Revenue

```python
df["Revenue"].cumsum()
```

Shows cumulative revenue.

---

# Rolling Sales Average

```python
df["Sales"].rolling(
 7
).mean()
```

Weekly trend.

---

# Stock Price Trend

```python
stock["Close"].rolling(
 20
).mean()
```

20-day moving average.

---

# Exponential Trend

```python
stock["Close"].ewm(
 span=20
).mean()
```

EMA.

---

# Group-wise Window Functions

Advanced interview topic.

---

Example:

```python
df.groupby(
 "Department"
)["Salary"]
.cumsum()
```

Output:

```text
Running salary total
within each department
```

---

# Hidden Production Problems

---

## Problem 1

Using rolling on unsorted dates.

Bad:

```python
df.rolling(7)
```

without sorting.

---

Good:

```python
df.sort_values("Date")
```

first.

---

## Problem 2

Huge window sizes.

Example:

```python
rolling(100000)
```

Consumes memory.

---

## Problem 3

Using rolling for forecasting.

Rolling is descriptive.

Not predictive.

---

# Frequently Asked Interview Questions

### What is a rolling window?

Fixed-size moving window.

---

### Difference between rolling and expanding?

Rolling:

```text
Fixed Window
```

Expanding:

```text
Growing Window
```

---

### Why does rolling mean produce NaN?

Insufficient observations.

---

### What is min_periods?

Minimum observations required.

---

### What is EWM?

Exponentially weighted moving calculations.

---

### Why use EWM instead of rolling?

Recent values receive higher importance.

---

### What does cumsum() do?

Running total.

---

### What does cummax() do?

Running maximum.

---

### Why sort dates before rolling?

Window calculations require correct sequence.

---

# Quick Revision

```python
# Rolling Mean
s.rolling(3).mean()

# Rolling Sum
s.rolling(3).sum()

# Rolling Std
s.rolling(3).std()

# Expanding Mean
s.expanding().mean()

# Expanding Sum
s.expanding().sum()

# EWM
s.ewm(span=5).mean()

# Running Total
s.cumsum()

# Running Max
s.cummax()

# Running Min
s.cummin()

# Running Product
s.cumprod()
```
# Pivot Tables, `pivot()`, `pivot_table()`, `crosstab()`, Reshaping Data & Excel-Style Reporting

Pivot Tables are among the most powerful features in Pandas.

If you've used Excel Pivot Tables, Pandas provides similar functionality but with far more flexibility.

Real-world uses:

```text
Sales Dashboards
Revenue Reports
Business Intelligence
HR Analytics
Financial Reports
KPI Tracking
```

Many companies generate management reports using:

```python
pivot()
pivot_table()
crosstab()
```

---

# Why Pivot Tables Exist?

Suppose you have:

| Employee | Department | Salary |
| -------- | ---------- | ------ |
| Raj      | IT         | 50000  |
| Aman     | HR         | 40000  |
| Rohit    | IT         | 60000  |
| Karan    | HR         | 45000  |

Question:

```text
Department-wise Salary Summary?
```

Instead of:

```python
groupby()
```

Pivot tables provide a report-like structure.

---

# What is Reshaping?

Reshaping means:

```text
Change Data Layout
Without Changing Data
```

Example:

From:

```text
Long Format
```

To:

```text
Wide Format
```

---

# Long Format

| Month | Sales |
| ----- | ----- |
| Jan   | 100   |
| Feb   | 200   |
| Mar   | 300   |

---

# Wide Format

| Jan | Feb | Mar |
| --- | --- | --- |
| 100 | 200 | 300 |

---

# What is pivot()?

`pivot()` reshapes data.

It converts:

```text
Rows
    ↓
Columns
```

---

# Syntax

```python
df.pivot(
    index=,
    columns=,
    values=
)
```

---

# Example

Dataset:

| Date | Product | Sales |
| ---- | ------- | ----- |
| Jan  | A       | 100   |
| Jan  | B       | 200   |
| Feb  | A       | 150   |
| Feb  | B       | 300   |

---

```python
df.pivot(
    index="Date",
    columns="Product",
    values="Sales"
)
```

Output:

| Date | A   | B   |
| ---- | --- | --- |
| Jan  | 100 | 200 |
| Feb  | 150 | 300 |

---

# Internal Working

```text
Date → Rows

Product → Columns

Sales → Values
```

---

# Visualization

Original:

```text
Jan A 100
Jan B 200
Feb A 150
Feb B 300
```

---

Pivoted:

```text
       A    B

Jan   100  200
Feb   150  300
```

---

# Important Rule of pivot()

### Duplicate Combinations Not Allowed

Example:

| Date | Product | Sales |
| ---- | ------- | ----- |
| Jan  | A       | 100   |
| Jan  | A       | 200   |

---

Now:

```python
df.pivot(...)
```

Error:

```text
ValueError:
Index contains duplicate entries
```

---

# Why?

Pandas doesn't know whether:

```text
100
```

or

```text
200
```

should be placed inside the cell.

---

# Interview Question

### When should you use pivot()?

When:

```text
Unique combinations exist.
```

No duplicates.

---

# What is pivot_table()?

Enhanced version of:

```python
pivot()
```

Supports duplicates.

Supports aggregation.

Much more powerful.

---

# Syntax

```python
df.pivot_table(
    index=,
    columns=,
    values=,
    aggfunc=
)
```

---

# Example

Dataset:

| Date | Product | Sales |
| ---- | ------- | ----- |
| Jan  | A       | 100   |
| Jan  | A       | 200   |
| Jan  | B       | 300   |

---

```python
df.pivot_table(
    index="Date",
    columns="Product",
    values="Sales",
    aggfunc="sum"
)
```

Output:

| Date | A   | B   |
| ---- | --- | --- |
| Jan  | 300 | 300 |

---

# Why No Error?

Because:

```python
aggfunc="sum"
```

combines duplicates.

---

# Most Common aggfunc Values

---

## Sum

```python
aggfunc="sum"
```

---

## Mean

```python
aggfunc="mean"
```

Default.

---

## Count

```python
aggfunc="count"
```

---

## Min

```python
aggfunc="min"
```

---

## Max

```python
aggfunc="max"
```

---

## Multiple Aggregations

Very common interview topic.

---

```python
df.pivot_table(
    index="Department",
    values="Salary",
    aggfunc=[
      "sum",
      "mean",
      "max"
    ]
)
```

Output:

| Dept | Sum    | Mean  | Max   |
| ---- | ------ | ----- | ----- |
| HR   | 85000  | 42500 | 45000 |
| IT   | 110000 | 55000 | 60000 |

---

# Multiple Value Columns

```python
df.pivot_table(
    index="Department",
    values=[
      "Salary",
      "Bonus"
    ],
    aggfunc="sum"
)
```

Output:

```text
Salary Bonus
```

both summarized.

---

# Multiple Indexes

Advanced reporting.

---

```python
df.pivot_table(
    index=[
      "Department",
      "City"
    ],
    values="Salary",
    aggfunc="sum"
)
```

Output:

```text
Department
      ↓
City
```

Creates MultiIndex.

---

# Multiple Columns

Example:

```python
df.pivot_table(
 index="Department",
 columns="Gender",
 values="Salary"
)
```

Output:

| Department | Male  | Female |
| ---------- | ----- | ------ |
| HR         | 40000 | 45000  |
| IT         | 55000 | 60000  |

---

# fill_value

Very important.

---

Without:

```python
NaN
```

appears.

---

Example:

```python
df.pivot_table(
 index="Department",
 columns="Gender",
 values="Salary",
 fill_value=0
)
```

Output:

```text
Missing Values → 0
```

---

# margins=True

One of the most useful reporting features.

---

Example:

```python
df.pivot_table(
 index="Department",
 values="Salary",
 aggfunc="sum",
 margins=True
)
```

Output:

| Department | Salary |
| ---------- | ------ |
| HR         | 85000  |
| IT         | 110000 |
| All        | 195000 |

---

# Why Use margins?

Creates:

```text
Grand Total
```

like Excel Pivot Tables.

---

# Custom Margin Name

```python
df.pivot_table(
 margins=True,
 margins_name="Total"
)
```

Output:

```text
Total
```

instead of:

```text
All
```

---

# MultiIndex Columns

Very important interview topic.

---

Example:

```python
df.pivot_table(
 index="Department",
 values="Salary",
 aggfunc=[
   "sum",
   "mean"
 ]
)
```

Output:

```text
       sum      mean
       Salary   Salary
```

Two-level columns.

---

# Flattening MultiIndex Columns

Common production task.

---

```python
pivot.columns = [
 "_".join(col)
 for col in pivot.columns
]
```

Output:

```text
sum_salary
mean_salary
```

---

# Crosstab

One of the most important analytical tools.

---

# What is crosstab()?

Creates frequency tables.

Equivalent to:

```text
Pivot Table For Counts
```

---

Example

| Gender | Department |
| ------ | ---------- |
| M      | IT         |
| F      | HR         |
| M      | IT         |

---

```python
pd.crosstab(
    df["Gender"],
    df["Department"]
)
```

Output:

| Gender | HR | IT |
| ------ | -- | -- |
| F      | 1  | 0  |
| M      | 0  | 2  |

---

# Why Use Crosstab?

Common in:

```text
Survey Analysis
Market Research
Demographics
HR Analytics
```

---

# Normalize Crosstab

Percentages instead of counts.

---

```python
pd.crosstab(
 df["Gender"],
 df["Department"],
 normalize=True
)
```

Output:

```text
Percentages
```

---

# Row Percentages

```python
normalize="index"
```

---

# Column Percentages

```python
normalize="columns"
```

---

# Pivot Table vs GroupBy

Huge interview question.

---

## groupby()

Returns:

```text
Analytical Summary
```

---

Example:

```python
df.groupby(
 "Department"
)["Salary"].mean()
```

---

## pivot_table()

Returns:

```text
Report Format
```

---

Example:

```python
df.pivot_table(
 index="Department",
 values="Salary"
)
```

---

# Comparison

| Feature           | groupby  | pivot_table |
| ----------------- | -------- | ----------- |
| Flexible          | ✅        | ❌           |
| Reporting         | ❌        | ✅           |
| Multi-Dimensional | Moderate | Excellent   |
| Excel Style       | ❌        | ✅           |

---

# melt()

Reverse operation of pivot.

Very important interview topic.

---

Wide Data:

| Jan | Feb | Mar |
| --- | --- | --- |
| 100 | 200 | 300 |

---

Convert:

```python
pd.melt(df)
```

Output:

| Variable | Value |
| -------- | ----- |
| Jan      | 100   |
| Feb      | 200   |
| Mar      | 300   |

---

# Why Use melt()?

Machine Learning often prefers:

```text
Long Format
```

instead of:

```text
Wide Format
```

---

# stack()

Converts columns into rows.

---

Example:

```python
df.stack()
```

Useful for reshaping MultiIndex data.

---

# unstack()

Opposite of:

```python
stack()
```

Converts rows into columns.

---

# Real Business Examples

---

## Sales Dashboard

```python
df.pivot_table(
 index="Region",
 columns="Month",
 values="Revenue",
 aggfunc="sum"
)
```

---

## Employee Salary Report

```python
df.pivot_table(
 index="Department",
 values="Salary",
 aggfunc=["mean","max"]
)
```

---

## Survey Results

```python
pd.crosstab(
 df["Gender"],
 df["Response"]
)
```

---

## Product Performance

```python
df.pivot_table(
 index="Product",
 values="Sales",
 aggfunc="sum"
)
```

---

# Hidden Production Problems

---

## Problem 1

Using:

```python
pivot()
```

when duplicates exist.

Causes:

```text
ValueError
```

Use:

```python
pivot_table()
```

instead.

---

## Problem 2

Forgetting:

```python
fill_value
```

Large reports contain many NaNs.

---

## Problem 3

Ignoring MultiIndex columns after multiple aggregations.

Can break exports.

---

## Problem 4

Using crosstab for huge cardinality columns.

Example:

```text
CustomerID
```

with millions of unique values.

Consumes huge memory.

---

# Frequently Asked Interview Questions

### Difference Between pivot() and pivot_table()?

pivot:

```text
No duplicates allowed
```

pivot_table:

```text
Duplicates allowed
Aggregation supported
```

---

### What does margins=True do?

Adds grand totals.

---

### What is crosstab()?

Creates frequency/count tables.

---

### Difference Between pivot_table() and groupby()?

pivot_table:

```text
Report Oriented
```

groupby:

```text
Analysis Oriented
```

---

### What is melt()?

Converts wide data into long format.

---

### What is stack()?

Columns → Rows.

---

### What is unstack()?

Rows → Columns.

---

# Quick Revision

```python
# Pivot
df.pivot(
 index="Date",
 columns="Product",
 values="Sales"
)

# Pivot Table
df.pivot_table(
 index="Department",
 values="Salary",
 aggfunc="sum"
)

# Multiple Aggregations
df.pivot_table(
 index="Department",
 values="Salary",
 aggfunc=["sum","mean"]
)

# Grand Total
df.pivot_table(
 margins=True
)

# Crosstab
pd.crosstab(
 df["Gender"],
 df["Department"]
)

# Melt
pd.melt(df)

# Stack
df.stack()

# Unstack
df.unstack()
```

# DateTime Handling in Pandas (`to_datetime()`, Date Arithmetic, Time Zones, Resampling & Time-Series Analysis)

Date and Time data is one of the most important topics in Pandas.

Almost every enterprise dataset contains:

```text
Transaction Dates
Order Dates
Log Timestamps
Stock Prices
Sensor Data
User Activity
Financial Records
```

If you work as a:

```text
Data Analyst
Data Engineer
ML Engineer
Business Analyst
```

you will use DateTime operations daily.

---

# Why DateTime Is Important?

Example Dataset:

| OrderID | OrderDate  | Sales |
| ------- | ---------- | ----- |
| 1       | 2024-01-01 | 100   |
| 2       | 2024-01-02 | 150   |
| 3       | 2024-01-03 | 200   |

Questions:

```text
Monthly Sales?
Weekly Revenue?
Orders Last 30 Days?
Year-over-Year Growth?
```

All require DateTime processing.

---

# Understanding DateTime Data

Many beginners think:

```python
"2024-01-01"
```

is a date.

Actually:

```python
type("2024-01-01")
```

Output:

```text
str
```

Just a string.

---

# Why Is This A Problem?

Strings cannot perform:

```text
Date Arithmetic
Resampling
Time-Series Analysis
Date Filtering
```

---

# Solution

Convert string to DateTime.

---

# 1. to_datetime()

Most important DateTime function.

---

## Syntax

```python
pd.to_datetime()
```

---

## Example

```python
df["Date"] = pd.to_datetime(
    df["Date"]
)
```

---

Before:

```python
df["Date"].dtype
```

Output:

```text
object
```

---

After:

```python
datetime64[ns]
```

---

# Internal Conversion

```text
"2024-01-01"
        ↓
Timestamp
        ↓
datetime64[ns]
```

---

# Why datetime64?

Optimized NumPy-based datetime storage.

Allows:

```text
Fast Calculations
Filtering
Sorting
Resampling
```

---

# Checking Datatype

```python
df.info()
```

Output:

```text
Date datetime64[ns]
```

Always verify after conversion.

---

# Multiple Date Formats

Pandas automatically detects many formats.

---

## ISO Format

```python
pd.to_datetime(
 "2024-01-01"
)
```

---

## Slash Format

```python
pd.to_datetime(
 "01/01/2024"
)
```

---

## Full Date

```python
pd.to_datetime(
 "January 1 2024"
)
```

---

All convert successfully.

---

# Format Parameter

Interview favorite.

---

Example:

```python
pd.to_datetime(
 df["Date"],
 format="%d-%m-%Y"
)
```

---

Date:

```text
15-06-2024
```

Format:

```text
%d → Day
%m → Month
%Y → Year
```

---

# Why Use format?

Advantages:

```text
Faster Parsing
Less Ambiguity
Better Performance
```

---

# Common Format Codes

| Code | Meaning |
| ---- | ------- |
| %Y   | Year    |
| %m   | Month   |
| %d   | Day     |
| %H   | Hour    |
| %M   | Minute  |
| %S   | Second  |

---

Example

```python
"%Y-%m-%d %H:%M:%S"
```

Represents:

```text
2024-06-15 14:30:45
```

---

# errors Parameter

Very important.

---

Example:

```python
pd.to_datetime(
 df["Date"],
 errors="coerce"
)
```

---

Bad Date:

```text
2024-13-50
```

becomes:

```python
NaT
```

instead of error.

---

# What is NaT?

Equivalent of:

```python
NaN
```

for datetime values.

---

Meaning:

```text
Not a Time
```

---

# Date Components (`dt` Accessor)

Most important interview topic.

---

Once converted:

```python
datetime64
```

you can extract parts.

---

# Year

```python
df["Date"].dt.year
```

Output:

```text
2024
2024
2024
```

---

# Month

```python
df["Date"].dt.month
```

Output:

```text
1
2
3
```

---

# Day

```python
df["Date"].dt.day
```

---

# Hour

```python
df["Date"].dt.hour
```

---

# Minute

```python
df["Date"].dt.minute
```

---

# Second

```python
df["Date"].dt.second
```

---

# Day Name

```python
df["Date"].dt.day_name()
```

Output:

```text
Monday
Tuesday
Wednesday
```

---

# Month Name

```python
df["Date"].dt.month_name()
```

Output:

```text
January
February
March
```

---

# Weekday

```python
df["Date"].dt.weekday
```

Output:

```text
Monday = 0
Tuesday = 1
...
Sunday = 6
```

---

# Quarter

```python
df["Date"].dt.quarter
```

Output:

```text
1
2
3
4
```

---

# Business Use Cases

### Monthly Revenue

```python
df.groupby(
 df["Date"].dt.month
)["Sales"].sum()
```

---

### Quarterly Sales

```python
df.groupby(
 df["Date"].dt.quarter
)["Sales"].sum()
```

---

### Weekday Analysis

```python
df.groupby(
 df["Date"].dt.day_name()
)["Sales"].sum()
```

---

# Date Filtering

Very common.

---

## Exact Date

```python
df[
 df["Date"] ==
 "2024-01-01"
]
```

---

## Greater Than

```python
df[
 df["Date"] >
 "2024-01-01"
]
```

---

## Between Dates

```python
df[
 (df["Date"] >= "2024-01-01")
 &
 (df["Date"] <= "2024-03-31")
]
```

---

# Date Arithmetic

Huge interview topic.

---

Example

```python
df["EndDate"] -
df["StartDate"]
```

Output:

```text
Timedelta
```

---

# Example

```python
2024-01-10
-
2024-01-01
```

Result:

```text
9 days
```

---

# Timedelta

Represents duration.

---

```python
pd.Timedelta(
 days=5
)
```

Output:

```text
5 days
```

---

# Add Days

```python
df["Date"] +
pd.Timedelta(days=7)
```

Adds:

```text
7 days
```

---

# Subtract Days

```python
df["Date"] -
pd.Timedelta(days=30)
```

Returns:

```text
30 days earlier
```

---

# DateOffset

More advanced than Timedelta.

---

Example:

```python
pd.DateOffset(
 months=1
)
```

---

Add One Month:

```python
df["Date"] +
pd.DateOffset(months=1)
```

---

Why not Timedelta?

Because:

```text
Months
Years
```

have variable lengths.

---

# DateOffset Examples

```python
pd.DateOffset(days=10)
```

---

```python
pd.DateOffset(months=2)
```

---

```python
pd.DateOffset(years=1)
```

---

# Current Date

```python
pd.Timestamp.now()
```

Output:

```text
Current Timestamp
```

---

# Today's Date

```python
pd.Timestamp.today()
```

---

# Date Range

Very common.

---

```python
pd.date_range(
 start="2024-01-01",
 end="2024-01-10"
)
```

Output:

```text
10 dates
```

---

# Generate Monthly Dates

```python
pd.date_range(
 start="2024-01-01",
 periods=12,
 freq="M"
)
```

---

# Frequency Codes

| Code | Meaning |
| ---- | ------- |
| D    | Day     |
| W    | Week    |
| M    | Month   |
| Q    | Quarter |
| Y    | Year    |
| H    | Hour    |
| T    | Minute  |

---

Example:

```python
freq="D"
```

Daily.

---

```python
freq="M"
```

Monthly.

---

# Setting Date Index

Very important.

---

```python
df = df.set_index(
 "Date"
)
```

Output:

```text
Date becomes index
```

---

# Why Use Date Index?

Enables:

```text
Time-Series Indexing
Resampling
Rolling Windows
Fast Filtering
```

---

# Partial Date Selection

Amazing Pandas feature.

---

```python
df.loc["2024"]
```

Entire year.

---

```python
df.loc["2024-06"]
```

Entire month.

---

```python
df.loc["2024-06-15"]
```

Specific day.

---

# Time Zones

Advanced interview topic.

---

# Why Time Zones Matter?

Example:

```text
India
USA
Europe
```

same timestamp means different local times.

---

# Localize

```python
df.index.tz_localize(
 "Asia/Kolkata"
)
```

---

# Convert Time Zone

```python
df.index.tz_convert(
 "UTC"
)
```

---

# Common Time Zones

```text
UTC
Asia/Kolkata
US/Eastern
Europe/London
```

---

# Resampling

One of the most important Pandas features.

---

# What is Resampling?

Change frequency of time-series data.

---

Example:

```text
Daily Data
      ↓
Monthly Data
```

---

# Monthly Sum

```python
df.resample(
 "M"
).sum()
```

---

# Monthly Mean

```python
df.resample(
 "M"
).mean()
```

---

# Weekly Sales

```python
df.resample(
 "W"
).sum()
```

---

# Annual Revenue

```python
df.resample(
 "Y"
).sum()
```

---

# Real Example

Daily Sales:

```text
Jan 1 → 100
Jan 2 → 150
Jan 3 → 200
```

---

```python
df.resample(
 "M"
).sum()
```

Output:

```text
January Total = 450
```

---

# Difference Between GroupBy and Resample

---

## GroupBy

```python
groupby(
 df["Date"].dt.month
)
```

Groups by extracted month.

---

## Resample

```python
resample("M")
```

Uses actual DateTime index.

More powerful.

---

# Rolling + DateTime

Very common.

---

7-Day Moving Average:

```python
df["Sales"].rolling(
 7
).mean()
```

---

30-Day Moving Average:

```python
df["Sales"].rolling(
 30
).mean()
```

---

# Hidden Production Problems

---

## Problem 1

Forgetting:

```python
pd.to_datetime()
```

before analysis.

---

## Problem 2

Using strings instead of datetime.

---

## Problem 3

Not setting Date as index before resampling.

---

Bad:

```python
df.resample("M")
```

without DateTime index.

Error.

---

## Problem 4

Time zone mismatches.

Common in global applications.

---

# Frequently Asked Interview Questions

### What does pd.to_datetime() do?

Converts strings into datetime objects.

---

### What is NaT?

Missing DateTime value.

Equivalent of NaN for dates.

---

### Difference Between Timedelta and DateOffset?

Timedelta:

```text
Fixed Duration
```

DateOffset:

```text
Calendar Aware
```

---

### Why Use DateTime Index?

Supports:

```text
Resampling
Time-Series Analysis
Fast Date Filtering
```

---

### What does resample() do?

Changes frequency of time-series data.

---

### Difference Between GroupBy and Resample?

GroupBy:

```text
General Grouping
```

Resample:

```text
Time-Based Grouping
```

---

### What is tz_localize()?

Assigns a timezone.

---

### What is tz_convert()?

Converts between timezones.

---

# Quick Revision

```python
# Convert
df["Date"] = pd.to_datetime(
 df["Date"]
)

# Extract Parts
df["Date"].dt.year
df["Date"].dt.month
df["Date"].dt.day

# Day Name
df["Date"].dt.day_name()

# Timedelta
df["Date"] + pd.Timedelta(days=7)

# DateOffset
df["Date"] + pd.DateOffset(months=1)

# Date Range
pd.date_range(
 start="2024-01-01",
 periods=12,
 freq="M"
)

# Date Index
df.set_index("Date")

# Resample
df.resample("M").sum()

# Timezone
df.index.tz_localize(
 "Asia/Kolkata"
)

# Convert TZ
df.index.tz_convert(
 "UTC"
)
```





















