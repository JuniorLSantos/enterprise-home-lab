# Network Configuration

## Overview

This document describes the initial network configuration of the
enterprise home lab.

The lab currently uses an isolated VirtualBox Internal Network named
`LAB-LAN`. This network allows the virtual machines to communicate with
each other without exposing the lab directly to the physical network.

---

## Network Design

| Component | Configuration |
|---|---|
| VirtualBox network | LAB-LAN |
| Network type | Internal Network |
| Network address | 192.168.10.0/24 |
| Domain Controller | 192.168.10.10 |
| Planned gateway | 192.168.10.1 |
| DNS server | 192.168.10.10 |
| DHCP | Not configured yet |

---

## DC01 Static IP Configuration

The domain controller uses the following network configuration:

| Setting | Value |
|---|---|
| Hostname | DC01 |
| IPv4 address | 192.168.10.10 |
| Subnet prefix | /24 |
| Subnet mask | 255.255.255.0 |
| Default gateway | 192.168.10.1 |
| Preferred DNS | 192.168.10.10 |

A static IP address is required because client computers and Active
Directory services must always be able to locate the domain controller
at the same address.

---

## VirtualBox Internal Network

The DC01 network adapter was connected to the VirtualBox Internal
Network named `LAB-LAN`.

An Internal Network was selected to:

- Isolate the lab from the physical home network.
- Prevent accidental changes to the production home network.
- Allow communication between laboratory virtual machines.
- Prepare the environment for a future pfSense firewall.
- Provide full control over DNS, DHCP and routing.

All virtual machines connected to the lab must use the exact same
Internal Network name.

---

## Gateway Reservation

The address `192.168.10.1` is reserved for the future pfSense firewall.

At this stage, no device is using that address. Therefore, the gateway
does not respond and the internal network does not yet have internet
access.

This is expected behavior until pfSense is deployed.

---

## DNS Configuration

DC01 uses its own address, `192.168.10.10`, as the preferred DNS server.

Active Directory relies on DNS records to locate services such as:

- Domain controllers
- Kerberos authentication
- LDAP
- Global Catalog
- Domain services

Domain members will also use DC01 as their DNS server.

Public DNS servers should not be configured directly on domain clients.
External name resolution will later be handled through DNS forwarding
and the pfSense gateway.

---

## Validation Commands

The network configuration can be inspected with:

```powershell
ipconfig /all
Get-NetIPConfiguration
Get-DnsClientServerAddress
```

The configured address can be tested with:

```powershell
Test-NetConnection 192.168.10.10
```

After more machines are added to the lab, communication can be tested
with:

```powershell
ping 192.168.10.10
nslookup corp.juniorlab.test
```

---

## Current Limitations

The current network does not yet provide:

- Internet access
- Routing between networks
- DHCP address assignment
- Firewall filtering
- Network Address Translation

These services will be implemented during the pfSense deployment.

---

## Lessons Learned

- Domain controllers require predictable IP addresses.
- Active Directory clients must use the internal DNS server.
- VirtualBox Internal Networks isolate laboratory traffic.
- A configured gateway does not work until a device responds at that
  address.
- DNS, routing and internet connectivity are separate services.

---

## Next Steps

- Deploy pfSense.
- Configure the LAN interface as `192.168.10.1`.
- Provide internet access to `LAB-LAN`.
- Configure DNS forwarding.
- Configure DHCP.
- Deploy a Windows 11 domain client.