# Enterprise Home Lab

## Overview

This repository documents the development of an enterprise-style home lab designed to simulate a real corporate IT infrastructure.

The goal is to gain hands-on experience with Windows Server, Active Directory, Linux, networking and cybersecurity while documenting every step of the learning process.

---

<h2 align="center">Lab Architecture</h2>

<p align="center">
  <img src="images/architecture-diagram.png"
       alt="Enterprise Home Lab Architecture"
       width="850">
  <br>
  <em>Figure 1 - Enterprise Home Lab Network Topology</em>
</p>

---

## Project Status

Last updated: September 30, 2026

| Phase | Status |
|---|---|
| Project Planning | ✅ Completed |
| Architecture Design | ✅ Completed |
| VirtualBox Environment | ✅ Completed |
| Windows Server 2022 Deployment | ✅ Completed |
| Static Network Configuration | ✅ Completed |
| Active Directory Domain Services | ✅ Completed |
| DNS Configuration and Validation | ✅ Completed |
| Organizational Unit Structure | ✅ Completed |
| Users and Security Groups | ✅ Completed |
| Administrative Account Separation | ✅ Completed |
| pfSense Firewall | ⏳ Next milestone |
| DHCP Server | ⏳ Planned |
| Windows 11 Domain Client | ⏳ Planned |
| Group Policy | ⏳ Planned |
| File Server | ⏳ Planned |
| SIEM and Monitoring | ⏳ Planned |

---

## Latest Milestone

The first domain controller, DC01, was successfully deployed using
Windows Server 2022.

Current environment:

- Domain: corp.juniorlab.test
- NetBIOS name: CORP
- Domain Controller: DC01
- DC01 address: 192.168.10.10/24
- DNS server: DC01
- Network: VirtualBox Internal Network (LAB-LAN)
- Active Directory DNS and service tests completed successfully
- Departmental OUs, users and security groups created
- Separate standard and privileged administrator accounts implemented

---

## Technologies

- Windows Server 2022
- Windows 11
- Active Directory
- DNS
- DHCP
- Group Policy
- Ubuntu Server
- Docker
- pfSense
- Kali Linux
- Splunk

---

## Learning Goals

This project aims to improve my practical skills in:

- Windows Server Administration
- Active Directory
- Network Services
- Linux Administration
- Cybersecurity
- Infrastructure Documentation

---

## Documentation

- [Project Planning](docs/01-planning.md)
- [Project Roadmap](docs/01-road-map.md)
- [VirtualBox Installation](docs/02-virtualbox-installation.md)
- [Windows Server Installation](docs/03-windows-server-installation.md)
- [Network Configuration](docs/04-network-configuration.md)
- [Active Directory Deployment](docs/05-active-directory.md)
- [DNS Configuration](docs/06-dns.md)

---


## Project Roadmap

| Phase | Status |
|--------|--------|
| Project Planning | ✅ Completed |
| Lab Architecture | ✅ Completed |
| VirtualBox Installation |  ✅ Completed |
| Windows Server Installation |  ✅ Completed |
| Active Directory |  ✅ Completed |
| DNS |  ✅ Completed |
| DHCP | ⏳ Pending |
| GPO | ⏳ Pending |
| File Server | ⏳ Pending |
| Splunk | ⏳ Pending |

---

Project started in July 2026.