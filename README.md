## openvpn-install
OpenVPN [road warrior](http://en.wikipedia.org/wiki/Road_warrior_%28computing%29) installer for Debian, Ubuntu and CentOS.

This script will let you setup your own VPN server in no more than a minute, even if you haven't used OpenVPN before. It has been designed to be as unobtrusive and universal as possible.

### Installation
Run the script and follow the assistant:

`wget https://raw.githubusercontent.com/ishto7/openvpn-install/master/openvpn-install.sh -O openvpn-install.sh && bash openvpn-install.sh`

The recommended settings are for Iranian users. Please use them to avoid being blocked!

Once it ends, you can use the same file for all of your users in the same time.

### Firewall Config
If you're using CSF, you should add some files to your CSF Configs. So first:

`nano /etc/csf/csfpre.sh`

Then add these lines to it:
```#!/bin/bash
iptables -I OUTPUT --match tcp --protocol tcp --dport 137 --jump DROP
iptables -I FORWARD --match tcp --protocol tcp --dport 137 --jump DROP
iptables -I FORWARD --match udp --protocol udp --dport 137 --jump DROP
iptables -I FORWARD -d 192.168.0.0/16 -j DROP
iptables -I FORWARD -d 172.16.0.0/12 -j DROP
iptables -I FORWARD -d 100.64.0.0/10 -j DROP
iptables -I FORWARD -d 127.0.0.0/8 -j DROP
iptables -I FORWARD -d 169.254.0.0/16 -j DROP
iptables -I FORWARD -d 192.0.0.0/24 -j DROP
iptables -I FORWARD -d 192.0.2.0/24 -j DROP
iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -s 10.8.0.0/24 -j ACCEPT
iptables -A FORWARD -j REJECT
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
iptables -t nat -A POSTROUTING -j SNAT --to-source 188.188.188.188
```

The `188.188.188.188` is your public IP. Change it in your file.

### Run Service On Reboot
To make sure the service always runs on reboot:
`systemctl enable openvpn@server`

It's no harm to restart the service when you're done installing:
`systemctl restart openvpn@server`
