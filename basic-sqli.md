# Basic SQL Injection (Authentication Bypass & Data Retrieval)

## Overview
Basic SQL injection occurs when user input is directly included in a SQL query without proper validation. This allows an attacker to manipulate the query logic and bypass application controls such as authentication or data filtering.

## Lab Source
Completed via PortSwigger Web Security Academy

## Labs Covered
- SQL injection in WHERE clause (retrieving hidden data)
- SQL injection vulnerability allowing login bypass

---

## Methodology

### 1. Retrieving Hidden Data (WHERE Clause Injection)

I intercepted a request that filtered products by category using Burp Suite.

The request looked like:
GET /filter?category=Gifts
I modified the `category` parameter with the payload:
' OR 1=1--
This caused the query to return all products, including hidden or unreleased items.

---

### 2. Authentication Bypass (Login Injection)

I intercepted the login request using Burp Suite.

The application submitted a username and password to the server.

I modified the username field with the payload:
administrator'--
This payload comments out the password check in the SQL query.

As a result, I was able to log in as the administrator without knowing the password.

---

## Explanation of Payloads

### `' OR 1=1--`
- Always evaluates to TRUE  
- Bypasses filtering conditions  

### `administrator'--`
- Closes the query string  
- Comments out the password check  

---

## Impact
- Unauthorized access to restricted data  
- Authentication bypass  
- Potential full compromise of the application  

---

## Remediation
- Use parameterized queries (prepared statements)  
- Avoid embedding user input directly into SQL queries  
- Implement proper input validation  
- Follow secure coding practices from OWASP  

---

## What I Learned
This lab showed how simple SQL injection payloads can bypass application logic. I learned how attackers can manipulate queries to gain unauthorized access and retrieve hidden data.
