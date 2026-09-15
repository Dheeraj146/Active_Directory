# Windows Server and Windows Client Setup

This guide provides the practical build procedure for the Active Directory lab.

## 1. Build the Domain Controller

### VM configuration

Recommended starting point:

- 2–4 vCPUs
- 4–8 GB RAM
- 60+ GB disk
- One virtual NIC
- Windows Server 2022 Desktop Experience

### Installation

1. Create the VM.
2. Mount the Windows Server 2022 ISO.
3. Boot from the ISO.
4. Select language, time, and keyboard settings.
5. Select Server with Desktop Experience.
6. Install to the virtual disk.
7. Set the local Administrator password.
8. Log in.

## 2. Rename the Server

Example:

```powershell
Rename-Computer -NewName "DC01" -Restart
```

After restart:

```cmd
hostname
```

## 3. Configure a Static IP

Example lab network:

```text
IP:      192.168.56.10
Mask:    255.255.255.0
Gateway: 192.168.56.1
DNS:     192.168.56.10
```

Use values appropriate to the virtualization network actually configured on the host.

Verify:

```cmd
ipconfig /all
```

## 4. Install AD DS

### Server Manager

```text
Server Manager
 -> Manage
 -> Add Roles and Features
 -> Role-based or feature-based installation
 -> Select DC01
 -> Active Directory Domain Services
 -> Install
```

### PowerShell

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

## 5. Create the Forest

From Server Manager:

```text
Notifications
 -> Promote this server to a domain controller
 -> Add a new forest
```

Example lab domain:

```text
cloud.com
```

Use a dedicated lab namespace rather than a production namespace.

Configure the DSRM password and complete the prerequisite checks. Allow the server to reboot.

## 6. Verify AD DS and DNS

After reboot, check Server Manager for AD DS and DNS.

Open:

```text
Tools
 -> Active Directory Users and Computers
Tools
 -> DNS
Tools
 -> Group Policy Management
```

Useful PowerShell verification:

```powershell
Get-WindowsFeature AD-Domain-Services
Get-Service DNS
```

## 7. Create OUs

Open Active Directory Users and Computers.

Create:

```text
SALES OU
TECH OU
Test OU
```

These OUs can later be used for GPO experiments.

## 8. Create Test Users

Inside an OU:

```text
New -> User
```

Create at least two test accounts so that policy filtering can be demonstrated.

Example:

```text
SalesUser01
SalesUser02
```

## 9. Build the Windows Client

Create a second VM using Windows 10 or Windows 11.

Recommended:

- 2–4 vCPUs
- 4–8 GB RAM
- 50+ GB disk
- One virtual NIC

Install Windows normally and create a temporary local administrator account.

## 10. Configure Client DNS

Before attempting to join the domain, configure the client's preferred DNS server to the Domain Controller.

Example:

```text
Client IP:       192.168.56.20
DNS Server:      192.168.56.10
```

Verify:

```cmd
ipconfig /all
nslookup cloud.com
ping 192.168.56.10
```

## 11. Join the Client to the Domain

Open System Properties and select the option to change the computer name/domain membership.

Enter:

```text
cloud.com
```

Provide authorized domain credentials when prompted.

Restart the client.

## 12. Verify Domain Authentication

Log in using the test domain account.

Run:

```cmd
whoami
```

Expected format:

```text
CLOUD\SalesUser01
```

Run:

```cmd
systeminfo
```

and verify the system reports the expected domain information.

## 13. First GPO Test

On the Domain Controller:

```text
Server Manager
 -> Tools
 -> Group Policy Management
```

Create a test GPO and link it to the Test OU.

Configure an easily observable setting, then on the client run:

```cmd
gpupdate /force
gpresult /r
```

Confirm that the setting is effective.

## 14. Evidence Collection

Capture screenshots after each major milestone:

1. Server Manager showing installed roles.
2. AD Users and Computers showing the domain and OUs.
3. DNS Manager.
4. Group Policy Management.
5. Domain-joined client.
6. GPO editor showing the configured setting.
7. `gpupdate /force` result.
8. `gpresult /r` result.
9. Final client behavior.

Screenshots should show the relevant configuration clearly while avoiding passwords, product keys, or sensitive personal information.
