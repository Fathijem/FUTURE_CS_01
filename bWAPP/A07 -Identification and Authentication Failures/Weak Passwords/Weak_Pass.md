# Broken Authentication – Weak Passwords

---

## 1. Description
Weak password vulnerabilities occur when a web application allows users (or administrators) to set simple, easily guessable passwords without enforcing strong password policies.  
Attackers can exploit this by guessing common passwords or using automated brute-force tools to gain unauthorized access to accounts.  
This significantly increases the risk of account takeover, data theft, and privilege escalation.

---

## 2. CVSS Score & Severity
**Severity:** High  
**CVSS v3.1 Base Score:** **7.5**  

---

## 3. Impact
- **Account Takeover** – Unauthorized access to user or admin accounts.
- **Sensitive Data Exposure** – Personal information, credentials, and confidential files at risk.
- **Privilege Escalation** – Gaining administrative access to manage or alter the system.
- **Service Abuse** – Use of compromised accounts for malicious activities.
- **Platform Reputation Damage** – Loss of trust and potential legal consequences.

---

## 4. Attack Vector
The attack leverages the lack of password complexity and enforcement in the authentication process.  
Example attack methods:
- Guessing common credentials like `admin/admin` or `test/test123`.
- Using a wordlist with a brute-force tool to automate login attempts.

---

## 5. Vulnerability Scan
The vulnerability was confirmed using:
- **OWASP ZAP** – Detected absence of password complexity checks.
- **Manual Testing** – Successfully authenticated with trivial passwords.

**ZAP Scan Screenshot:**  
<img width="867" height="270" alt="image" src="https://github.com/user-attachments/assets/eca02085-e575-44b0-bdac-08644932922b" />


---

## 6. How the Attack is Performed

### Step 1 – Access the login page
Navigate to the web application's login form.  
<img width="752" height="329" alt="image" src="https://github.com/user-attachments/assets/7fd73d51-d11b-4ab2-ab27-fc519b227ced" />


---

### Step 2 – Attempt weak credentials
Test simple usernames and passwords such as:
Username: test
Password: test
<img width="752" height="295" alt="image" src="https://github.com/user-attachments/assets/500d5fcb-5ed5-480b-bd55-751a498ba147" />

---

### Step 4 – Medium Level Difficulty
<img width="752" height="297" alt="image" src="https://github.com/user-attachments/assets/3fd600b6-75b4-4339-b638-9829bb378b9f" />

---

### Step 4 –High Level Difficulty
<img width="752" height="297" alt="image" src="https://github.com/user-attachments/assets/2ffbac69-f857-4d44-a7b4-90a513fde66e" />


---

### Step 5 – Source code verification
Review application code to confirm lack of complexity checks.  On reviewing the code, we can find that passwords are stored in plain text for all the levels. 0 indicates low and 2 indicated high. using this we can exploit and gain access.
<img width="752" height="348" alt="image" src="https://github.com/user-attachments/assets/f939471f-229e-4247-b45c-2ca743ffdc5a" />


---

## 7. Prevention & Mitigation

| Method | Description |
|--------|-------------|
| **Strong Password Policy** | Require a mix of uppercase, lowercase, numbers, and special characters, with at least 12 characters in length. |
| **Password Hashing** | Store passwords using secure hashing algorithms such as `bcrypt` or `Argon2`. |
| **Account Lockout** | Lock accounts after a number of failed login attempts to deter brute force attacks. |
| **Multi-Factor Authentication (MFA)** | Require additional verification beyond just a password. |
| **Regular Password Audits** | Enforce periodic password updates and monitor password strength. |
| **User Education** | Inform users about the risks of weak passwords and best practices for creating strong ones. |
