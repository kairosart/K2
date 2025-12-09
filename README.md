# ✅ **K2 — High-Level Summary of the Room & Solution**

**K2** is a beginner–intermediate TryHackMe room themed around a mountain-climbing challenge. The goal is to escalate through several levels of access on a Linux machine, ultimately achieving full system compromise. The room focuses on:

### **1. Enumeration**

You begin with a target host that you must enumerate using standard techniques:

- Port scanning
    
- Service fingerprinting
    
- Basic website or service exploration
    

The enumeration phase reveals an entry point—usually a simple web service or an exposed application.

---

### **2. Initial Foothold**

Once you identify the vulnerable service, you gain limited access to the system.  
Common concepts involved:

- Identifying version-specific vulnerabilities
    
- Exploiting basic misconfigurations
    
- Using publicly available exploits or manual tricks
    

The foothold typically lands you as a **low-privileged user** on the box.

---

### **3. Privilege Escalation — User**

After gaining initial access, you enumerate the system again from inside.  
You usually discover:

- An interesting file
    
- Weak permissions
    
- A password leak
    
- Or a misconfigured script
    

Using this, you move to another user account with higher privileges.

---

### **4. Privilege Escalation — Root**

The final escalation typically involves one of:

- Exploitable SUID binaries
    
- Misconfigured PATH variables
    
- Cron jobs running as root
    
- A mismanaged backup or maintenance script
    

Solving this allows you to become **root** and retrieve the final flag.

---

### **5. Flags**

The room has:

- A _user_ flag (after first or second escalation)
    
- A _root_ flag (after full compromise)
    

---

### ⭐ Summary

In short, **K2** teaches:

- Proper enumeration
    
- Exploitation of simple misconfigurations
    
- Lateral movement between users
    
- Root privilege escalation through standard Linux weaknesses
    

It’s designed to build your confidence in organizing and chaining multiple small weaknesses into a full compromise.




# ✅ **Security Remedies for the K2 Room**

These remedies correspond to the kinds of misconfigurations and vulnerabilities exploited during the room.

---

## **1. Service Hardening & Enumeration Prevention**

### 🔹 **Keep Services Updated**

Make sure all running services (web servers, CMS, SSH, etc.) are patched and updated to their latest stable versions.

### 🔹 **Disable and Remove Unused Services**

If a port or service doesn’t need to be exposed, disable it entirely from the server.

### 🔹 **Use Firewalls**

Limit access to exposed services using:

- **UFW**
    
- **iptables**
    
- Cloud firewalls
    

Only allow ports that must be reachable.

---

## **2. Credentials & Password Fixes**

### 🔹 **Use Strong, Unique Passwords**

The room typically involves weak or exposed passwords.  
Remedy:

- Enforce password complexity and rotation.
    
- Disable plaintext password storage.
    

### 🔹 **Restrict File Permissions**

Ensure:

- No readable credential files for regular users
    
- Sensitive configuration files are owned by root and only readable by root
    

---

## **3. File Permission & User Privilege Issues**

### 🔹 **Audit File Permissions**

Make sure that:

- No unnecessary SUID executables exist
    
- Scripts aren’t writable by low-privilege users
    
- Home directories and users cannot access each other’s sensitive files
    

### 🔹 **Minimal Privilege Principle**

Users should only have the access they need.  
Remove unnecessary group memberships.

---

## **4. Misconfigured Scripts / Cron Jobs**

### 🔹 **Secure Cron Jobs**

Weak cron scripts are a common escalation path.  
Fixes:

- Ensure scripts run by root are not writable by any other user.
    
- Store them in protected directories (e.g. `/root`, `/usr/local/sbin`).
    
- Avoid using world-writable directories.
    

### 🔹 **Avoid Using Wildcard Commands Unsafely**

Cron scripts that use `tar`, `cp`, or wildcards can be exploited.  
Specify full paths in scripts (e.g. `/bin/tar` instead of `tar`).

---

## **5. SUID / Privilege Escalation Vectors**

### 🔹 **Audit SUID Binaries**

Run:

`find / -type f -perm -4000 2>/dev/null`

Remove or secure any SUID binaries that are not essential.

### 🔹 **Avoid Custom SUID Scripts**

Scripts with SUID bits are unsafe and should never be used.

---

## **6. Web Application Security**

### 🔹 **Disable Directory Listing**

Web servers should not expose file structures.

### 🔹 **Validate and Sanitize Input**

If the application accepts input, validate it to prevent injection vulnerabilities.

### 🔹 **Use HTTPS**

Encrypt communications to avoid credential sniffing.

---

## **7. Logging & Monitoring**

### 🔹 **Enable System Logging**

Use:

- syslog
    
- journald
    
- centralized logging if possible
    

### 🔹 **Monitor for Suspicious Activity**

Look for:

- Unexpected new users
    
- Unusual cron jobs
    
- Changed file permissions
    
- Odd processes
    

---

# ⭐ Final Summary

The K2 room teaches that small misconfigurations can lead to full system compromise.  
**The key remedies** are:

- Patch and limit exposed services
    
- Protect file permissions
    
- Use strong credentials
    
- Remove unsafe SUID binaries
    
- Secure cron jobs and scripts
    
- Enforce least privilege
    
- Monitor system activity


# K2 TryHackMe Room — Flowchart / Diagram

```
                   ┌──────────────────────────┐
                   │        START (K2)        │
                   └──────────────┬───────────┘
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │  1. Enumerate Target     │
                    │  - Scan ports            │
                    │  - Probe services        │
                    └─────────────┬─-──────────┘
                                  │
                                  ▼
                     ┌─────────────────────────┐
                     │   2. Identify Entry     │
                     │   - Vulnerable service  │
                     │   - Misconfig/exposure  │
                     └────────────┬─--─────────┘
                                  │
                                  ▼
                     ┌─────────────────────────┐
                     │  3. Initial Foothold    │
                     │  - Exploit service      │
                     │  - Gain low-priv shell  │
                     └────────────┬─--─────────┘
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │  4. Local Enumeration    │
                    │  - Check files, perms    │
                    │  - Find creds/scripts    │
                    └─────────────┬─-──────────┘
                                  │
                                  ▼
                   ┌───────────────────────────┐
                   │  5. User Priv Escalation  │
                   │  - Weak perms / passwords │
                   │  - Misconfigurations      │
                   │  → New higher-user shell  │
                   └───────────────┬───────────┘
                                   │
                                   ▼
                       ┌──────────────────-────┐
                       │ 6. Root Enum          │
                       │ - SUID files          │
                       │ - Cron jobs           │
                       │ - Writable scripts    │
                       └───────────┬───────────┘
                                   │
                                   ▼ 
                     ┌─────────────────────────┐
                     │   7. Root PrivEsc       │
                     │   - Exploit SUID/cron   │
                     │   - PATH abuses         │
                     │   → Gain root           │
                     └─────────────┬───────────┘
                                   │
                                   ▼
                     ┌─────────────────────────┐
                     │     8. Collect Flags    │
                     │     - User flag         │
                     │     - Root flag         │
                     └─────────────────────────┘
```