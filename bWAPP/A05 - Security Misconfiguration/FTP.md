# Insecure FTP Configuration - Anonymous Login

---

## 1. Description
An insecure FTP configuration was identified where the server allows **anonymous login** without authentication.  
This configuration grants any remote user access to files hosted on the server, including potentially sensitive data, without requiring valid credentials.

This vulnerability exists on the target FTP service running **ProFTPD 1.3.1** (Bee-box 192.168.1.11).  
Once connected, an attacker can list, download, and potentially upload files, leading to unauthorized information disclosure.

---

## 2. CVSS Score & Severity
**Severity:** High  
**CVSS v3.1 Base Score:** **7.5**  

---

## 3. Impact
- **Information Disclosure** – Sensitive files accessible without authentication.
- **Intellectual Property Theft** – Download of proprietary or confidential documents.
- **Data Manipulation** – Possible upload/overwrite of existing files.
- **Further Exploitation** – Obtained files could aid in further attacks.

---

## 4. Attack Vector
The vulnerability is exposed via **Port 21 (FTP)**, accepting username `anonymous` with an empty password or generic email.  
The server provides access to the root FTP directory and its contents without any form of authorization.

---

## 5. Vulnerability Scan

### **Apache IP Disclosure**
The Apache server response header discloses the internal IP address in error messages, revealing internal network structure to external parties. 
By viewing this, we can find the Apache Server version, the ip of the vulnerable website. In this case it is localhost, in normal pentesting it will display the ip of the vulnerable website or machine.
<img width="1280" height="578" alt="image" src="https://github.com/user-attachments/assets/bc1cf33c-0a61-4c6a-a2ac-a57858d1a9ad" />


### **Nmap Scan Output**
<img width="1280" height="378" alt="image" src="https://github.com/user-attachments/assets/4544424a-d47f-4016-94e4-b7ad97bd85aa" />

---

## 6. How the Attack is Performed
1. Run `nmap` to identify open ports and services.
2. Connect to the FTP service:
   ```bash
   ftp 192.168.1.11
   Name: anonymous
   Password: <blank>
   ```
   <img width="970" height="166" alt="image" src="https://github.com/user-attachments/assets/55308a28-5868-4f8f-b450-0c9862779ece" />

3. Upon successful login, we can try to find current directory:
<img width="297" height="36" alt="image" src="https://github.com/user-attachments/assets/b8fcf5fd-db41-48fc-b128-ccea7722b308" />

4. Then we can list all the files in the directory using "ls"
<img width="1161" height="177" alt="image" src="https://github.com/user-attachments/assets/6d4abb22-5ce2-43e6-ae81-ad238066fa98" />

5. Using get command we can perform data exflitration and steal sensitive data to our machine.
<img width="1706" height="137" alt="image" src="https://github.com/user-attachments/assets/fb42a390-c0fb-4351-928e-e95d0de855f6" />

## 7. Prevention & Mitigation

| Method | Description |
|--------|-------------|
| **Disable Anonymous FTP** | Restrict FTP access to authenticated users only. |
| **Use SFTP/FTPS** | Implement secure file transfer protocols with encryption. |
| **Restrict Directory Access** | Apply chroot jail or allowlist to limit accessible directories. |
| **Firewall Rules** | Restrict FTP port access to trusted IP addresses. |
| **Monitor & Log** | Enable logging of FTP sessions and monitor for suspicious activity. |
