> [!Note] 
>Stop the first machine and start the second. 

## Nmap

The enumeration process starts all over again.
### Services

#Attaking_Machine 
 Run another Nmap scan.

```
nmap -p- k2.thm -Pn
```

![[nmap3 1.png]]

### **🖥️ Interpreting Each Open Port**

| Port         | Service                | Meaning                                                                    |
| ------------ | ---------------------- | -------------------------------------------------------------------------- |
| **53/tcp**   | DNS                    | Domain Name System — standard for name resolution. DCs always run this.    |
| **88/tcp**   | Kerberos               | Authentication protocol used in Active Directory.                          |
| **135/tcp**  | MSRPC                  | Used for remote procedure calls, DCOM — common on Windows hosts.           |
| **139/tcp**  | NetBIOS-SSN            | File/print sharing legacy NetBIOS.                                         |
| **389/tcp**  | LDAP                   | Directory services in Active Directory. Very important for domain queries. |
| **445/tcp**  | SMB (microsoft-ds)     | File sharing, authentication (NTLM), common attack surface.                |
| **464/tcp**  | kpasswd5               | Kerberos password change protocol.                                         |
| **593/tcp**  | HTTP RPC-EPMAP         | RPC endpoint mapping over HTTP.                                            |
| **636/tcp**  | LDAPS                  | Secure LDAP (TLS).                                                         |
| **3268/tcp** | Global Catalog (LDAP)  | AD global catalog — allows forest-wide searches.                           |
| **3269/tcp** | Global Catalog (LDAPS) | Secure version of port 3268.                                               |
| **3389/tcp** | RDP                    | Remote Desktop. Major admin + attack surface.                              |
| **5985/tcp** | WinRM (HTTP)           | Remote management (PowerShell remoting).
### Ports

A deeper enumeration to see the ports and more information.

```
nmap k2.thm -sC -sV -p 53,88,135,139,389,445,464,593,636,3268,3269,3389,5985 -Pn
```

![[ports3.png|1109x819]]

You can determine the name of the machine `K2ROOTDC`. 

Add `K2RootDC.k2.thm` to our `/etc/host` file.

**Next step:** [[The Summit/Foothold|Foothold]]

