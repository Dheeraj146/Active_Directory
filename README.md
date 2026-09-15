# Active Directory Lab

A hands-on **Microsoft Active Directory Domain Services (AD DS)** lab built to demonstrate the deployment, configuration, administration, troubleshooting, and security of a Windows domain environment.

This repository is intentionally structured as a **practical infrastructure and security project**, not as a collection of theory notes. Each major concept is connected to a configuration task, command, verification step, or troubleshooting scenario.

---

## Project Overview

This project builds a small enterprise-style Windows domain in a virtualized lab environment. The environment uses **Windows Server 2022** as the Domain Controller and a Windows client such as **Windows 10 or Windows 11** as a domain-joined workstation.

The lab covers the complete lifecycle of a basic Active Directory environment:

```text
Windows Server 2022
        |
        | Install AD DS + DNS
        v
Domain Controller
        |
        +-----------------------------+
        |                             |
        v                             v
Active Directory                    DNS
        |                             |
        +-------------+---------------+
                      |
                      v
              Windows 10 / 11 Client
                      |
                      +-- Domain Join
                      +-- User Login
                      +-- GPO Processing
                      +-- Policy Verification
```

The objective is to understand **how the components work together**, rather than simply learning the definition of each component.

---

# Lab Objectives

The project demonstrates practical experience with:

- Installing and configuring Windows Server 2022.
- Configuring a Windows Server machine for Active Directory.
- Installing the Active Directory Domain Services role.
- Promoting Windows Server to a Domain Controller.
- Installing and configuring DNS as part of the AD environment.
- Creating an Active Directory domain.
- Managing users and computers.
- Creating Organizational Units (OUs).
- Creating and managing security groups.
- Moving users into appropriate OUs.
- Joining Windows client systems to the domain.
- Logging into Windows using domain accounts.
- Creating and linking Group Policy Objects.
- Configuring User Configuration and Computer Configuration policies.
- Understanding GPO inheritance and precedence.
- Working with Block Inheritance.
- Working with Enforced GPOs.
- Configuring GPO Security Filtering.
- Testing policy behavior from a domain-joined client.
- Using `gpupdate` to refresh policies.
- Using `gpresult` to identify applied policies.
- Using Windows administrative tools for troubleshooting.
- Understanding WMI/WMIC and modern PowerShell alternatives.
- Connecting Active Directory administration with SOC monitoring and Windows security.

---

# 1. Lab Requirements

## Recommended Virtual Machines

| System | Recommended OS | Purpose |
|---|---|---|
| VM 01 | Windows Server 2022 | Domain Controller, AD DS, DNS |
| VM 02 | Windows 10 Pro | Domain-joined client / testing workstation |
| VM 03 | Windows 11 Pro | Optional second domain-joined client |

A single Windows client is sufficient for the core lab. A second client is useful when testing different GPO scopes, security filtering, user groups, and computer policies.

## Recommended Lab Resources

For a comfortable virtualized lab, allocate approximately:

### Domain Controller

- 2–4 virtual CPU cores
- 4–8 GB RAM
- 60+ GB virtual disk
- 1 virtual network adapter

### Windows Client

- 2–4 virtual CPU cores
- 4–8 GB RAM
- 50+ GB virtual disk
- 1 virtual network adapter

The lab can be adjusted according to the host machine's available resources.

---

# 2. Operating System Downloads

Always obtain installation media from Microsoft or an appropriately licensed organization source. Do not commit ISO files to this repository.

## Windows Server 2022

Microsoft provides a Windows Server 2022 evaluation through the Microsoft Evaluation Center. The evaluation download provides the 64-bit ISO and supports Standard/Datacenter evaluation scenarios. Microsoft currently documents a 180-day evaluation period. citeturn0search0turn0search1

**Official download:**

- [Windows Server 2022 Evaluation Center](https://www.microsoft.com/en-in/evalcenter/evaluate-windows-server-2022)

For this lab, select:

- 64-bit ISO
- Server with Desktop Experience
- Standard Evaluation or Datacenter Evaluation

Server with Desktop Experience is recommended for this lab because the graphical administration tools make it easier to understand AD DS, DNS, Group Policy, Server Manager, and the Windows administrative ecosystem.

## Windows 10

Microsoft provides Windows 10 ISO media through its official software download page. Windows 10 reached end of support on **14 October 2025**, so it should now primarily be treated as a legacy/testing client in a controlled lab. citeturn0search4

**Official download:**

- [Windows 10 ISO / Software Download](https://www.microsoft.com/software-download/windows10)

Windows 10 is still useful in this project because it provides a realistic Windows client for practicing domain joining, Group Policy processing, authentication, and legacy Windows administration scenarios.

## Windows 11

Microsoft provides Windows 11 installation media and ISO options through its official software download page. The page supports installation media creation and ISO downloads for x64 systems. citeturn1search12turn1search2

**Official download:**

- [Windows 11 Software Download](https://www.microsoft.com/software-download/windows11)

Windows 11 is the preferred modern Windows client for extending this lab into current endpoint security and SOC monitoring scenarios.

> **Licensing note:** The repository provides links to Microsoft's official download pages; it does not redistribute Windows installation media or product keys.

---

# 3. Virtual Network Design

The virtual machines should be placed on a network where the Windows client can communicate directly with the Domain Controller.

A simple isolated lab can use a host-only/internal network, while an environment requiring Internet access can use a carefully configured NAT or routed network.

Example:

```text
                 Host Machine
                     |
              Virtual Network
                     |
          +----------+----------+
          |                     |
          v                     v
  Windows Server 2022       Windows 10/11
     Domain Controller          Client
          |                     |
          +---------+-----------+
                    |
               AD Authentication
               DNS Resolution
               Group Policy
```

### Critical DNS rule

The domain client should normally use the **Domain Controller's DNS service** for Active Directory name resolution. Pointing the client only at an unrelated public DNS server can break domain discovery and other AD functionality.

Before attempting the domain join, verify connectivity and DNS resolution.

Example commands on the client:

```cmd
ipconfig /all
ping <domain-controller-ip>
nslookup <domain-name>
```

---

# 4. Windows Server 2022 Installation

Create a virtual machine and attach the Windows Server 2022 ISO.

During installation:

1. Boot from the ISO.
2. Select the appropriate language, time, and keyboard options.
3. Select the Windows Server edition.
4. Choose **Server with Desktop Experience**.
5. Accept the license terms.
6. Select the installation disk.
7. Complete the installation.
8. Set a strong local Administrator password.
9. Log into the server.

After installation, do not immediately promote the machine to a Domain Controller. First perform basic server configuration.

---

# 5. Initial Windows Server Configuration

## 5.1 Rename the Server

Use a meaningful hostname such as:

```text
DC01
```

A clear naming convention becomes increasingly important when multiple servers are introduced.

Example PowerShell:

```powershell
Rename-Computer -NewName "DC01" -Restart
```

Verify the hostname:

```powershell
hostname
```

## 5.2 Configure a Static IP Address

A Domain Controller should have a stable IP address so that clients can reliably locate its services.

Example lab configuration:

```text
DC01
IP Address:      192.168.56.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.56.1
DNS Server:      192.168.56.10
```

The exact addressing depends on the virtual network used in the lab.

Verify the configuration:

```cmd
ipconfig /all
```

## 5.3 Install Windows Updates

Before configuring the server as a Domain Controller, install appropriate updates available for the lab environment.

This reduces the risk of building the rest of the lab on an unnecessarily outdated operating-system installation.

---

# 6. Install Active Directory Domain Services

Active Directory Domain Services can be installed through Server Manager.

### GUI workflow

```text
Server Manager
   |
   +-- Manage
       |
       +-- Add Roles and Features
           |
           +-- Role-based or feature-based installation
               |
               +-- Select server
                   |
                   +-- Active Directory Domain Services
```

Select AD DS and allow Server Manager to install the required management tools.

PowerShell can also be used:

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

After installation, the server has the AD DS role installed but is not yet a Domain Controller.

---

# 7. Promote the Server to a Domain Controller

After installing AD DS, Server Manager displays a notification indicating that configuration is required.

Select:

```text
Promote this server to a domain controller
```

For a new isolated lab, select:

```text
Add a new forest
```

Example lab domain:

```text
cloud.com
```

The actual domain name can be changed to a domain reserved for the lab, such as:

```text
lab.local
adlab.test
corp.example
```

A lab should avoid using a real production domain name.

During promotion, configure:

- Forest name.
- Domain name.
- Domain functional level as appropriate.
- DNS Server.
- Global Catalog.
- Directory Services Restore Mode password.
- Database location.
- Log location.
- SYSVOL location.

The server will restart after successful promotion.

---

# 8. Verify Domain Controller Installation

After reboot, verify that the server is operating as a Domain Controller.

Open Server Manager and verify that AD DS and DNS are available.

The administrative tools should include items such as:

- Active Directory Users and Computers.
- Active Directory Administrative Center.
- Active Directory Domains and Trusts.
- Active Directory Sites and Services.
- Group Policy Management.
- DNS Manager.
- Event Viewer.
- Windows PowerShell.

The lab screenshots demonstrate this administrative environment, including Server Manager and the **Tools** menu containing Active Directory and Group Policy management utilities.

---

# 9. Active Directory Users and Computers

Open:

```text
Server Manager
  -> Tools
      -> Active Directory Users and Computers
```

The console provides a hierarchical view of the domain.

A typical domain contains default containers and OUs such as:

```text
cloud.com
|
+-- Builtin
+-- Computers
+-- Domain Controllers
+-- ForeignSecurityPrincipals
+-- Managed Service Accounts
+-- Users
+-- SALES OU
+-- TECH OU
+-- Test OU
```

The screenshots from this lab show a domain containing **SALES OU**, **TECH OU**, and **Test OU**, demonstrating practical OU creation and directory organization.

---

# 10. Create Organizational Units

Right-click the domain:

```text
New -> Organizational Unit
```

Example OUs used in the lab:

```text
SALES OU
TECH OU
Test OU
```

OUs should represent a meaningful administrative boundary. They are particularly useful for applying Group Policy to a defined population of users or computers.

Example structure:

```text
cloud.com
|
+-- SALES OU
|
+-- TECH OU
|
+-- Test OU
```

---

# 11. Create Domain Users

Inside an OU:

```text
Right-click OU
   -> New
      -> User
```

Provide the user's:

- First name.
- Last name.
- User logon name.
- Password.
- Account options.

Example:

```text
OU: SALES OU
User: SalesUser01
```

The account can then be used to test domain authentication and Group Policy behavior.

---

# 12. Domain Groups

Groups allow permissions and policies to be managed at scale.

Example:

```text
SALES OU
   |
   +-- Sales Users
        |
        +-- User01
        +-- User02
        +-- User03
```

Instead of assigning permissions individually, administrators can assign permissions to the group.

This becomes particularly important when implementing least privilege and centralized access control.

---

# 13. Join Windows 10/11 to the Domain

Before joining the client:

1. Configure the client's IP settings.
2. Set the preferred DNS server to the Domain Controller.
3. Confirm the client can communicate with the Domain Controller.
4. Confirm the domain can be resolved.

Useful commands:

```cmd
ipconfig /all
ping <domain-controller-ip>
nslookup <domain-name>
```

### GUI domain join

Open:

```text
System Properties
   -> Computer Name
      -> Change
```

Select:

```text
Domain
```

Enter the Active Directory domain name.

Provide domain credentials when prompted and restart the client.

After reboot, select the appropriate domain account and authenticate using domain credentials.

---

# 14. Verify the Domain Join

On the client, verify the system's domain membership.

Useful commands:

```cmd
systeminfo
```

or:

```cmd
whoami
```

The logged-in identity should reflect the domain account, for example:

```text
CLOUD\SalesUser01
```

The exact domain and account name depend on the lab configuration.

---

# 15. Group Policy Management

Open:

```text
Server Manager
  -> Tools
      -> Group Policy Management
```

The Group Policy Management Console allows administrators to:

- Create GPOs.
- Edit GPOs.
- Link GPOs.
- Configure policy settings.
- Inspect inheritance.
- Configure enforcement.
- Configure security filtering.
- Model policy processing.
- Generate policy reports.

---

# 16. Practical GPO Exercise — Restrict Control Panel

A practical exercise performed in this lab is configuring a Group Policy to restrict Control Panel access.

### Objective

Create a policy that changes the Windows client behavior for selected users or computers.

### Workflow

```text
Create GPO
   |
   v
Configure setting
   |
   v
Link GPO to OU
   |
   v
Apply security filtering if required
   |
   v
Refresh client policy
   |
   v
Verify behavior
```

This practical exercise demonstrates the complete policy lifecycle rather than only explaining what a GPO is.

---

# 17. GPO Precedence — LSDOU

Group Policy processing is commonly described using:

```text
L = Local
S = Site
D = Domain
OU = Organizational Unit
```

Therefore:

```text
Local -> Site -> Domain -> OU
```

When the same setting is configured by multiple applicable GPOs, the effective result depends on processing order, inheritance, enforcement, security filtering, and the specific policy configuration.

### Practical troubleshooting rule

When a GPO does not behave as expected, do not immediately recreate the GPO. First determine:

1. Where the GPO is linked.
2. Whether the user/computer is in the expected OU.
3. Whether the GPO is enabled.
4. Whether Security Filtering permits application.
5. Whether inheritance is blocked.
6. Whether another GPO has higher precedence.
7. Whether the client has refreshed policy.
8. What `gpresult` reports.

---

# 18. Block Inheritance Practical

Block Inheritance can be configured on an OU to prevent normal inheritance from higher levels.

Example:

```text
Domain
 |
 +-- Domain GPO
 |
 +-- SALES OU
      |
      +-- Block Inheritance
```

The practical test should involve:

1. Creating a domain-level policy.
2. Confirming it applies to a test client.
3. Enabling Block Inheritance on the target OU.
4. Refreshing policy.
5. Checking the effective result.
6. Comparing the behavior with an Enforced policy.

This produces a much stronger understanding of inheritance than memorizing a definition.

---

# 19. Enforced GPO Practical

An Enforced GPO is useful when a higher-level policy must continue to apply even when lower-level inheritance behavior would otherwise prevent it.

Example test:

```text
Domain
 |
 +-- Security Policy [Enforced]
 |
 +-- SALES OU [Block Inheritance]
```

Compare:

```text
Test A: Normal GPO + Block Inheritance
Test B: Enforced GPO + Block Inheritance
```

Use `gpresult` on the client to determine which policies were applied.

---

# 20. Security Filtering Practical

Security Filtering can be used to restrict which users or computers can apply a GPO.

Example:

```text
GPO
 |
 +-- Link: SALES OU
 |
 +-- Security Filter: Sales Users
```

Practical test:

1. Create two test users.
2. Place both in the same OU.
3. Create a GPO.
4. Configure a visible test setting.
5. Apply Security Filtering to one user/group.
6. Log in as both users.
7. Run `gpupdate /force`.
8. Compare the resulting configuration.
9. Use `gpresult /r` to confirm policy processing.

This demonstrates that OU membership and security filtering solve different scope problems.

---

# 21. Force Group Policy Update

Use:

```cmd
gpupdate /force
```

A successful refresh does not automatically prove that the desired GPO was applied. Verification is still required.

---

# 22. Verify Effective Policy with gpresult

Use:

```cmd
gpresult /r
```

For a detailed HTML report:

```cmd
gpresult /h C:\Temp\gpresult.html
```

The report can help determine:

- Which user policies were applied.
- Which computer policies were applied.
- Which GPOs were denied.
- Security filtering effects.
- Group Policy processing details.

A useful troubleshooting workflow is:

```text
GPO configuration
      |
      v
Link / Scope
      |
      v
gpupdate /force
      |
      v
gpresult /r
      |
      v
Observe effective policy
      |
      v
Troubleshoot discrepancies
```

---

# 23. Windows Administrative Tools Used

The lab environment exposes a number of useful administrative tools from **Server Manager -> Tools**, including:

- Active Directory Administrative Center.
- Active Directory Domains and Trusts.
- Active Directory Module for Windows PowerShell.
- Active Directory Sites and Services.
- Active Directory Users and Computers.
- ADSI Edit.
- Computer Management.
- DNS.
- Event Viewer.
- Group Policy Management.
- Local Security Policy.
- Performance Monitor.
- Services.
- System Configuration.
- System Information.
- Task Scheduler.
- Windows Defender Firewall with Advanced Security.
- Windows PowerShell.
- Windows Server Backup.

The provided Server Manager screenshot demonstrates this administration-toolset view.

---

# 24. WMI / WMIC

Windows Management Instrumentation provides a management interface for Windows system information and administrative operations.

WMIC was a command-line interface to WMI and is now deprecated on modern Windows versions. Existing administrative and security workflows may still contain WMIC commands, so understanding the technology remains useful.

Modern Windows administration should generally prefer PowerShell and CIM/WMI cmdlets where appropriate.

Examples of useful PowerShell approaches include:

```powershell
Get-CimInstance Win32_OperatingSystem
Get-CimInstance Win32_ComputerSystem
Get-CimInstance Win32_Process
```

---

# 25. Active Directory Security Perspective

Active Directory is a critical security boundary in enterprise Windows environments.

An attacker who compromises privileged AD identities can potentially obtain broad access to domain resources. Therefore, administrators and SOC analysts must understand how normal identity operations appear in logs and how abnormal activity can be detected.

Important security areas include:

- Privileged account management.
- Least privilege.
- Security group membership.
- Domain Controller protection.
- Authentication monitoring.
- Account lifecycle management.
- GPO change monitoring.
- PowerShell logging.
- Windows security event logging.
- Lateral movement detection.
- Credential abuse detection.

---

# 26. SOC Relevance

The Active Directory lab provides the infrastructure knowledge required to interpret Windows security telemetry.

A SOC analyst may investigate events involving:

- Successful logons.
- Failed logons.
- Account creation.
- Account deletion.
- Password changes.
- Group membership changes.
- Privileged group modifications.
- New computer accounts.
- GPO changes.
- Administrative logons.
- Suspicious PowerShell execution.
- Lateral movement.
- Abnormal authentication patterns.

For example, an alert indicating a user was added to a privileged group is much easier to investigate when the analyst understands how Active Directory groups and administrative roles are actually configured.

---

# 27. Troubleshooting Playbook

## Client cannot join domain

Check:

```cmd
ipconfig /all
nslookup <domain-name>
ping <domain-controller-ip>
```

Then verify:

- Client DNS points to the Domain Controller.
- Domain Controller is reachable.
- AD DS and DNS services are running.
- Time synchronization is reasonable.
- The domain name is correct.

## GPO is not applying

Check:

```cmd
gpupdate /force
gpresult /r
```

Then investigate:

- OU placement.
- GPO link.
- GPO status.
- Security Filtering.
- Block Inheritance.
- Enforced policies.
- GPO precedence.
- User vs computer configuration.

## User cannot authenticate

Check:

- Account name.
- Password.
- Account status.
- Domain membership.
- DNS.
- Domain Controller connectivity.
- Time synchronization.

---

# 28. Evidence and Screenshots

Screenshots are treated as **lab evidence**, not decoration. Each screenshot should demonstrate a configuration or verification result.

Recommended evidence includes:

| Evidence | Purpose |
|---|---|
| Server Manager Dashboard | Shows installed server roles and management state |
| Server Manager Tools | Shows available AD/DNS/GPO administration tools |
| AD Users and Computers | Shows domain structure and OUs |
| Domain Controller configuration | Demonstrates server promotion/configuration |
| DNS Manager | Demonstrates AD-integrated DNS configuration |
| GPO Management Console | Demonstrates GPO creation and linking |
| GPO Editor | Demonstrates the actual policy configuration |
| Block Inheritance | Demonstrates inheritance control |
| Enforced GPO | Demonstrates policy enforcement |
| Security Filtering | Demonstrates targeted GPO scope |
| Windows client domain membership | Demonstrates successful domain join |
| `gpupdate /force` | Demonstrates policy refresh |
| `gpresult /r` | Demonstrates effective policy |
| Restricted client setting | Demonstrates the actual policy result |

The repository contains a dedicated screenshot documentation plan in [`screenshots/README.md`](screenshots/README.md).

---

# 29. Practical Lab Checklist

Use this checklist as the execution sequence for rebuilding the environment:

- [ ] Download Windows Server 2022 ISO from Microsoft.
- [ ] Download Windows 10 and/or Windows 11 ISO from Microsoft.
- [ ] Create Server VM.
- [ ] Install Windows Server 2022 Desktop Experience.
- [ ] Rename server to `DC01`.
- [ ] Configure static IP.
- [ ] Configure DNS.
- [ ] Install AD DS.
- [ ] Promote server to Domain Controller.
- [ ] Create a new forest/domain.
- [ ] Verify AD DS.
- [ ] Verify DNS.
- [ ] Open Active Directory Users and Computers.
- [ ] Create SALES OU.
- [ ] Create TECH OU.
- [ ] Create Test OU.
- [ ] Create test users.
- [ ] Create security groups.
- [ ] Create Windows client VM.
- [ ] Configure client DNS to the Domain Controller.
- [ ] Join client to the domain.
- [ ] Log in using a domain account.
- [ ] Create a test GPO.
- [ ] Link the GPO to an OU.
- [ ] Configure a practical policy.
- [ ] Run `gpupdate /force`.
- [ ] Run `gpresult /r`.
- [ ] Generate an HTML `gpresult` report.
- [ ] Test GPO precedence.
- [ ] Test Block Inheritance.
- [ ] Test Enforced GPO behavior.
- [ ] Test Security Filtering.
- [ ] Capture evidence screenshots.
- [ ] Document troubleshooting observations.

---

# 30. Repository Structure

```text
Active_Directory/
|
+-- README.md
|
+-- docs/
|   +-- WINDOWS_SERVER_SETUP.md
|   +-- PRACTICAL_LABS.md
|   +-- TROUBLESHOOTING.md
|
+-- screenshots/
|   +-- README.md
|   +-- 01-server-manager-dashboard.png
|   +-- 02-active-directory-users-and-computers.png
|   +-- 03-server-manager-tools.png
|   +-- ...
|
+-- scripts/
    +-- powershell/
    +-- cmd/
```

Do **not** commit:

- Windows ISO files.
- Product keys.
- Passwords.
- NTLM hashes.
- Private keys.
- Authentication tokens.
- Personal information.
- Sensitive network credentials.

---

# 31. Future Expansion

The lab can be expanded into a full Windows enterprise security lab by adding:

- Multiple Domain Controllers.
- Windows Event Forwarding.
- Sysmon.
- Advanced Windows auditing.
- PowerShell logging.
- Microsoft Defender configuration.
- Kerberos authentication analysis.
- NTLM authentication analysis.
- LDAP security.
- Privileged Access Management concepts.
- Active Directory attack-path analysis.
- Wazuh integration.
- SIEM ingestion of Windows Security logs.
- Authentication attack detection.
- Privilege escalation detection.
- Lateral movement detection.
- GPO change monitoring.
- Active Directory incident-response scenarios.

The long-term goal is to evolve the environment from a basic Windows administration lab into a **security-focused enterprise Active Directory detection and response laboratory**.

---

# Conclusion

This project demonstrates practical Active Directory administration from initial Windows Server deployment through domain creation, organizational structure, domain joining, Group Policy implementation, policy troubleshooting, and security analysis.

The lab is intentionally evidence-driven: every major configuration should be validated through the relevant Windows console, command, client behavior, or screenshot.

That approach makes the repository useful not only as documentation of an Active Directory environment, but also as a practical portfolio demonstrating Windows administration, identity and access management, Group Policy, troubleshooting, and security operations fundamentals.
