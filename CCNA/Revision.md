# Table of Contents

- [Table of Contents](#table-of-contents)
- [OSI Model 7 Layers](#osi-model-7-layers)
- [IPv4 Addressing](#ipv4-addressing)
  - [IPv4 Subnetting](#ipv4-subnetting)
- [Address Resolution Protocol (ARP)](#address-resolution-protocol-arp)
- [Basic Commands](#basic-commands)
  - [show ip interface brief](#show-ip-interface-brief)
- [DNS Configuration](#dns-configuration)
- [IP Routing](#ip-routing)
  - [Static Routing](#static-routing)
  - [Possible ICMP-type Values](#possible-icmp-type-values)
  - [Possible Output Characters From the Ping Facility](#possible-output-characters-from-the-ping-facility)
  - [IP Traceroute Text Characters](#ip-traceroute-text-characters)
- [Cisco Discovery Protocol (CDP)](#cisco-discovery-protocol-cdp)
- [⚠️ Link Layer Discovery Protocol (LLDP)](#️-link-layer-discovery-protocol-lldp)
- [RIP Routing Protocol](#rip-routing-protocol)
- [OSPF Routing Protocol](#ospf-routing-protocol)


[Cisco CCNA 200-301 – The Complete Guide to Getting Certified
](https://www.udemy.com/course/ccna-complete/learn/lecture/7868552#overview)


# OSI Model 7 Layers
# IPv4 Addressing
## IPv4 Subnetting
https://subnetting.org/

# Address Resolution Protocol (ARP)
It's a layer 2 protocol that maps IP addresses to MAC addresses.
⚠️ It is used to resolve the MAC address of a device on the same local network when its IP address is known.

# Basic Commands
```
show version                # Show the version of the device
show running-config         # Show the running configuration
show startup-config         # Show the startup configuration
show ip interface brief     # Show the IP interface brief
show ip route               # Show the routing table
show run interface vlan 1   # Show the configuration of the interface
show interface vlan 1       # Show detailed information about the interface
show arp                    # Show the ARP table
show ip arp                 # Show the ARP table
ping                        # Ping a device
traceroute                  # Traceroute to a device
show ip protocols           # Show the IP protocols
```

## show ip interface brief
| Interface | IP-Address  | OK | Method | Status | Protocol |
|-----------|-------------|----|--------|--------|----------|
| Vlan1     | 192.168.1.1 | up | dhcp   | up     | up       |
| Vlan2     | 192.168.2.1 | up | dhcp   | up     | up       |
| Vlan3     | 192.168.3.1 | up | dhcp   | up     | up       |
Where the methods can be:
  dhcp, manual (through CLI), NVRAM (from startup config), IPCP,
  DHCP, unassigned (no IP address), unset (no IP address through CLI), other (unknown)
[What's the meaning of method in show ip int bri?](https://community.cisco.com/t5/switching/what-s-the-meaning-of-method-in-show-ip-int-bri/td-p/1346081)

# DNS Configuration
```
Switch# enable
Switch# configure terminal

# Static DNS
Switch(config)# ip host {name} {address}

# Dynamic DNS
Switch(config)# ip name-server {address}
Switch(config)# ip domain-lookup
## Options
Switch(config)# ip domain-name cisco.com
Switch(config)# ip domain list cisco.com
```

# IP Routing
## Static Routing
```
Switch(config)# ip route {network} {mask} {next-hop}
Switch(config)# ip route 0.0.0.0 0.0.0.0 {next-hop} # Default route
```
[IP Traceroute Text Characters](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-121-mainline/12778-ping-traceroute.html)

## Possible ICMP-type Values
|-----------------------------------------------------------------------------------------|
| ICMP Type | Description                                                                 |
|-----------------------------------------------------------------------------------------|
| 0         | echo-reply                                                                  |
| 3         | destination unreachable (net, host, protocol, port, fragmentation needed)   |
| 8         | echo                                                                        |
| 11        | time-exceeded (time to live exceeded, fragment reassembly time exceeded)    |
| 12        | parameter-problem                                                           |
|-----------------------------------------------------------------------------------------|

## Possible Output Characters From the Ping Facility
|-----------------------------------------------------------------------------------------|
| Character | Description                                                                 |
|-----------------------------------------------------------------------------------------|
| !         | Each exclamation point indicates receipt of a reply.                        |
| .         | Each period indicates the network server timed out as it waits for a reply. |
| U         | A destination unreachable error PDU was received.                           |
| Q         | Source quench (destination too busy).                                       |
| M         | Could not fragment.                                                         |
| ?         | Unknown packet type.                                                        |
| &         | Packet lifetime exceeded.                                                   |
|-----------------------------------------------------------------------------------------|

## IP Traceroute Text Characters
|--------------------------------------------------------------------------------|
| Character | Description                                                        |
|--------------------------------------------------------------------------------|
| nn msec   | For each node, the round-trip time in milliseconds for the specified number of probes |
| *         | The probe timed out                                                |
| A         | Administratively prohibited (example, access-list)                 |
| Q         | Source quench (destination too busy)                               |
| I         | User interrupted test                                              |
| U         | Port unreachable                                                   |
| H         | Host unreachable                                                   |
| N         | Network unreachable                                                |
| P         | Protocol Unreachable                                               |
| T         | Timeout                                                            |
| ?         | Unknown packet type                                                |
|--------------------------------------------------------------------------------|

# Cisco Discovery Protocol (CDP)
It's a Cisco proprietary layer 2 protocol that allows devices to discover each other on the network.
```
# Enable CDP
Switch# configure terminal
Switch(config)# cdp run

# Disable CDP
Switch# configure terminal
Switch(config)# no cdp run # Disable CDP on all interfaces
Switch(config)# cdp disable # Disable CDP on all interfaces

## ⚠️ Disable CDP on a specific outgoing interface
Switch(config)# interface {interface}
Switch(config-if)# no cdp enable

# Show CDP information
Switch# show cdp neighbors
Switch# show cdp neighbors detail
Switch# show cdp traffic
Switch# show cdp cache
```

# ⚠️ Link Layer Discovery Protocol (LLDP)
It's an open standard layer 2 protocol that allows devices to discover each other on the network.
It may be disabled by default on some Cisco devices.
It's only supported on physical interfaces, not on virtual interfaces.
It can only discover up to 1 device per port.
It can discover Linux servers

```
# Enable LLDP
Switch# configure terminal
Switch(config)# lldp run

# Disable LLDP
Switch# configure terminal
Switch(config)# no lldp run

## ⚠️ Disable LLDP on a specific outgoing interface
Switch(config)# interface {interface}
Switch(config-if)# no lldp transmit
Switch(config-if)# no lldp receive

# Show LLDP information
Switch# show lldp neighbors
Switch# show lldp neighbors detail
Switch# show lldp traffic
```

# RIP Routing Protocol
It's a distance-vector routing protocol.
```
Switch(config)# router rip
Switch(config-router)# network {network}
```

# OSPF Routing Protocol
It's a link-state routing protocol.
