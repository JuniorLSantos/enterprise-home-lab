# Enterprise Home Lab Planning

## Objective

Build an enterprise-style virtual infrastructure to simulate a small corporate environment.

The project will be used to practice Windows Server administration, Active Directory, networking, Linux administration and cybersecurity while documenting every implementation step.

## Technologies

Windows Server 2022 -> Active Directory -> Windows 11 -> Ubuntu -> Kali Linux -> pfSense -> Splunk

---

## Host Machine

- Operating System: Windows 11
- Hypervisor: Oracle VirtualBox

---

## Virtual Machines

## Resource Allocation

| VM | Role | CPU | RAM | Disk |
|----|------|----:|----:|-----:|
| DC01 | Active Directory, DNS, DHCP | 2 | 4 GB | 60 GB |
| CLIENT01 | Windows 11 Client | 2 | 4 GB | 60 GB |
| LINUX01 | Docker Services | 2 | 2 GB | 30 GB |
| FW01 | pfSense Firewall | 2 | 2 GB | 20 GB |
| KALI01 | Penetration Testing | 2 | 2 GB | 40 GB |
| SPLUNK01 | Log Management | 2 | 4 GB | 80 GB |

## Design Decisions

The CPU, memory and storage values were selected to provide a balance between performance and efficient use of the host computer's resources.

The environment is intended for study purposes and simulates a small business infrastructure.
---

## Network Configuration

| Device | IP Address | Purpose |
|--------|------------|---------|
| FW01 | 192.168.10.1 (static) | Gateway / Firewall |
| DC01 | 192.168.10.10 (static) | Domain Controller / DNS / DHCP |
| CLIENT01 | DHCP (validated lease: 192.168.10.100) | Windows Client |
| LINUX01 | 192.168.10.30 (planned static) | Docker Server |
| KALI01 | 192.168.10.40 (planned static) | Security Testing |
| SPLUNK01 | 192.168.10.50 (planned static) | Log Management |

---

## DHCP Configuration

| Setting | Value |
|---|---|
| DHCP server | DC01 (`192.168.10.10`) |
| Scope | CORP-LAN |
| Dynamic range | `192.168.10.100` - `192.168.10.200` |
| Default gateway | `192.168.10.1` |
| DNS server | `192.168.10.10` |
| DNS suffix | `corp.juniorlab.test` |
| Lease duration | 8 days |

The DHCP service on the pfSense LAN interface is disabled so that DC01 is the
only DHCP server on `LAB-LAN`.

---

## Service Roadmap

- Active Directory - Completed
- DNS - Completed
- DHCP - Completed
- Group Policy - Completed
- File Server - Planned
- Docker - Planned
- VPN - Planned
- Splunk - Planned

---

## Lessons Learned

- Learned how to design the initial infrastructure before deployment.
- Defined the virtual machines required for the lab.
- Planned the network addressing scheme.
- Centralized DHCP on Windows Server and separated infrastructure addresses
  from the dynamic client pool.

---
