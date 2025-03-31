
General network scan:
```bash
nmap -sS IpAddress
``` 
>[!note] Or use these flags
>```-sT``` for TCP or ```-sU``` for UDP.
>```-sS``` -> specifies a SYN scan (also called stealth scan). It sends a SYN packet that is like a handshake request and if the port is open, the target responds with SYN-ACK and instead of completing the handshake, Nmap sends a RSP (reset) packet, leaving no connection logs. If the port is closed, the target sends an RST back.
  
Perform a quick scan for open ports followed by a full port scan

```bash
nmap -p0- -v -A -T4 IpAddress
```
>[!note] What does each flag do?
>```-p0-``` asks Nmap to scan every possible TCP port.
>```-v``` asks Nmap to be verbose about it.
>```-A``` enables aggressive tests such as remote OS detection, service/version detection, and the Nmap Scripting Engine (NSE).
>```-T4``` enables a more aggressive timing policy to speed up the scan.

Other useful flags:

>[!Note] Other useful flags
>```-p-``` scan for all ports
>```-F``` scan top 100 ports (common ports).
>```-T <0-5>``` specifies how fast to perform the scan, the slower (0) the stealthy.
>```-sA``` Performs ACK scan on specified ports
>```-Pn``` Disables ICMP Echo requests
>```-n``` Disables ARP ping
>```-D``` Decoy scanning method: generates various random IP addresses inserted into the IP header to disguise the origin of the packet sent.
>```--disable-arp-ping``` Disables ARP ping.
>```--packet-trace``` Shows all packets sent and received
>```--min-parallelism < number >```. frequency.
>```--max-rtt-timeout < time >``` set packet timeout.
>```--min-rate < number >``` set how many packets simultaneously are sent
>```--max-retries < number >``` set number of retries.
>```--script``` run a script from the Nmap Scripting Engine (NSE) -> check [Nmap NSE Scripts](https://nmap.org/nsedoc/scripts/)

Regarding Firewalls, IDPs and IDPs:
>[!Note] Tips
>Nmap's TCP ACK scan (-sA) method is much harder to filter for firewalls and IDS/IPS systems than regular SYN (-sS) or Connect scans (sT) because they only send a TCP packet with only the ACK flag. When a port is closed or open, the host must respond with an RST flag. Unlike outgoing connections, all connection attempts (with the SYN flag) from external networks are usually blocked by firewalls. However, the packets with the ACK flag are often passed by the firewall because the firewall cannot determine whether the connection was first established from the external network or the internal network.

To scan a DNS server, it usually uses UDP, to get some information about the domain server you can use:
```bash
nmap -p 53 -sU -sV -vv targetIP
```
>[!Note]
>-p 53 → Scans port 53 only.
>-sU → Scans UDP ports.
>-sV → Enables service version detection.
>-vv → Increases verbosity.

For TCP all ports:
```bash
nmap -p- -sS -sV -vv 10.129.206.60
```

