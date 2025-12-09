On the previous step the shell you got is R.Bud.

#Winrm
- List the files.

```
Evil-WinRM PS C:\Users\r.bud\Documents> dir
```

![[jamesdpcs.png]]

- Look at the `notes.txt` content.

```
type notes.txt
```

![[hamesnote.png]]

- Look at the second note: `note_to_james.txt`.

```
type note_to_james.txt
```

![[jamesnote2.png]]

From the two notes we know a few things:

1. The password for `James` was `rockyou` and Rose says that at the very least it should include more characters. This is also a strong hint as to what wordlist we are going to use.
2. It has to be between `6-12` characters which coincidentally it does meet already.
3. At least `1` special character which is what the password is missing.
4. Any number between `0-999` which also what the password is missing.
5. James has already performed the password change. They kept the same word of rockyou but just added two more characters. They however don't have remote access.

**Next step:** [[Lateral Movement I]]


