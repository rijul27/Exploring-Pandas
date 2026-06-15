Pandas is a fast, powerful, and flexible open-source data analysis and manipulation library.Structurally, it is designed to handle labeled, tabular data—think of it as an in-memory SQL database or an Excel spreadsheet that you control entirely via code.

pandas is one of the most important libraries in Python for:

Data Analysis
Data Cleaning
Data Engineering
Machine Learning preprocessing
Automation
Reporting Systems
ETL Pipelines

Why Pandas Exists ?


Before Pandas:

Developers manually parsed files
Data transformation was slow
Handling missing values was difficult
Data aggregation required large custom code

Pandas provides:

Fast tabular operations
Vectorized computation
SQL-like data handling
Memory-efficient structures
Easy file parsing









































What is a DataFrame ?
A 2-dimensional labeled tabular data structure.A DataFrame is a two-dimensional, size-mutable, and potentially heterogeneous tabular data structure with labeled axes (rows and columns).
 







1. Reading CSV Files  (CSV = Comma Separated Values)

Example:

name,age,city
Raj,22,Delhi
Aman,25,Mumbai

CSV is the most common data format in industry because:

Lightweight
Fast
Universal
Human-readable
Easy for systems to exchange

Basic CSV Reading

	Output  Example
    name     age    city
0    Raj       22    Delhi
1   Aman   25   Mumbai

Notice:

Left side numbers are index
Columns automatically inferred



Important Parameters of read_csv()

1. sep (Separator)

Default separator:  ,

But sometimes files use:
|
;
\t

Example:
	
Means: Read the file named "data.txt" using Pandas, where columns are separated using the pipe symbol |.


Normally, CSV files use commas:

name,age,city
Raj,22,Delhi

Default behavior of:  pd.read_csv() assumes separator is: , (comma)

But Some Files Use Different Separators

Example:

Raj|22|Delhi
Aman|25|Mumbai

Here separator is: |

called:

Pipe delimiter
Pipe separator
	
So we tell Pandas:  sep="|"

meaning: "Split each row wherever you find |"


Example

File: data.txt

Name|Age|City
Raj|22|Delhi
Aman|25|Mumbai


	Output




What Happens if sep="|" is NOT Given?

Pandas assumes comma ,

So entire row becomes one column:

	
Sometimes files contain mixed separators:

Raj|22|Delhi
Aman,25,Mumbai

This causes: ParserError

Senior engineers validate raw files before processing.





2.header Parameter

In pandas, the header parameter is used while reading files like:

CSV
Excel
HTML tables

Its job is: To tell Pandas which row should be treated as the column names.

Basic Understanding of header

Suppose we have a CSV file:


	Code :


Pandas automatically assumes: 
First row = column names
	
Output : 

name	age 	city
Raj	22	Delhi
Aman	25	Mumbai









3. names

Assign custom column names while reading data files.

It is commonly used with:

read_csv()
read_table()
read_fwf()

Why names Exists ?

In real-world datasets:

Headers may be missing
Headers may be incorrect
Duplicate headers may exist
Random column names may appear
Export systems may generate broken headers

Instead of fixing files manually, Pandas allows developers to define columns directly.

Suppose data.csv contains:

Raj,22,Delhi
Aman,25,Mumbai

Notice:

No headers exist
	Output

    Name    Age    City
0    Raj       22     Delhi
1   Aman   25    Mumbai

Now Pandas creates custom headers.




5. usecols

Read only selected columns.

	Why Senior Engineers Use usecols() ?

Huge datasets may contain:

500+ columns
GBs of memory usage


Reading unnecessary columns wastes:

RAM
CPU
Disk I/O

6. nrows

Read only a limited number of rows from a file.

Basic Syntax

CSV


	Excel


	
What nrows Actually Does ?


Suppose file contains:

100000 rows

If you use: nrows=100
	Pandas will:

Open file
    ↓
Read first 100 rows
    ↓
Stop reading remaining rows
    ↓
Create DataFrame
	This improves:

Speed
Memory usage
Debugging efficiency









7. skiprows


Skip unwanted rows while reading a file.

It is commonly used in:


CSV files
Excel files	Text files
Real enterprise reports	Government datasets
ERP exports	
Machine-generated logs


Example:

Company Report 2026
Generated on 24-May-2026

Name,  Age,   City
Raj,       22,     Delhi
Aman,  25,     Mumbai

Without skiprows: Pandas may incorrectly interpret Company Report 2026 as a header row.

Basic Syntax

	
Meaning:

Skip first 2 rows
Row 3 becomes the new header row automatically.






8. dtype

Specify datatypes manually.


Datatype determines:

Memory usage
Speed
Allowed operations
Internal storage format
Performance efficiency
	Manually Setting dtype During read_csv()



  


     

  
  
9. parse_dates

parse_dates is used to automatically convert date/time columns into proper datetime objects while reading data.

Without parse_dates, dates are usually loaded as: object
which means: Plain string values
With parse_dates, Pandas converts them into: datetime64[ns]
which is a highly optimized datetime datatype.


Why parse_dates is Important?

Almost every enterprise application contains:

timestamps
logs
	
transaction dates
user activity dates
	audit times
financial records
	analytics windows


Without proper datetime conversion:

You cannot efficiently perform:

filtering
sorting
	grouping
time analysis
	rolling calculations
resampling
	trend analysis



Basic Syntax
	Example CSV














10. encoding in Pandas (read_csv)

What is Encoding?

Encoding is the process of converting human-readable characters into bytes so that computers can store and read text files.

When you save a CSV file, the text is not stored as actual characters. It is stored as binary data (bytes).

Example:

Hello

Internally becomes:

01001000
01100101
01101100
01101100
01101111

The mapping between characters and bytes is called character encoding.



   
Real Production Issue



Handling Bad Rows in Pandas

One of the most common problems when working with real-world datasets is bad rows (also called malformed rows, corrupted records, or invalid records).

A bad row is a row that does not follow the expected structure of the file.

For example, if a CSV file should have 3 columns :



but one row contains extra values:



The third row has 4 columns instead of 3, making it a bad row.
Why Bad Rows Occur in Real Projects ?

In production systems, data comes from:


Users
Excel exports
Third-party vendors
Legacy applications
APIs
Automated scripts

Any of these sources can generate invalid records.













Logging Bad Rows

Example:



When a bad row occurs:





Reading Huge CSV Efficiently — Chunk Processing in Pandas

In real-world projects, CSV files are often hundreds of MBs or even several GBs in size. If you try to load such files using a normal read_csv() call, Pandas attempts to load the entire file into memory (RAM) at once.



For small files, this works perfectly.

However, for huge files:

5 GB CSV File
      ↓
Pandas tries to load everything
      ↓
RAM becomes full
      ↓
System slows down
      ↓
MemoryError / Crash

This is where Chunk Processing becomes extremely important.

Chunk Processing means:Reading a large file in smaller pieces instead of loading the entire file into memory.

Basic Syntax



	What Happens Internally?

    CSV File
        ↓
    Read first 10,000 rows
        ↓
    Create DataFrame
        ↓
    Process rows
        ↓
    Discard from memory
        ↓
    Read next 10,000 rows
        ↓
Repeat

Memory remains stable because only one chunk is loaded at a time.


   











Reading Excel Files

Installing Dependency : pip install openpyxl

Why needed? Pandas requires an engine to interpret Excel structure.

 






















  









Reading HTML Tables in Pandas (read_html())

In the real world, a large amount of data is published on websites in the form of HTML tables.

Examples:
Stock Market Data	Sports Statistics	Government Reports

Instead of manually copying data from websites into Excel, Pandas provides a powerful function: pd.read_html()

which automatically extracts HTML tables and converts them into DataFrames.









 

























Hands-on







Pandas Advance Techniques





































Series in Pandas

A Series is the most fundamental data structure in pandas.


Think of a Series as:

A one-dimensional labeled array capable of holding any datatype.

It is similar to:

A single column in Excel
A single column from a SQL table
A Python list with labels (indexes)
A NumPy array with indexing support








  





































































 
  










































 







Important Update (Pandas 2.1+)

In newer versions of Pandas, applymap() has been deprecated in favor of DataFrame.map() for element-wise DataFrame operations.

Modern equivalent:


In interviews and existing codebases, you will still see applymap(), so it's important to understand it thoroughly.


















   
  








































