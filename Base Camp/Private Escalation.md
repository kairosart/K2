You are connected via SSH as user `james`.

#SSH
1. Enumerate the `id`.
```
id
```

![[id.png]]

You can see `james` is part of the `adm` group,

2. You know about the user `rose`. So take a look at the logs. In `/var/logs`, run:
```
grep -iR` `rose`
```
You'll find all entries that have `rose` as a topic in any way. 

![[rose.png]]

In `nginx/access.log`, you found the failed login attempt of `rose` with a `clear text password`. 

`rose:RdzQ7MSKt)fNaz3!`

If you try to change user to rose you get `su: Authentication failure`.

## Getting root

Use the rose's password to change to root.

```
su - root
```

![[root.png]]

