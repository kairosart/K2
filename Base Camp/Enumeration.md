## Nmap

Scan all ports on the target using Nmap.
```
nmap -p- -T4 <MACHINE IP> -Pn
```

![[nmap.png]]

Here's a breakdown of the Nmap scan results you provided, interpreting each open port and its likely role in the system:

### 🖥️ Host Summary

- **Hostname:** `K2ROOTDC`
    
- **Operating System:** Microsoft Windows
    
- **CPE Identifier:** `cpe:/o:microsoft:windows` (used for standardized OS identification)
    
- **Role:** Likely a **Domain Controller** in a Windows Active Directory environment
    

### 🔍 Port Analysis

|Port|Service|Description|
|---|---|---|
|**88/tcp**|`kerberos-sec`|Kerberos authentication service used by Active Directory. Confirms this host is part of a domain controller setup. The server time is also revealed, which can be useful for Kerberos ticket validation.|
|**389/tcp**|`ldap`|Lightweight Directory Access Protocol. Used for querying and modifying items in Active Directory. The domain (`k2.thm0.`) and site (`Default-First-Site-Name`) indicate this is part of a structured AD forest.|
|**464/tcp**|`kpasswd5?`|Typically used for Kerberos password changes. The question mark suggests Nmap wasn't able to fully confirm the service, but it's likely related to Kerberos.|
|**593/tcp**|`ncacn_http`|RPC over HTTP. Often used by services like Outlook Anywhere or remote management tools. Indicates remote procedure calls are exposed via HTTP.|
|**636/tcp**|`tcpwrapped`|This is the secure version of LDAP (LDAPS). "tcpwrapped" means the service is protected by a wrapper (like SSL/TLS), and Nmap couldn’t probe it further without SSL-specific options.|

### 🧠 Interpretation

This host is almost certainly a **Windows Domain Controller**:

- It runs **Kerberos** and **LDAP**, which are core to Active Directory.
    
- It supports **secure LDAP (LDAPS)** and **Kerberos password changes**.
    
- The presence of **RPC over HTTP** suggests remote management capabilities.
    

If you're assessing this for security or enumeration:

- These ports are critical for domain authentication and directory services.
    
- You may want to follow up with tools like `ldapsearch`, `rpcclient`, or `kerbrute` depending on your goals.

