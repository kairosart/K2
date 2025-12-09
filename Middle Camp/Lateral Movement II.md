You can't access via `evil-winrm` nor `RDP` with `j.bold's` credentials.


`BloodHound-python` is a tool used to gather Active Directory data, focusing on relationships and attack paths between users, groups, and computers. It collects information such as group memberships and user privileges, which can be visualized to help identify potential privilege escalation opportunities.


## dnschef

Since you could get problems with the name server using the tool that it does not work properly, you must use `dnschef`. DNSChef is a DNS proxy tool used for DNS spoofing, allowing users to intercept and modify DNS requests. It can be used in penetration testing to redirect traffic or simulate malicious DNS responses for specific domains.

#Attaking_Machine 
```
dnschef --fakeip <MACHINE IP>
```

![[dnschef.png]]

## bloodhound-python

#Attaking_Machine 
After setting up DNSChef, we use the following call to bloodhound-python with the credentials from `r.bud` to gather Active Directory data to find an exploit path.

```
bloodhound-python -d k2.thm -c All -u 'r.bud' -p 'vRMkaVgdfxhW!8' -dc k2.thm -ns 127.0.0.1 
```

![[bloodhound.png]]

After this you'll get a few `.json` files on your directory.

![[jsonfiles.png]]

## Bloodhound

At the time of writing this walkthrough I was working on Kali Linux 2025.4.

> [!Note]
So as to install neo4j and bloodhoun I've done a section that does it for you automatically.
[[BloodHound Community Edition (CE) Setup Guide on Kali Linux (VM)UNATTENDED BLOODHOUND INSTALLER — Kali 2025.4 Repo Edition]]


**Next step:** [[Bloodhound analysis]]


