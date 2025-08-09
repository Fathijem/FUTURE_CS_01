# PHP CGI Remote Code Execution (RCE)

---

## 1. Description
PHP CGI Remote Code Execution is a vulnerability that occurs when PHP is run in CGI mode without proper configuration.  
Attackers can pass specially crafted query strings (e.g., `-d` parameters) to execute arbitrary PHP code on the server.  
This often arises from the misuse of PHP-CGI binary or improper `php.ini` configurations, allowing direct code injection and system command execution.

---

## 2. CVSS Score & Severity
**Severity:** Critical  
**CVSS v3.1 Base Score:** **9.8**  

---

## 3. Impact
- **Remote Code Execution** – Full control of the web server.
- **Data Theft** – Access to sensitive files and credentials.
- **Privilege Escalation** – Move from web server access to full system compromise.
- **Persistence** – Install backdoors for continued access.
- **Service Disruption** – Stop or alter website functionality.

---

## 4. Attack Vector
The attack exploits the way PHP-CGI processes command-line arguments embedded in the query string.  
Example malicious request:
```
http://target/php-cgi/php-cgi.exe?-d+allow_url_include%3dOn+-d+auto_prepend_file%3dphp://input
```
The attacker can send PHP code in the POST body, which will be executed by the server.

---

## 5. Vulnerability Scan
The vulnerability was confirmed using:
- **Manual Testing** – Crafted payloads executed successfully.
- **PHP Information Disclosure** – Verified CGI mode and dangerous configurations.
  
**PHP Info Disclosure Screenshot:**  
<img width="1280" height="557" alt="image" src="https://github.com/user-attachments/assets/f493851c-b552-4243-a643-35b11e607eba" />

---

## 6. How the Attack is Performed

### Step 1 – Identify PHP in CGI mode
Discovered through information disclosure that the target runs PHP in CGI mode.  
<img width="1447" height="627" alt="image" src="https://github.com/user-attachments/assets/6cc4abff-f283-49be-8a51-c9a3ba72d541" />

---

### Step 2 – After knowing website IP, find suitable exploit
Once the website's IP was identified, searched for a matching PHP CGI vulnerability exploit.  
<img width="1280" height="494" alt="image" src="https://github.com/user-attachments/assets/65a622b4-e91d-41d1-827b-2dc4170606d6" />

---

### Step 3 – Exploit using Metasploit
Using Metasploit, searched for the relevant exploit module.
<img width="1176" height="174" alt="image" src="https://github.com/user-attachments/assets/6240d592-da54-42a7-aafc-de25b4c1af90" />

---

### Step 4 – Gain shell access via Metasploit
Executed the public exploit using Metasploit, establishing a remote shell.  
<img width="652" height="97" alt="image" src="https://github.com/user-attachments/assets/4fa4a177-a3ec-4a74-8c0c-85665f48f737" />

---

### Step 5 – Maintain and interact with the shell
Interacted with the compromised system and escalated access.  
<img width="988" height="420" alt="image" src="https://github.com/user-attachments/assets/868fdf2a-6b70-4bd7-a62f-b09fea7336d8" />

---

## 7. Prevention & Mitigation

| Method | Description |
|--------|-------------|
| **Disable PHP-CGI** | Avoid running PHP in CGI mode unless absolutely necessary. |
| **Patch PHP** | Apply vendor security updates that address CGI vulnerabilities. |
| **Restrict Access** | Use `.htaccess` or server configs to block direct access to PHP-CGI binary. |
| **Harden PHP Config** | Disable dangerous settings like `allow_url_include` and enforce safe defaults. |
| **Web Application Firewall (WAF)** | Detect and block suspicious query strings containing PHP command-line parameters. |
