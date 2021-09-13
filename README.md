# linux-network-workshop

## Preparation:
### Install Apache WebServer on CentOS/8
  ```sh
  yum install -y httpd
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
### tshark
## Certificate
### Self-signed certificate
### Letsencrypt
