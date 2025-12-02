
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

