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

After this you'll get a few .json files on your directory.

![[jsonfiles.png]]
## neo4j

#Attaking_Machine 
Run neo4j.

```
sudo neo4j start
```

![[neo4j.png]]

## Bloodhound

### Install bloodhound on Kali Linux

https://www.kali.org/tools/bloodhound/

1. Run:
```
sudo apt update && sudo apt install -y bloodhound
```



