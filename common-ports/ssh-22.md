# SSH - (22)

#### Scanning

{% code overflow="wrap" %}
```bash
nmap -p22 <ip> --script ssh2-enum-algos,ssh-hostkey,ssh-auth-methods --script-args ssh_hostkey=full,ssh.user=root
```
{% endcode %}

#### Banner Grabbing

```bash
nc -vn <IP> 22        
```

#### Brute Forcing

```
hydra -v -V -u -l root -P <password list> -t 1 <ip> ssh
```







