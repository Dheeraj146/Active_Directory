# Windows Server 2022 Setup

## Objective

Prepare a Windows Server 2022 virtual machine to become the Active Directory Domain Controller for the lab.

## Recommended VM Configuration

- CPU: 2–4 vCPUs
- RAM: 4–8 GB
- Storage: 60 GB or more
- Network: one virtual NIC connected to the lab network
- Installation: Windows Server 2022 Desktop Experience

Adjust resources according to the host machine and the number of VMs running simultaneously.

## Download

Use Microsoft's official evaluation media:

**Windows Server 2022 Evaluation:**
https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022

## Installation

1. Create a new VM in the virtualization platform.
2. Mount the Windows Server 2022 ISO.
3. Boot the VM from the ISO.
4. Select language, time, and keyboard options.
5. Select **Windows Server 2022 Standard/Datacenter with Desktop Experience** as required by the lab.
6. Select the virtual disk and complete installation.
7. Set a strong local Administrator password.
8. Log in and allow initial configuration to complete.

## Initial Server Configuration

### Rename the server

Use a meaningful hostname such as `DC01`.

PowerShell:

```powershell
Rename-Computer -NewName "DC01" -Restart
```

Verify:

```cmd
hostname
```

### Configure a static IP

A Domain Controller should have a stable address. Configure a static address appropriate for the virtual network.

Example:

```text
DC01
IP address:      192.168.56.10
Subnet mask:     255.255.255.0
Default gateway: 192.168.56.1
Preferred DNS:   192.168.56.10
```

Do not copy these addresses blindly; use the addressing scheme of the actual lab network.

Verify:

```cmd
ipconfig /all
```

### Configure the server DNS role later

For a single-DC lab, the Domain Controller will normally provide DNS for the AD domain. The exact DNS client configuration should be validated after DNS/AD DS installation.

## Install AD DS

### Server Manager

```text
Server Manager
→ Manage
→ Add Roles and Features
→ Role-based or feature-based installation
→ Select DC01
→ Active Directory Domain Services
→ Install
```

### PowerShell

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

## Promote the Server to a Domain Controller

After AD DS installation:

```text
Server Manager
→ Notifications
→ Promote this server to a domain controller
→ Add a new forest
```

For the existing lab, the domain shown in the supplied Active Directory screenshot is `cloud.com`.

Configure:

- Forest/domain name
- Functional levels as appropriate for the lab
- DNS installation
- Global Catalog
- Directory Services Restore Mode (DSRM) password
- Database, log, and SYSVOL paths

Run prerequisite checks and complete the promotion. The server will restart.

## Verify the Domain Controller

Open:

```text
Server Manager
→ Tools
→ Active Directory Users and Computers
```

Also verify:

```text
Tools
→ DNS
Tools
→ Group Policy Management
```

PowerShell checks:

```powershell
Get-WindowsFeature AD-Domain-Services
Get-Service DNS
```

Useful domain-controller diagnostics include:

```cmd
dcdiag
```

## Evidence to Capture

Capture screenshots of:

1. Server Manager dashboard.
2. Static IP configuration.
3. AD DS role installation.
4. Domain Controller promotion wizard.
5. Successful promotion.
6. DNS Manager.
7. Active Directory Users and Computers.
8. Group Policy Management.

## Troubleshooting

### Domain promotion fails

Check:

- Hostname and network configuration.
- Static IP configuration.
- DNS configuration.
- Time synchronization.
- Required administrative privileges.
- Server Manager prerequisite warnings.

### Client cannot locate the domain

Start with DNS. A domain client should normally use the AD DNS server rather than an unrelated public DNS resolver for domain discovery.

Useful tests:

```cmd
nslookup cloud.com
ipconfig /all
ping <DC-IP>
```

## Security Notes

- Use strong administrative credentials.
- Keep the lab isolated from production networks.
- Never commit passwords, recovery keys, or product keys to GitHub.
- Treat the Domain Controller as a high-value security asset.
