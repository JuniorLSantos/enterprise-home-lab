# pfSense Deployment

## Overview

FW01 was deployed as the network security gateway for the lab environment.

## Network Design

WAN:
- Interface: em0
- Mode: DHCP
- Network: VirtualBox NAT

LAN:
- Interface: em1
- Network: LAB-LAN
- Address: 192.168.10.1/24
- DHCP service: Disabled

FW01 provides the default gateway, firewall and NAT services. DHCP is disabled
on the LAN interface because DC01 is the authoritative DHCP server for the
corporate network.

## Validation

Tests performed:

✅ pfSense → Internet
- Ping 8.8.8.8 successful

✅ DC01 → pfSense
- Ping 192.168.10.1 successful

✅ pfSense → DC01
- Ping 192.168.10.10 successful

✅ CLIENT01 → pfSense
- Ping 192.168.10.1 successful with zero packet loss

## Current Architecture

Internet
 |
pfSense FW01
 |
LAB-LAN
 |
DC01

## Service Separation

| System | Network responsibility |
|---|---|
| FW01 | Gateway, firewall and NAT |
| DC01 | Active Directory, DNS and DHCP |

The migration from pfSense DHCP to Windows DHCP is documented in
[Windows DHCP Server Deployment and Validation](12-dhcp-server.md).
