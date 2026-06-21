* yj_floating-ip
A small script to keep a machine's IP address consistent across media and interfaxe changes
I use it to help [pi-hole](https://github.com/pi-hole/pi-hole) to stay available even if the network connection changes on a rPI3. Seldomly, ethernet got disconnected, so I wanted to have WIFI as a backup connection. But because tge sevondary DNS server in my LAN is already occupied by another machine, I needed to make sure, the IP affress of that pi-hole stays the same..

This ought to be usefull for other applications, too, but I use it wirh pi-holr.
** Requirements
- A Linux flavour of your choice. I prefer Debian, but any fairly recent distro should work
- `network-manager`. Usually it's already there
- `bash`
- `arping`

** Installation and Usage
1. get the bash-script [90-pihole-floating-ip](90-pihole-floating-ip)
2. edit the file and change IP addresses and interface names according to your needs and system configuration
  - `VIP` in CIDR notation
  - `VIP_ADDR`
  - `WIFI` from ifconfig
  - `ETH` from ifconfig
3. copy the script to ```
/etc/NetworkManager/dispatcher.d/90-pihole-floating-ip
```
4. make it runable
```
sudo chmod 755 /etc/NetworkManager/dispatcher.d/90-pihole-floating-ip
```
5. tesr it:
``` 
ip link show wlan0                     nmcli -f GENERAL.HWADDR,GENERAL.CONNECTION device show wlan
```
