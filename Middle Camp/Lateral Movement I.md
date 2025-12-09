
Now you have to get `j.bold` password.

From the notes you found on [[Shell as R.Bud]] you know:

1. `James` has already performed the password change. The kept the same word of `rockyou` but just added two more characters. They however don't have remote access.
2. The password for James was rockyou and Rose says that at the very least it should include more characters. This is also a strong hint as to what wordlist we are going to use.
3. It has to be between `6-12 characters` which coincidentally it does meet already.
4. At least `1 special character` which is what the password is missing.
5. Any `number between 0-999` which also what the password is missing.

## James' password

#Attaking_Machine 

1. The word `rockyou` can be found on `rockyou.txt` so y ou can  to look at any password that contains that.

```
grep -i rockyou rockyou.txt | wc -w
```

  The result is: 7640

2. Copy all those words to a file.
```
cat /usr/share/wordlists/rockyou.txt | grep -i rockyou > rocks.txt
```

3. Bruteforce `j.bold` password with the `rocks.txt` file.

```
~/tools/kerbrute_linux_amd64 bruteuser -d k2.thm --dc <MACHINE IP> rocks.txt j.bold
```

![[j.bold.png]]

j.bold's password is `#8rockyou`.

**Next step:** [[Second Question]]
