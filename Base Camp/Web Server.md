
#Attaking_Machine 
Take a look at  http://k2.thm.

![[home.png]]

There's nothing interesting. It seems a static website.

## Subdomains

To enumerate potential subdomains use `gobuster`.

```
gobuster vhost -u http://k2.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

![[subdomains.png]]


- Add both domains to /etc/hosts.

### Visit the subdomains

http://admin.k2.thm/

![[admin.png|424x491]]

http://it.k2.thm/

![[it.png|421x510]]

