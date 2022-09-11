# Create an Access Point from Radxa Zero

[home](../README.md)

[back](radxa-flash-backup-image.md)

---
## Introduction
How to create an access point from Radxa Zero, some steps are taken from these websites
- https://nims11.wordpress.com/2012/04/27/hostapd-the-linux-way-to-create-virtual-wifi-access-point/
- https://www.tecmint.com/setup-a-dns-dhcp-server-using-dnsmasq-on-centos-rhel/
- https://computingforgeeks.com/install-and-configure-dnsmasq-on-ubuntu/

---

## Steps

```bash
sudo nmcli device wifi connect "ssid" password "password"
```

3 files that will created/changed:
- `~/dnsmasq.conf` -> `/etc/dnsmasq.conf`
- `~/hostapd.conf`
- `~/resolv.conf` -> `/etc/resolv.conf`


```bash
cd 
sudo apt-get install -y dnsmasq
cp /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
#sudo systemctl start dnsmasq.service
sudo systemctl status dnsmasq.service
```

Create in `~/` directory, using `nano dnsmasq.conf` and copy the following down
```bash
# Interface to bind to
interface=wlan0
# Specify starting_range,end_range,lease_time
dhcp-range=10.0.0.10,10.0.0.200,12h
```
After this backup the original file and then overwrite with the new file from root directory
```bash
sudo cp /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
sudo cp ~/dnsmasq.conf /etc/dnsmasq.conf
```

Create in `~/` directory, using `nano hostapd.conf` and copy the following down
```bash
interface=wlan0
ssid=rock
hw_mode=g
channel=1
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=password
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

Create in `~/` directory, using `nano resolv.conf` and copy the following down
```bash
#set localhost as the only nameserver
nameserver 127.0.0.1
```
After this backup the original file and then overwrite with the new file from root directory
```bash
sudo cp /etc/resolv.conf /etc/resolv.conf.orig
sudo cp ~/resolv.conf /etc/resolv.conf
```

**Important to take down any current connection to wlan0**
```bash
ifconfig
sudo nmcli con down id <ssid>
```

```bash
sudo ip addr add 10.0.0.1/24 dev wlan0
ifconfig # Check to see whether your ip has changed
sudo killall dnsmasq
sudo systemctl start dnsmasq.service
cd
sudo hostapd hostapd.conf
```

## Results upon successful setup 
Successful AP configuration and launch of `hostapd` and handshake being completed, output of `sudo hostapd hostapd.conf`
```bash
#rock@radxa-zero:~$ sudo hostapd hostapd.conf                                                                       
Configuration file: hostapd.conf                                                                                   
wlan0: Could not connect to kernel driver                                                                          
Using interface wlan0 with hwaddr xx:xx:xx:xx:xx:xx and ssid "rock"                                                
wlan0: interface state UNINITIALIZED->ENABLED                                                                      
wlan0: AP-ENABLED                                                                                                  
wlan0: STA xx:xx:xx:xx:xx:xx IEEE 802.11: associated                                                               
wlan0: AP-STA-CONNECTED xx:xx:xx:xx:xx:xx                                                                         
wlan0: STA xx:xx:xx:xx:xx:xx RADIUS: starting accounting session 75D9D27491207E36                                  
wlan0: STA xx:xx:xx:xx:xx:xx WPA: pairwise key handshake completed (RSN)
```

Successful output of `sudo systemctl status dnsmasq.service`
```bash
dnsmasq.service - dnsmasq - A lightweight DHCP and caching DNS server
     Loaded: loaded (/lib/systemd/system/dnsmasq.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-09-08 15:53:46 UTC; 29s ago
    Process: 2587 ExecStartPre=/usr/sbin/dnsmasq --test (code=exited, status=0/SUCCESS)
    Process: 2588 ExecStart=/etc/init.d/dnsmasq systemd-exec (code=exited, status=0/SUCCESS)
    Process: 2596 ExecStartPost=/etc/init.d/dnsmasq systemd-start-resolvconf (code=exited, status=0/SUCCESS)
   Main PID: 2595 (dnsmasq)
      Tasks: 1 (limit: 4248)
     Memory: 524.0K
     CGroup: /system.slice/dnsmasq.service
             └─2595 /usr/sbin/dnsmasq -x /run/dnsmasq/dnsmasq.pid -u dnsmasq -r /run/dnsmasq/resolv.conf -7 /etc/dn
smasq.d,.dpkg-dist,.dpkg-old,.dpkg-new --local-service --trust-anchor=.,20326,8,2,e06d44b80b8f1d39a95c0b0d7c65d0845
8e880409bbc683457104237c7f8ec8d

Sep 08 15:53:46 radxa-zero dnsmasq[2595]: compile time options: IPv6 GNU-getopt DBus i18n IDN DHCP DHCPv6 no-Lua TF
TP conntrack ipset auth nettlehash DNSSEC loop-detect inotify dumpfile
Sep 08 15:53:46 radxa-zero dnsmasq-dhcp[2595]: DHCP, IP range 10.0.0.10 -- 10.0.0.200, lease time 12h
Sep 08 15:53:46 radxa-zero dnsmasq[2595]: no servers found in /run/dnsmasq/reso
lv.conf, will retry
Sep 08 15:53:46 radxa-zero dnsmasq[2595]: read /etc/hosts - 2 addresses
Sep 08 15:53:46 radxa-zero dnsmasq[2621]: /etc/resolvconf/update.d/libc: Warning: /etc/resolv.conf is not a symboli
c link to /run/resolvconf/resolv.conf
Sep 08 15:53:46 radxa-zero systemd[1]: Started dnsmasq - A lightweight DHCP and caching DNS server.
```

---

## Troubleshooting
If there is an error of the port being occupied (etc `port 53`), there are 2 ways to solve this, first is to permanently remove `systemd-resolved` as shown [here](https://askubuntu.com/a/1170073) 

**Method 1**
```bash
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
sudo systemctl mask systemd-resolved
```

**Method 2**
```bash
# https://askubuntu.com/questions/500162/how-do-i-restart-dnsmasq
ps aux | grep dns
# There may be a nobody inside the list, kill that blocking process
sudo kill <PID>
```

To restart the network service you can `sudo service network-manager restart`

---

After this you can proceed to [Remove Autoboot Countdown](radxa-remove-autoboot-countdown.md)