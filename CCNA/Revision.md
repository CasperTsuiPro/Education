[Cisco CCNA 200-301 – The Complete Guide to Getting Certified
](https://www.udemy.com/course/ccna-complete/learn/lecture/7868552#overview)


# OSI Model 7 Layers
# IPv4 Addressing
## IPv4 Subnetting
https://subnetting.org/

# Address Resolution Protocol (ARP)
It's a layer 2 protocol that maps IP addresses to MAC addresses.
⚠️ It is used to resolve the MAC address of a device on the same local network when its IP address is known.

# DNS Configuration
```
enable
config t

# Static DNS
## ip host {name} {address}

# Dynamic DNS
ip name-server {address}
ip domain-lookup
## Options
ip domain-name cisco.com
ip domain list cisco.com
```