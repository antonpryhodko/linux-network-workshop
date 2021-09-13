# linux-network-workshop

## Preparation:
### Install Apache WebServer on CentOS/8
  ```sh

yum install -y httpd epel-release mod_ssl

# open port
firewall-cmd --permanent --add-port=443/tcp

# reload
firewall-cmd --reload
  ```
  
### Trafic generator
```sh
while true; do curl -s http://127.0.0.1; done > /dev/null 2>/dev/null &
```

## CentOS/8 Network tools:
### nmap
```sh
# Network scan
nmap -sP 192.168.0.0/24

# Host scan
nmap <ip>
nmap -F <ip>      # fast
nmap -O <ip>     # detect OS
nmap -sV <ip>     # detect services and versions
nmap -sU <ip>     # detect UDP services

# Alternative host discovery
nmap -PS <ip>     # TCP SYN scan
nmap -PA <ip>     # TCP ACK scan
nmap -PO <ip>     # IP ping
nmap -PU <ip>     # UDP ping

# Alternative service discovery
nmap -sS <ip>      
nmap -sT <ip>
nmap -sA <ip>
nmap -sW <ip>

# Checking firewalls
nmap -sN <ip>
nmap -sF <ip>
nmap -sX <ip>
```
### dig
```sh
dig <domain>
  dig <domain> +noall +answer
  dig <domain> +short
  dig MX <domain>
  dig NS <domain>
  dig ANY <domain>

  dig -x <IP>
  dig -x <IP> +short

  dig @8.8.8.8 <domain>

  dig -f input.txt +noall +answer
```
### curl
```sh
curl ifconfig.me
```
### ethtool
```sh
ethtool eth0                       # Print general info on eth0
ethtool -i eth0                    # Print kernel module info
ethtool -S eth0                    # Print eth0 traffic statistics
ethtool -a eth0                    # Print RX, TX and auto-negotiation settings
ethtool -p eth0                    # Blink LED

# Changing NIC settings...
ethtool -s eth0 speed 100
ethtool -s eth0 autoneg off
ethtool -s eth0 duplex full
ethtool -s eth0 wol g               # Turn on wake-on-LAN
```
### ip
```sh
ip link show
ip link set eth0 up
ip addr show
ip neigh show
```

## Sniffers
### tcpdump
```sh
tcpdump -D
tcpdump --list-interfaces
tcpdump -i eth1
tcpdump –n
tcpdump -c 5
tcpdump –tttt
tcpdump -i eth0 -c 15 port 80
tcpdump host 10.0.0.1
tcpdump -i eth0 icmp
tcpdump -n -i eth0 src 10.0.0.1 and dst port 80
tcpdump -i eth0 not icmp
tcpdump -i eth0 -c 15 -w dump.pcap
tcpdump -i eth0 -c 15 -w dump.pcap
tcpdump -c10 -i eth0 -n -A port 80
tcpdump -c10 -i eth0 -n -X port 80
```
### tshark
```sh
tshark -D
tshark -i eth0
tshark -i eth0 host 10.0.0.1
tshark -i eth0 src host 10.0.0.1
tshark -i eth0 dst host 10.0.0.1
tshark -i eth0 src net 10.0.0.0/24
tshark -i eth0 host 10.0.0.1 and port 80
tshark -i eth0 -a duration:10 -w dump.pcap
tshark 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<>2)) != 0)' -R 'http.request.method == "GET" || http.request.method == "HEAD"'
tshark 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' -R 'http.request.method == "GET" || http.request.method == "HEAD"'
tshark tcp port 80 or tcp port 443 -V -R "http.request || http.response"
tshark -Y http
```
## Certificate
### Self-signed certificate
Generate
```sh 
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```
Review
```sh 
openssl x509 -text -noout -in certificate.pem
```
Combine your key and certificate in a PKCS#12 (P12) bundle:
```sh 
openssl pkcs12 -inkey key.pem -in certificate.pem -export -out certificate.p12
```

Validate your P2 file.
```sh 
openssl pkcs12 -in certificate.p12 -noout -info
```
### Letsencrypt
```sh

# install certbot
yum install certbot python3-certbot-apache

# generate Let’s Encrypt SSL
certbot --apache -d example.com

# renew
certbot renew
crontab -e
# Then add this line:

0 0 * * * /usr/bin/certbot renew > /dev/null 2>&1

# OR

echo "0 0,12 * * * root python3 -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

https://www.ssllabs.com/ssltest/analyze.html?d=example.com

