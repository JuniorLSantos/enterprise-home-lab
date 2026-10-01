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

## Validation

Tests performed:

✅ pfSense → Internet
- Ping 8.8.8.8 successful

✅ DC01 → pfSense
- Ping 192.168.10.1 successful

✅ pfSense → DC01
- Ping 192.168.10.10 successful

## Current Architecture

Internet
 |
pfSense FW01
 |
LAB-LAN
 |
DC01