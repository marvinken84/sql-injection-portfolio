# UNION-Based SQL Injection (Column Discovery & Data Extraction)

## Overview
UNION-based SQL injection allows an attacker to combine the results of multiple SQL queries and retrieve data from the database. This technique is used to extract information when the application displays query results.

## Lab Source
Completed via PortSwigger Web Security Academy

## Labs Covered
- Determining the number of columns
- Finding a column that accepts text input

---

## Methodology

### Step 1: Intercepting the Request
I used Burp Suite to intercept a request that filters products by category.

Example request:
GET /filter?category=Gifts
The `category` parameter was used in a backend SQL query, making it injectable.

---

### Step 2: Determining the Number of Columns

I tested the number of columns using the UNION SELECT statement.

Initial payload:' UNION SELECT NULL--
This caused an error, indicating an incorrect number of columns.

I then increased the number of NULL values:
' UNION SELECT NULL, NULL--
I continued adding NULL values until the error disappeared.

Final working payload:
' UNION SELECT NULL, NULL, NULL--
This confirmed that the query returns **3 columns**.

---

### Step 3: Finding a Column that Accepts Text

Next, I needed to identify which column could display text data.

I replaced each NULL value with a test string one at a time:
' UNION SELECT 'test', NULL, NULL--
If an error occurred, I moved to the next column:
' UNION SELECT NULL, 'test', NULL--
Eventually, one of the payloads worked and the string appeared in the response.

This confirmed which column could display text data.

---

### Step 4: Using the Working Column
Once the correct column was identified, it could be used to extract data from the database using UNION queries.

---

## Explanation of Key Concepts

### UNION SELECT
- Combines results from multiple queries  
- Requires same number of columns  

### NULL Values
- Used as placeholders when column types are unknown  

---

## Impact
- Ability to extract sensitive data from the database  
- Exposure of internal database structure  
- Potential full data compromise  

---

## Remediation
- Use parameterized queries (prepared statements)  
- Avoid dynamic SQL queries  
- Validate and sanitize all user inputs  
- Implement least-privilege database access  

---

## What I Learned
This lab taught me how attackers determine database structure before extracting data. I learned how to identify the number of columns and locate the correct column for displaying injected data.
