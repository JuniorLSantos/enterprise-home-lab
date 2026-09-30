
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
    
    ```

This structure separates users, computers, servers, service accounts
and administrative accounts.

It will also allow Group Policies to be applied to specific departments
and types of devices in future stages of the project.

---

## Security Groups

The following Global Security groups were created:

| Group | Purpose |
|---|---|
| GG_IT_Users | IT department users |
| GG_HR_Users | Human Resources users |
| GG_Finance_Users | Finance department users |
| GG_Sales_Users | Sales department users |

The `GG` prefix identifies the groups as Global Groups.

Users were added to groups according to their departments instead of
being assigned permissions individually.

---

## User Accounts

Test users were created to simulate employees from different departments.

| User | Department | Security Group |
|---|---|---|
| Junior Santos | IT | GG_IT_Users |
| Helena Costa | HR | GG_HR_Users |
| Marcos Ribeiro | Finance | GG_Finance_Users |
| Beatriz Alves | Sales | GG_Sales_Users |

These accounts are used only inside the lab environment.

---

## Administrative Account Separation

Two separate account types were implemented:

- A standard account for normal daily activities.
- A privileged account for administrative tasks.

The privileged account was added to the Domain Admins group, while the
standard account remained without domain administrative privileges.

This follows the principle of least privilege and reduces the risk of
performing daily activities with unnecessary administrative access.

---

## Validation

The following PowerShell commands were used to validate the environment:

```powershell
Get-ADDomain
Get-ADForest
Get-ADDomainController
Get-ADOrganizationalUnit -Filter *
Get-ADGroupMember "Domain Admins"
```

Active Directory health was also tested with:

```powershell
dcdiag
dcdiag /test:Advertising
dcdiag /test:Services
dcdiag /test:DNS
```

The Connectivity, Advertising, Services and DNS tests completed
successfully.

---

## Problems Encountered

### Moving an Organizational Unit

An Organizational Unit was accidentally created under the wrong parent
OU. The move was initially blocked because the object was protected
against accidental deletion.

The protection was temporarily disabled, the OU was moved to the correct
location and the protection was enabled again.

### Password Complexity Rejection

A password was initially rejected even though it contained uppercase
and lowercase letters, numbers and special characters.

The password contained part of the account name. After using a password
without information related to the username, the account was created
successfully.

No passwords are stored in this repository.

---

## Lessons Learned

- Installing AD DS and promoting a server are separate operations.
- Active Directory depends heavily on DNS.
- Domain controllers require predictable IP addresses.
- Organizational Units and security groups have different purposes.
- Administrative accounts should not be used for daily activities.
- Configuration must be validated instead of assuming that installation
  means the service is working.

---

## Next Steps

- Document the network configuration.
- Document the DNS configuration.
- Deploy pfSense as the lab gateway.
- Configure DHCP.
- Deploy a Windows 11 client.
- Join the client to the domain.
- Create and test Group Policies.