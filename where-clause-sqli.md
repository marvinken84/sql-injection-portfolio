# SQL Injection in WHERE Clause (Retrieving Hidden Data)

## Overview
This lab demonstrated a SQL injection vulnerability in a WHERE clause that allowed me to bypass filtering and retrieve hidden (unreleased) products.

## Lab Source
Completed via PortSwigger Web Security Academy

## Methodology

### Step 1: Intercepting the Request
I used Burp Suite to intercept a request that filters products by category.

The request included a parameter like:
' OR 1=1--
This payload alters the query logic so that the condition always evaluates to TRUE.

---

### Step 3: Submitting the Modified Request
I forwarded the modified request from Burp Suite to the server.

---

### Step 4: Observing the Response
The response now included additional products that were not normally visible, including unreleased items.

This confirmed that the SQL query had been manipulated successfully.


---

## Explanation of the Payload
' OR 1=1--
- `'` → closes the original query string  
- `OR 1=1` → always TRUE condition  
- `--` → comments out the rest of the query  

This causes the database to return all records instead of filtered results.

---

## Impact
- Exposure of hidden or restricted data  
- Bypass of application filtering logic  
- Potential access to sensitive database content  

---

## Remediation
- Use parameterized queries (prepared statements)  
- Avoid directly embedding user input in SQL queries  
- Implement proper input validation  
- Follow best practices from OWASP  

---

## What I Learned
This lab helped me understand how SQL injection can bypass application logic and expose hidden data. I learned how simple payloads can manipulate backend queries and return unintended results.
