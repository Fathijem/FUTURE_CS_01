# Cross-Site Scripting (XSS) - Reflected (GET & POST)

---

## 1. Description
Cross-Site Scripting (XSS) is a web security vulnerability that allows attackers to inject malicious JavaScript into a website.  
In **Reflected XSS**, the malicious payload is embedded into a URL or request parameter and is immediately reflected back in the server’s response without being stored.  
When a victim visits the crafted URL, their browser executes the injected script in the context of the vulnerable site.

This vulnerability exists in **bWAPP** in both **GET** and **POST** forms, where user-supplied input is directly embedded into the HTML response without proper sanitization or output encoding.

---

## 2. CVSS Score & Severity
**Severity:** High  
**CVSS v3.1 Base Score:** **7.4**  
**Vector:** AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N

---

## 3. Impact
- **Session Hijacking** – Steal cookies or session tokens.
- **Phishing Attacks** – Redirect users to malicious sites.
- **Keylogging** – Capture user keystrokes.
- **Browser Exploitation** – Execute arbitrary code in the victim’s browser.
- **Reputation Damage** – Use of the site to spread malicious payloads.

---

## 4. Attack Vector
The vulnerable parameters are:
- **GET method:** `firstname`, `lastname`
- **POST method:** `firstname`, `lastname`

Example vulnerable GET request:
```
http://localhost/bWAPP/xss_get.php?firstname=<script>alert(document.cookie)</script>&lastname=Test&form=submit
```

Example vulnerable POST body:
```
firstname=<script>alert(document.cookie)</script>&lastname=Test&form=submit
```
Both vectors allow the injection of arbitrary JavaScript, which executes in the victim’s browser.

---

## 5. Vulnerability Scan
The vulnerability was confirmed using:
- **OWASP ZAP** – Detected reflected script injection in GET and POST parameters.
- **Manual Testing** – Injected payloads in the application forms and observed JavaScript execution.

**ZAP Scan Screenshot:**
<img width="842" height="398" alt="image" src="https://github.com/user-attachments/assets/36752652-866d-4fca-8999-1efaff5ee407" />

---

## 6. How the Attack is Performed

### Case 1 – Reflected XSS via GET
1. Navigate to **XSS - Reflected (GET)** in bWAPP.
2. Set security level to **low**.
3. Inject payload into `firstname` field:
   ```html
   <script>alert(document.cookie)</script>
   ```
<img width="752" height="333" alt="image" src="https://github.com/user-attachments/assets/dae6d5a9-62ca-471a-8a06-f603141fcf85" />

4. Submit form → The payload executes in the browser, showing the PHPSESSID.
<img width="752" height="396" alt="image" src="https://github.com/user-attachments/assets/de819e75-e1e7-49b4-bba0-729e29e63c10" />

5. The original input also gets displayed after the script execution:
<img width="752" height="344" alt="image" src="https://github.com/user-attachments/assets/425488fc-3e3e-4cd0-9901-f8186a34fb29" />

### Case 2 – Reflected XSS via POST
1. Navigate to XSS - Reflected (POST) in bWAPP.
2. Set security level to low.
3. Inject payload into firstname field:
<script>alert(document.cookie)</script>
<img width="1280" height="473" alt="image" src="https://github.com/user-attachments/assets/087e7a91-0f21-4a44-b4a5-0da6544c91d6" />

4. Submit form → The payload executes, showing the PHPSESSID.
<img width="752" height="351" alt="image" src="https://github.com/user-attachments/assets/bd03bf81-908c-4a89-97fd-13a8c93bfda2" />

## 7. Prevention & Mitigation

| Method | Description |
|--------|-------------|
| **Input Validation** | Reject special HTML/JavaScript characters (`<`, `>`, `"`, `'`). |
| **Output Encoding** | Encode user input before inserting into HTML, JS, or attributes. |
| **HTTPOnly Cookies** | Mark session cookies as `HttpOnly` to block JavaScript access. |
| **Content Security Policy (CSP)** | Limit allowed script sources to trusted domains. |
| **Security Testing** | Use automated tools (ZAP, Burp) and manual pentests regularly. |
   
