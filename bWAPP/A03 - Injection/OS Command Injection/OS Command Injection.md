1. OS Command Injection
OS Command Injection is a vulnerability that allows an attacker to execute arbitrary operating system commands on the server hosting the application. It occurs when user input is concatenated directly into a system command without proper validation or sanitization. This gives the attacker the ability to run additional commands with the same privileges as the running application, potentially leading to full system compromise.

2. Impact
Remote Code Execution (RCE) – Full control over the target server.

Data Theft – Reading sensitive files (e.g., /etc/passwd, config files).

System Compromise – Creating or modifying system files, adding backdoors.

Pivoting – Using the compromised server to attack internal network systems.

Denial of Service – Executing commands to disrupt services.

Severity: Critical
CVSS v3.1 Base Score: 9.8 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)

3. Attack Vector
The vulnerability lies in the application’s DNS lookup functionality, where the user input is directly passed to a system command without sanitization. Example vulnerable parameter:
