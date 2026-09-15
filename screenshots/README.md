# Active Directory Lab Screenshots

Screenshots in this directory are intended to provide visual evidence of the practical work performed in the lab.

## Current Evidence

The following screenshots have been identified from the lab work and should be stored with the corresponding files in this directory:

### 01 — Server Manager Dashboard

Shows the Windows Server management environment and installed roles, including:

- AD DS
- DHCP
- DNS
- File and Storage Services

### 02 — Active Directory Users and Computers

Shows the domain structure and the OUs created during the lab, including:

- SALES OU
- TECH OU
- Test OU

### 03 — Server Manager Tools

Shows the Windows Server administrative toolset, including:

- Active Directory Users and Computers
- Active Directory Administrative Center
- Active Directory Domains and Trusts
- Active Directory Sites and Services
- DNS
- Event Viewer
- Group Policy Management
- Windows PowerShell
- Other Windows administration tools

## Additional Screenshots Recommended

To make the project fully evidence-driven, capture the following as the lab is rebuilt or extended:

- Windows Server installation screen.
- Server hostname configuration.
- Static IP configuration.
- AD DS role installation.
- AD DS promotion wizard.
- Successful Domain Controller promotion.
- DNS Manager.
- Active Directory Users and Computers with OUs.
- User creation.
- Group creation.
- Windows client network configuration.
- Windows client domain membership.
- Successful domain login.
- Group Policy Management Console.
- GPO creation.
- GPO link to an OU.
- GPO Editor showing the configured setting.
- Security Filtering configuration.
- Block Inheritance configuration.
- Enforced GPO configuration.
- `gpupdate /force` output.
- `gpresult /r` output.
- HTML `gpresult` report.
- Final client-side policy result.

## Screenshot Naming Convention

Use numbered names so that the evidence follows the practical workflow:

```text
01-server-manager-dashboard.png
02-active-directory-users-and-computers.png
03-server-manager-tools.png
04-server-static-ip.png
05-add-ad-ds-role.png
06-domain-controller-promotion.png
07-dns-manager.png
08-organizational-units.png
09-domain-users.png
10-domain-joined-client.png
11-group-policy-management.png
12-gpo-editor.png
13-security-filtering.png
14-block-inheritance.png
15-enforced-gpo.png
16-gpupdate.png
17-gpresult.png
18-final-policy-result.png
```

## Security Rules

Before committing screenshots, remove or obscure:

- Passwords.
- Product keys.
- Authentication tokens.
- Private IP information if it is not intended to be public.
- Personal usernames or personal information where appropriate.
- Secrets displayed in command output.

Screenshots should primarily demonstrate configuration and verification, not expose sensitive information.
