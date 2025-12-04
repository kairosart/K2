
# 🧩 **1. What Are LLMNR and NBT-NS?**

### 🔹 **LLMNR (Link-Local Multicast Name Resolution)**

- A Windows protocol used to resolve hostnames on a local network when DNS fails.
    
- Uses multicast (IPv4) → _224.0.0.252_.
    

### 🔹 **NBT-NS (NetBIOS Name Service)**

- Older protocol used for name resolution and network service discovery.
    
- Used when DNS and LLMNR fail.
    

Both protocols allow machines to ask:

> “Who has _HOSTNAME_? Tell me your IP.”

And they **trust whoever answers first**.

---

# 🧨 **2. Poisoning**

Poisoning is when an attacker _pretends_ to be the system another machine is looking for.

### How poisoning works

1. A Windows machine requests a name:
    
    `Who has FILESERVER?`
    
2. DNS fails → Windows sends an LLMNR or NBT-NS request.
    
3. The attacker responds:
    
    `I am FILESERVER → send your login credentials to me.`
    
4. The victim automatically sends **NTLM authentication traffic** (NTLMv2 hash).
    

This lets the attacker:

- Capture NTLMv2 hashes
    
- Attempt offline cracking
    
- Relay the authentication to another host (see below)
    

Tools used:

- **Responder**
    
- **Inveigh**
    
- **Metasploit’s LLMNR spoofing modules**
    

---

# 🔁 **3. NTLM Relay (the powerful part)**

Instead of cracking the hash, you can **relay** the victim’s NTLM authentication to another machine in real time.

This is called **NTLM Relay**.

### How NTLM relay works

1. Victim tries to authenticate to the attacker (thinking it’s real server)
    
2. Attacker forwards the NTLM authentication to a _real_ server
    
3. If the server accepts NTLM (many still do), attacker gets:
    
    - SMB session
        
    - LDAP session
        
    - HTTP session
        
    - Or even **remote code execution**
        

Tools used:

- **ntlmrelayx (Impacket)**
    
- **Responder + ntlmrelayx combination**
    

---

# 🛠 Example attack chain

### 1️⃣ Poisoning

Using Responder:

`responder -I eth0`

Capture NTLM hashes.

### 2️⃣ Relaying

Disable SMB challenge response in Responder, enable HTTP/LDAP relay:

`ntlmrelayx.py -t ldap://10.0.0.5 --dump`

Outcome:

- If the victim is a domain admin: domain takeover
    
- If the victim is a regular user: may still get useful access depending on permissions

| Technique                  | Goal                             | What Happens                                               |
| -------------------------- | -------------------------------- | ---------------------------------------------------------- |
| **LLMNR/NBT-NS Poisoning** | Capture creds                    | Attacker impersonates a host → victim sends NTLMv2 hash    |
| **NTLM Relay**             | Use creds (no cracking required) | Attacker forwards NTLM auth to real server → remote access |