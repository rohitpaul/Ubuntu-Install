# Ubuntu-Install
Code and scripts to install and mainatin my ubuntu server. This provides means to document and share findings from the web for easy furture access.

# Post install
## Static IP
Edit */etc/network/interfaces*
```
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface static ip to 192.168.1.199
auto enp0s3
iface enp0s3 inet static
    address 192.168.1.199
    netmask 255.255.255.0
    broadcast 192.168.1.255
    gateway 192.168.1.1
    dns-nameservers 192.168.1.1 8.8.8.8
```

## Webmin
```
sudo sh -c 'echo "deb http://download.webmin.com/download/repository sarge contrib" > /etc/apt/sources.list.d/webmin.list'
wget -qO - http://www.webmin.com/jcameron-key.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install webmin
```
