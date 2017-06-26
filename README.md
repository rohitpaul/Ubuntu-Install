# Ubuntu-Install
Code and scripts to install and mainatin my ubuntu server. This provides means to document and share findings from the web for easy furture access.

# Post install
## Static IP
Edit `/etc/network/interfaces`

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
### Run Webmin on Subdomin using proxy

This method can also be used to make Webmin accessible via an Apache virtual host, like http://webmin.yourdomain.com/. The steps to follow are :
Make sure mod_proxy is installed on your Apache webserver.
Add a virtual host to your Apache configuration file like:
```
<VirtualHost _default_>
ServerName webmin.yourdomain.com
ProxyPass / http://localhost:10000/
ProxyPassReverse / http://localhost:10000/
SSLProxyEngine on
<Proxy *>
allow from all
</Proxy>
</VirtualHost>
```
If the virtual host uses a port other than 80 (like 9000), add a line to your Apache configuration like :
`Listen 9000`
In `/etc/webmin/config`, add the `line referer=apachehost`, where apachehost is the hostname from the URL used to access Webmin via Apache. If the referer line already has some hosts listed, add apachehost to it.
Re-start Apache to apply the configuration.
No changes need to be made to `/etc/webmin/config`, because no prefix is appended to the URL path.



### Flush All Rules, Delete All Chains, and Accept All
This section will show you how to flush all of your firewall rules, tables, and chains, and allow all network traffic.

Note: This will effectively disable your firewall. You should only follow this section if you want to start over the configuration of your firewall.

First, set the default policies for each of the built-in chains to ACCEPT. The main reason to do this is to ensure that you won't be locked out from your server via SSH:
```
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
```
Then flush the nat and mangle tables, flush all chains (-F), and delete all non-default chains (-X):

```sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo iptables -F
sudo iptables -X
```
Your firewall will now allow all network traffic. If you list your rules now, you will will see there are none, and only the three default chains (INPUT, FORWARD, and OUTPUT) remain.
