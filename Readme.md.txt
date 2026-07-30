Active Directory Security Audit

> A PowerShell-based security auditing tool for Windows systems — identifies vulnerabilities in user accounts, password policies, firewall configurations, open ports, and login activity.

---

## ?? About the Project

This project was developed as a **Minor Degree Course (MDC) project** for the **B.Tech CSE program** at **Model Institute of Engineering and Technology (MIET), Jammu** under the course **MDC-CS-401 — Introduction to Cyber Security**.

The tool automates common Windows security checks that administrators typically perform manually — making the process faster, repeatable, and less error-prone.

---

## ?? Features

| Module | What it checks |
|---|---|
| ?? User Account Audit | Lists all local accounts; flags active/default accounts |
| ??? Admin Privilege Check | Lists members of the Administrators group |
| ?? Password Policy Analysis | Checks min length, lockout threshold, expiry |
| ?? Active Session Monitoring | Shows currently logged-in users |
| ?? Firewall Status | Checks Domain, Private, Public profile states |
| ?? Shared Folder Detection | Lists all network shares on the system |
| ?? Open Port Scanning | Shows all LISTENING ports (first 15) |
| ? Failed Login Detection | Reads Security Event Log for Event ID 4625 |
| ?? System Info | Hostname and current user context |

---

## ??? Technologies Used

- **Language:** PowerShell 5.1+
- **Platform:** Windows 10 / Windows Server 2019
- **APIs:** Windows Event Log, Windows Registry, NetSecurity
- **Run As:** Administrator (elevated privileges required)

---

## ?? Files

```
Active_Directory_SecurityAudit/
¦
+-- AD_Audit.ps1       # Main audit script — read-only, safe to run
+-- AD_Fix.ps1         # Remediation script — applies security fixes
```

---

## ?? How to Run

### Step 1 — Open PowerShell as Administrator
Right-click the Start menu ? **Windows PowerShell (Admin)**

### Step 2 — Navigate to the project folder
```powershell
cd Desktop\Active_Directory_SecurityAudit
```

### Step 3 — Run the audit
```powershell
.\AD_Audit.ps1
```

### Step 4 — (Optional) Apply security fixes
```powershell
.\AD_Fix.ps1
```

> ?? **Run only on systems you own or have explicit authorization to audit. Unauthorized use is illegal.**

---

## ?? Sample Audit Checks

```powershell
# List all local users
net user

# List administrator group members
net localgroup administrators

# Check password and lockout policy
net accounts

# Show all listening ports
netstat -an | findstr LISTENING

# Check firewall status
netsh advfirewall show allprofiles state

# Detect failed login attempts (Event ID 4625)
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 5
```

---

## ?? Findings from Live Audit (DESKTOP-6R5JT6I)

| Finding | Severity | Status |
|---|---|---|
| Minimum password length = 0 | ?? Critical | Fixed by AD_Fix.ps1 |
| No account lockout threshold | ?? Critical | Fixed by AD_Fix.ps1 |
| Regular user 'Hp' in Administrators group | ?? High | Fixed by AD_Fix.ps1 |
| Port 3389 (RDP) exposed | ?? High | Fixed by AD_Fix.ps1 |
| 2 failed login events detected | ?? Medium | Monitored |

---

## ?? What AD_Fix.ps1 Does

```powershell
net accounts /minpwlen:8           # Set minimum password length to 8
net accounts /lockoutthreshold:5   # Lock account after 5 failed attempts
net localgroup administrators Hp /delete   # Remove unnecessary admin
netsh advfirewall firewall set rule group="remote desktop" new enable=No  # Disable RDP
```

---

## ?? References

1. Microsoft Corporation — [Windows PowerShell Documentation](https://learn.microsoft.com/powershell/)
2. Microsoft Corporation — [Active Directory Domain Services Overview](https://learn.microsoft.com/windows-server/identity/ad-ds/)
3. W. Stallings — *Cryptography and Network Security*, 8th ed., Pearson, 2023
4. Microsoft Corporation — [Windows Security Auditing](https://learn.microsoft.com/windows/security/)

---

## ?? Disclaimer

This tool is intended **only for authorized security auditing** on systems you own or have written permission to test. The authors are not responsible for any misuse of this tool.
