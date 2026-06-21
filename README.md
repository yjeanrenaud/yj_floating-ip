* yj_floating-ip
A small script to keep a machine's IP address consistent across media and interfaxe changes
I use it to help [pi-hole](https://github.com/pi-hole/pi-hole) to stay available even if the network connection changes on a rPI3. Seldomly, WIFI got disconnected, so I wanted to have ethernet as a backup connection or vice versa. But because tge sevondary DNS server in my LAN is already occupied by another machine, I needed to make sure, the IP affress of that pi-hole stays the same.

This ought to be usefull for other applications, too, but I use it wirh pi-holr.
** Requirements
- A Linux flavour of your choice. I prefer Debian, but any fairly recent distro should work
- `network-manager`. Usually it's already there
- `bash`
- `arping`
- `ip` (from `iproute2`)
- `logger` (from `util-linux`)
you might do
```
sudo apt update
sudo apt install network-manager iproute2 arping
```

** Installation and Usage
1. get the bash-script [90-pihole-floating-ip](90-pihole-floating-ip)
2. edit the file and change IP addresses and interface names according to your needs and system configuration
  - `VIP` in CIDR notation
  - `VIP_ADDR`
  - `WIFI` from ifconfig
  - `ETH` from ifconfig
*Warning ⚠️* </br>
You *ABSOLUTELY MUST* choose a safe IP for the floating IP. The floating IP must be
1. unused,
2. in the same subnet,
3. outside the DHCP pool or reserved,
5. and not already assigned to another host.
Otherwise this can create IP conflicts which might be difficult to debug.
7. copy the finished script to
```
/etc/NetworkManager/dispatcher.d/90-pihole-floating-ip
```
4. make it runable
```
sudo chown root:root /etc/NetworkManager/dispatcher.d/90-pihole-floating-ip
sudo chmod 755 /etc/NetworkManager/dispatcher.d/90-pihole-floating-ip
```
5. test it:
``` 
nmcli device status
ip -br addr

sudo /etc/NetworkManager/dispatcher.d/90-pihole-floating-ip eth0 up

ip -4 addr show dev eth0
ip -4 addr show dev wlan0
journalctl -t pihole-floating-ip -n 50
```
