# Domain Controller

## Overview

A Domain Controller (DC) is a Windows Server system running Active Directory Domain Services. It stores directory information and provides core domain services to domain members.

## Responsibilities

A Domain Controller participates in:

- User and computer authentication.
- Directory lookups.
- Kerberos-based domain authentication.
- LDAP directory services.
- Group Policy processing.
- Domain-related DNS services.
- Replication when multiple Domain Controllers exist.

## Lab Role

The lab uses a Windows Server 2022 system as the primary Domain Controller. The supplied Server Manager screenshot shows AD DS and DNS installed on the server.

## Practical Verification

Open:

```text
Server Manager → Tools
```

Verify access to Active Directory Users and Computers, Active Directory Administrative Center, Active Directory Domains and Trusts, Active Directory Sites and Services, DNS, and Group Policy Management.

Open Active Directory Users and Computers and confirm the domain and Domain Controllers container are available.

## Domain Controller Health

Use built-in Windows diagnostic and management tools to check service status, DNS, directory health, and event logs. Record failures and their corrective actions instead of simply restarting services without identifying the cause.

## Security Importance

The Domain Controller is one of the highest-value assets in a Windows enterprise. Administrative compromise can affect users, computers, groups, authentication, and policy. Defensive priorities include least privilege, privileged-account protection, patching, auditing, restricted administration, and monitoring.

## Future Expansion

The lab can later be extended with a second Domain Controller to demonstrate replication, redundancy, sites, and multi-DC troubleshooting.
