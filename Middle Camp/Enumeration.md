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

![[nmap2.png]]


There's not [[Web Server]] so the server seems a pure AD machine.

### Ports

A deeper enumeration to see the ports and more information.

```
nmap k2.thm -sC -sV -p 53,88,135,139,389,445,464,593,636,3268,3269,3389,5985,9389,49669,49670,49671,49674,49679,49708 -Pn
```

![[PORT2.png|499x171]]


You can determine the name of the machine `K2SERVER`. 

Add `K2SERVER.K2.THM` to our `/etc/host` file.


---

As already mentioned on the main page, this is worthy of a network. The machines are related to each other. And y ou need to pay attention to the note in the Room description.

> *Use all of the information gathered from your previous findings in order to keep making your way to the top.*

**Next step:** [[Middle Camp/Foothold]]
