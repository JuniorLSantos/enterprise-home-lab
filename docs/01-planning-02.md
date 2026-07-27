# Enterprise Home Lab Planning

## Objective

Build an enterprise-style virtual infrastructure to simulate a small corporate environment.

The project will be used to practice Windows Server administration, Active Directory, networking, Linux administration and cybersecurity while documenting every implementation step.

## **Technologies**

Windows Server 2022
↓
Active Directory
↓
Windows 11
↓
Ubuntu
↓
Kali Linux
↓
pfSense
↓
Splunk

---

## **Host Machine**

- Operating System: Windows 11
- Hypervisor: Oracle VirtualBox

---

## **VMs**

**VM*               **Função**            **CPU**            **Ram**            **DISK**
DC01                AD,DNS,DHCP              2                 4 GB               60 GB
CLIENT01            Cliente                  2                 4 GB               60 GB
LINUX01             Docker                   2                 2 GB               30 GB
FW01                Firewall                 2                 2 GB               20 GB
KALI01              Pentest                  2                 2 GB               40 GB
SPLUNK01            Logs                     2                 4 GB               80 GB

## Resource Planning

The CPU, memory and storage values were selected to provide a balance between performance and efficient use of the host computer's resources.

The environment is intended for study purposes and simulates a small business infrastructure.
---

## **Redes**

Network Address:
>192.168.10.0/24

Subnet Mask:
>255.255.255.0

Gateway:
>192.168.10.1

DC01: //(Servidor)
>192.168.10.10

CLIENT01: //(Windows 11)
>192.168.10.20

LINUX01: //(Ubuntu)
>192.168.10.30

FW01: //(pfSense)
>192.168.10.1

KALI01: //(Kali)
>192.168.10.40

SPLUNK01: //(Splunk)
>192.168.10.50

---

## Planned Services

- Active Directory
- DNS
- DHCP
- File Server
- Group Policy
- Docker
- VPN
- Splunk

---

## Lessons Learned

- Learned how to design the initial infrastructure before deployment.
- Defined the virtual machines required for the lab.
- Planned the network addressing scheme.

---