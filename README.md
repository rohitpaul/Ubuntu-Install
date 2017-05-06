# Ubuntu-Install
Code and scripts to install and mainatin my ubuntu server. This provides means to document and share findings from the web for easy furture access.


#Post install
Once installed, first step add static ip address

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
