# Windows Server 2022 Installation

## Overview

This document describes the deployment of the Windows Server 2022
virtual machine used as the first domain controller of the enterprise
home lab.

The server was installed manually in Oracle VirtualBox, updated and
validated before Active Directory Domain Services was deployed.

---

## Virtual Machine Configuration

| Setting | Configuration |
|---|---|
| Virtual machine name | DC01 |
| Hypervisor | Oracle VirtualBox |
| Operating system | Windows Server 2022 Standard Evaluation |
| Installation type | Desktop Experience |
| Architecture | 64-bit |
| Virtual CPUs | 4 |
| Memory | 8 GB |
| Virtual disk | 80 GB |
| Disk format | VDI |
| Storage allocation | Dynamically allocated |
| Initial network mode | NAT |
| Current network mode | Internal Network |
| Internal network name | LAB-LAN |

The dynamically allocated disk uses physical storage as data is written
instead of reserving the entire 80 GB immediately.

---

## Operating System Edition

The following edition was selected during installation:

```text
Windows Server 2022 Standard Evaluation (Desktop Experience)
```

Desktop Experience was selected because it provides the graphical
interface and management tools required during the initial learning
stages of the project.

The Server Core edition consumes fewer resources, but requires most
administrative operations to be performed through PowerShell or remote
management tools.

---

## Installation Process

The Windows Server 2022 ISO image was attached to the virtual optical
drive in VirtualBox.

The installation process included:

1. Booting the virtual machine from the ISO.
2. Selecting Windows Server 2022 Standard Evaluation.
3. Selecting the Desktop Experience edition.
4. Accepting the evaluation license terms.
5. Performing a custom installation.
6. Selecting the virtual disk as the installation destination.
7. Waiting for the operating system files to be installed.
8. Configuring the built-in Administrator account.
9. Signing in to Windows Server for the first time.

The Administrator password was stored securely outside the repository.
No credentials are included in the project documentation.

---

## Initial Server Configuration

After installation, the server was prepared for its future role as a
domain controller.

The initial configuration included:

- Confirming that Windows Server started correctly.
- Verifying the virtual hardware allocation.
- Renaming the server to `DC01`.
- Installing available Windows updates.
- Restarting the server when required.
- Confirming that Server Manager loaded successfully.
- Creating recovery snapshots.
- Configuring the final VirtualBox network mode.

The hostname was verified with:

```powershell
hostname
```

Expected result:

```text
DC01
```

---

## Windows Update Troubleshooting

### Symptoms

During the first update attempts, the virtual machine became very slow.

The following symptoms were observed:

- Windows Update remained at `0%` for an extended period.
- Windows Modules Installer Worker consumed significant CPU resources.
- The operating system became temporarily unresponsive.
- Shutdown remained on the Update Orchestrator Service screen.
- The virtual machine required additional troubleshooting before the
  updates could complete.

### Investigation

Task Manager was used to identify the processes consuming resources.

Windows Modules Installer Worker was using CPU while Windows processed
system components and updates.

The following services are related to the Windows update process:

| Service | Purpose |
|---|---|
| Windows Update | Detects and installs Windows updates |
| BITS | Transfers update files in the background |
| Cryptographic Services | Validates update packages |
| Windows Modules Installer | Installs and modifies Windows components |
| Update Orchestrator | Coordinates update installation and restarts |

### Resolution

A snapshot was created before repairing the Windows Update components.

After troubleshooting the update services and restarting the virtual
machine, Windows Update was executed again.

The updates progressed normally and completed successfully.

The update process was allowed to finish before the server was used for
the Active Directory deployment.

### Validation

The most recently installed updates can be reviewed with:

```powershell
Get-HotFix |
    Sort-Object InstalledOn -Descending |
    Select-Object -First 10
```

The status of the main update services can be checked with:

```powershell
Get-Service wuauserv,bits,cryptsvc,TrustedInstaller
```

---

## Performance Adjustments

The original virtual hardware allocation was not sufficient for a
comfortable Windows Server graphical experience during updates and
administrative tasks.

The final allocation was adjusted to:

```text
4 virtual CPUs
8 GB of RAM
80 GB dynamically allocated storage
```

These resources provide a better balance between server performance and
the resources available on the physical host.

Windows Update may still temporarily consume significant CPU and disk
resources. High resource usage during component installation does not
automatically mean the virtual machine has crashed.

---

## Snapshots

Snapshots were created at important recovery points.

| Snapshot | Purpose |
|---|---|
| 00-Before-Windows-Update-Repair | Recovery point before update troubleshooting |
| 01-Clean-Install-Updated | Clean and fully updated Windows Server installation |
| 02-Static-IP-Configured | Server with static network configuration |
| 03-ADDS-DNS-Validated | Active Directory and DNS deployment validated |

Snapshots make it possible to return to a known working state if a
future configuration causes a failure.

Snapshots are not a replacement for a complete backup because they
depend on the original virtual machine files.

---

## Installation Validation

The following commands can be used to inspect the server:

```powershell
hostname
Get-ComputerInfo
Get-NetIPConfiguration
Get-Service wuauserv,bits,cryptsvc,TrustedInstaller
```

The deployment was considered successful after confirming that:

- Windows Server started without installation errors.
- The server hostname was `DC01`.
- Server Manager opened correctly.
- Windows updates completed successfully.
- The virtual machine remained stable after restarting.
- A clean recovery snapshot was available.
- The server was ready for static network configuration.

---

## Security Considerations

The following precautions were followed:

- Administrative passwords were not stored in the repository.
- Passwords were stored in a password manager.
- The server was updated before deploying Active Directory.
- Snapshots were created before major changes.
- The final lab network was isolated from the physical home network.
- Only evaluation software and legitimate installation media were used.

---

## Problems Encountered

| Problem | Cause or observation | Resolution |
|---|---|---|
| Slow virtual machine | Updates and Windows Modules Installer consumed resources | Virtual hardware was adjusted and updates were allowed to finish |
| Windows Update remained at 0% | Update components were processing or required repair | Windows Update components were repaired and the update process was restarted |
| Shutdown appeared frozen | Update Orchestrator was stopping services | The update process was completed before continuing |
| Poor graphical responsiveness | Initial VM resource allocation was limited | CPU and memory allocation were increased |

---

## Lessons Learned

- Windows Server updates can require significant CPU, disk and time.
- High CPU usage can indicate active maintenance rather than a crash.
- Virtual machine resources should be adjusted according to workload.
- Snapshots should be created before risky configuration changes.
- A snapshot is useful for recovery but is not a complete backup.
- The operating system should be updated before installing critical
  infrastructure roles.
- Problems and their validation steps should be documented instead of
  recording only successful results.

---

## Related Documentation

- [Network Configuration](04-network-configuration.md)
- [Active Directory Deployment](05-active-directory.md)
- [DNS Configuration and Validation](06-dns.md)