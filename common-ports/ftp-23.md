# FTP - (23)

#### Scanning

```bash
nmap --script ftp-* -p 21 <ip>
```

#### Banner Grabbing

<pre class="language-bash"><code class="lang-bash">nc -vn &#x3C;IP> 21
<strong>openssl s_client -connect &#x3C;ip>:21 -starttls ftp 
</strong></code></pre>

#### Download all of the files

```bash
wget -m ftp://anonymous:anonymous@<ip>
wget -m --no-passive ftp://anonymous:anonymous@<ip>
```

#### Useful FTP commands

```bash
ls               # file listing
ls -R            # recursive file listing 
get <filename>   # download a file
put <filename>   # upload a file
```





