# Broken Authentication – Password Attacks

---

## 1. Description
Password attacks involve malicious attempts to gain unauthorized access to user accounts by exploiting weak, reused, or predictable credentials.  
Attackers often use brute-force, dictionary attacks, or credential stuffing techniques to guess or reuse known passwords, bypassing authentication controls.  
These attacks can result in account compromise, data theft, and service abuse.

---

## 2. CVSS Score & Severity
**Severity:** High  
**CVSS v3.1 Base Score:** **8.0**

---

## 3. Impact
- **Unauthorized Access** – Full compromise of user or administrator accounts.
- **Data Breach** – Exposure of sensitive personal, financial, or corporate data.
- **Privilege Escalation** – Attackers gaining higher system privileges.
- **Operational Disruption** – Systems being misused or taken offline.
- **Reputation Damage** – Loss of customer trust and potential legal issues.

---

## 4. Attack Vector
Password attacks exploit insecure authentication mechanisms by repeatedly guessing credentials until access is granted.  
Example methods include:
- **Brute Force** – Trying all possible password combinations.
- **Dictionary Attack** – Using precompiled lists of common passwords.
- **Credential Stuffing** – Using leaked username-password pairs from other breaches.

---

## 5. Vulnerability Scan
The vulnerability was identified using:
- **Manual Testing** – Demonstrated successful login via repeated automated attempts.
- **Hydra/OWASP ZAP** – Simulated brute-force login requests without triggering account lockout.
<img width="1027" height="470" alt="image" src="https://github.com/user-attachments/assets/fbc2dedf-4954-4d86-9e78-1be1c07f6edd" />

---

## 6. How the Attack is Performed

### Step 1 – Configure proxy for interception  
The attacker enables **FoxyProxy** in the browser to route traffic through **Burp Suite** for request interception.  
<img width="752" height="331" alt="image" src="https://github.com/user-attachments/assets/e8dbc963-f0a7-4f47-ac91-b0d43a283104" />

---

### Step 2 – Submit login attempt  
A random **username** and **password** are entered on the login page to generate a login request.  
<img width="752" height="331" alt="image" src="https://github.com/user-attachments/assets/a04607ae-8d3e-4251-90c8-82af41da1064" />


---

### Step 3 – Capture the request in Burp Suite  
The HTTP POST request containing the login credentials is intercepted in **Burp Proxy** and then sent to the **Intruder** tab for further testing.  
<img width="752" height="355" alt="image" src="https://github.com/user-attachments/assets/dc9bcb46-fc18-4fc8-839b-b58eb81c36d6" />

---

### Step 4 – Configure Intruder attack  
- **Attack Type:** `Cluster Bomb` is selected to try all combinations of usernames and passwords.  
- **Positions:** The username and password parameters in the request are marked as payload positions.  
<img width="752" height="355" alt="image" src="https://github.com/user-attachments/assets/5b680e3f-4ad4-45f9-8d82-7a4c59bae12a" />


---

### Step 5 – Prepare payload files  
Two separate text files are created:
- `username.txt` – contains a list of possible usernames  
- `password.txt` – contains a list of possible passwords  
<img width="752" height="385" alt="image" src="https://github.com/user-attachments/assets/25e8b54b-abf4-45eb-886f-d1ba0c01a836" />


---

### Step 6 – Run the attack  and Analyze results
The Intruder attack is started, sending multiple requests with all possible username-password combinations from the lists.  
The response lengths are analyzed to detect successful logins.  
<img width="752" height="405" alt="image" src="https://github.com/user-attachments/assets/46c0bc37-3aaf-47af-94f7-0394cd4ee07e" />

### Step 7 – Log in with discovered credentials  
The valid credentials are used to log in to the website, confirming successful exploitation.  
<img width="1902" height="835" alt="image" src="https://github.com/user-attachments/assets/c8cc265f-9b3a-4893-9900-88c7296db774" />

---

## 7. Prevention & Mitigation

| Method                        | Description                                                                 |
|--------------------------------|-----------------------------------------------------------------------------|
| **Enforce Strong Passwords**   | Require a mix of uppercase, lowercase, numbers, and symbols, minimum 12 characters. |
| **Account Lockout Mechanisms** | Temporarily block login after a set number of failed attempts.              |
| **Rate Limiting & CAPTCHA**    | Slow down automated attacks by requiring human interaction.                 |
| **Multi-Factor Authentication (MFA)** | Add another layer of security beyond passwords.                  |
| **Password Hashing**           | Store passwords using secure hashing algorithms like bcrypt or Argon2.      |
| **Monitor & Alert**            | Detect and respond to unusual login patterns.                               |

---


