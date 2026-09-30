
# Active Directory Deployment

## Overview

This document describes the deployment and initial configuration of
Active Directory Domain Services in the enterprise home lab.

The Windows Server 2022 virtual machine named DC01 was promoted to the
first domain controller of a new Active Directory forest.

---

## Environment

| Component | Configuration |
|---|---|
| Server name | DC01 |
| Operating system | Windows Server 2022 Standard Evaluation |
| Domain name | corp.juniorlab.test |
| NetBIOS name | CORP |
| Domain Controller IP | 192.168.10.10/24 |
| DNS Server | 192.168.10.10 |
| VirtualBox Network | LAB-LAN |
| Forest functional level | Windows Server 2016 |
| Domain functional level | Windows Server 2016 |

---

## AD DS Installation

The Active Directory Domain Services role was installed through
Server Manager.

Installing the AD DS role only adds the necessary components to the
server. The server becomes a domain controller only after completing
the domain controller promotion process.

---

## Domain Controller Promotion

DC01 was promoted as the first domain controller of a new forest.

The following configuration was used:

- Root domain name: `corp.juniorlab.test`
- NetBIOS domain name: `CORP`
- DNS Server: Enabled
- Global Catalog: Enabled
- Read-Only Domain Controller: Disabled

A Directory Services Restore Mode password was configured and stored
securely outside the repository.

---

## Organizational Unit Structure

The following Organizational Unit structure was created:

```text
Corp
├── Admin Accounts
├── Computers
│   ├── Laptops
│   └── Workstations
├── Groups
├── Servers
├── Service Accounts
└── Users
    ├── Finance
    ├── HR
    ├── IT
    └── Sales