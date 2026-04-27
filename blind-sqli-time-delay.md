# Blind SQL Injection with Time Delays and Information Retrieval

## Overview
This lab demonstrated a blind SQL injection vulnerability where no data was directly returned by the application. Instead, I used time delays to infer information from the database and extract sensitive data.

## Lab Source
Completed via PortSwigger Web Security Academy

## Methodology

### Step 1: Intercepting the Request
I used Burp Suite to intercept a request containing a tracking cookie.

The application used the cookie value in a backend SQL query, making it a potential injection point.

---

### Step 2: Testing for Injection
I modified the TrackingId cookie value to test for SQL injection.

Initial tests produced normal responses with no delay, confirming that the application did not directly return errors or data.

---

### Step 3: Confirming Time-Based Injection
I injected a time delay payload into the cookie:' OR SLEEP(5)--
The server response was delayed, confirming that my input was executed by the database.

---

### Step 4: Verifying Conditions
I used conditional statements to test true/false responses:
' OR IF(1=1, SLEEP(5), 0)-- 
' OR IF(1=2, SLEEP(5), 0)--
By comparing response times, I confirmed that I could control the query logic.

---

### Step 5: Determining Password Length
I used conditional payloads to determine the length of the administrator password:
' OR IF(LENGTH(password)>5, SLEEP(5), 0)--
By adjusting the number, I identified the exact length of the password.

---

### Step 6: Extracting the Password
I extracted the password one character at a time:
' OR IF(SUBSTRING(password,1,1)='a', SLEEP(5), 0)--
I repeated this process for each character position until I retrieved the full password.

---

### Step 7: Logging in as Administrator
Using the extracted credentials, I successfully logged into the application as the administrator user.

---

## Impact
- Unauthorized access to administrator account  
- Exposure of sensitive user credentials  
- Full compromise of application data  

---

## Remediation
- Use parameterized queries (prepared statements)  
- Avoid dynamic SQL queries  
- Validate and sanitize all user inputs  
- Follow security best practices from OWASP  

---

## What I Learned
This lab helped me understand how attackers can extract sensitive data even when applications do not return visible output. I learned how to use time delays and conditional logic to retrieve hidden information from a database.



