# Network Configuration

## Overview

This document describes the network configuration of the enterprise home lab.

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
| Gateway | 192.168.10.1 (FW01 / pfSense) |
| DNS server | 192.168.10.10 |
| DHCP server | 192.168.10.10 (DC01) |
| DHCP scope | 192.168.10.100 - 192.168.10.200 |

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
- Route controlled lab traffic through the pfSense firewall.
- Provide full control over DNS, DHCP and routing.

All virtual machines connected to the lab must use the exact same
Internal Network name.

---

## Gateway and Firewall

The address `192.168.10.1` is assigned to the LAN interface of FW01, the
pfSense firewall.

FW01 provides the default gateway, firewall policy and NAT path for systems on
`LAB-LAN`. Its WAN interface uses a VirtualBox NAT network, while its LAN
interface connects to the internal corporate network.

The DHCP service on the pfSense LAN interface is disabled. This prevents it
from competing with the Windows DHCP Server on DC01.

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

Public DNS servers are not configured directly on domain clients. External
queries are sent to DC01 and handled through DNS forwarding, with pfSense
providing the routed path to the internet.

---

## DHCP Configuration

DC01 provides dynamic IPv4 configuration to client systems through the
`CORP-LAN` scope.

| Setting | Value |
|---|---|
| Address pool | `192.168.10.100` - `192.168.10.200` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `192.168.10.1` |
| DNS server | `192.168.10.10` |
| DNS suffix | `corp.juniorlab.test` |
| Lease duration | 8 days |

Addresses below `.100` remain available for infrastructure systems that
require documented static configurations. CLIENT01 was migrated from its
original static address to DHCP and received `192.168.10.100` during
validation.

The full deployment and validation process is documented in
[Windows DHCP Server Deployment and Validation](12-dhcp-server.md).

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

DHCP configuration and the current lease can be inspected with:

```powershell
Get-DhcpServerv4Scope
Get-DhcpServerv4Lease -ScopeId 192.168.10.0
```

---

## Current State and Limitations

The network currently provides:

- Controlled internet access through pfSense NAT.
- Firewall and default-gateway services on FW01.
- Active Directory-integrated DNS on DC01.
- Dynamic IPv4 configuration from the Windows DHCP Server on DC01.
- Internal and external DNS resolution for CLIENT01.

Current limitations include a single IPv4 subnet, no DHCP failover and no
redundant domain controller. These are acceptable for the present lab scope.

---

## Lessons Learned

- Domain controllers require predictable IP addresses.
- Active Directory clients must use the internal DNS server.
- VirtualBox Internal Networks isolate laboratory traffic.
- Only one DHCP server should answer clients on the same broadcast domain.
- Infrastructure addresses and the dynamic client pool should not overlap.
- DNS, routing and internet connectivity are separate services.

---

## Next Steps

- Deploy SMB file services and group-based access controls.
- Add the planned Linux and monitoring systems.
- Consider DHCP reservations for systems that require predictable addresses.
- Add a reverse lookup zone and PTR records.
