# Active Directory Lab

A hands-on Active Directory lab covering Windows Server, Active Directory Domain Services (AD DS), DNS, domain administration, Group Policy, Windows clients, troubleshooting, and security-focused administration.

This repository is organized as a collection of **practical topic guides**. The README serves as an index; detailed explanations, procedures, commands, screenshots, observations, and troubleshooting notes are maintained in separate documentation files.

## Lab Environment

| Component | Platform | Purpose |
|---|---|---|
| Domain Controller | Windows Server 2022 | AD DS, DNS, domain services and Group Policy |
| Client 01 | Windows 10 | Domain-joined Windows workstation |
| Client 02 | Windows 11 | Domain-joined Windows workstation / policy testing |
| Virtualization | Virtual machine environment | Isolated Active Directory lab |

## Documentation

### Windows Environment

- [Windows Server 2022 Setup](docs/windows-server-2022-setup.md) — VM creation, OS installation, hostname, networking, static IP, and server preparation.
- [Windows 10 & 11 Client Setup](docs/windows-client-setup.md) — client installation, networking, DNS configuration, domain joining, and verification.

### Active Directory Fundamentals

- [Active Directory Domain Services](docs/active-directory-domain-services.md) — AD DS architecture, directory objects, authentication, authorization, and core components.
- [Domain Controller](docs/domain-controller.md) — Domain Controller deployment, responsibilities, verification, and administration.
- [DNS and Active Directory](docs/dns-and-active-directory.md) — DNS configuration, AD-integrated DNS, service discovery, troubleshooting, and practical tests.
- [Organizational Units](docs/organizational-units.md) — OU design, creation, object placement, delegation, and GPO scope.
- [Users and Security Groups](docs/users-and-groups.md) — user accounts, security groups, group membership, and access-management practices.

### Group Policy

- [Group Policy Objects](docs/group-policy.md) — GPO creation, configuration, linking, processing, and verification.
- [GPO Precedence and LSDOU](docs/gpo-precedence.md) — policy processing order, precedence, conflicts, and troubleshooting.
- [Block Inheritance and Enforced GPOs](docs/gpo-inheritance.md) — inheritance behavior, Block Inheritance, Enforced policies, and practical testing.
- [GPO Security Filtering](docs/gpo-security-filtering.md) — security filtering, permissions, scope, and user/computer targeting.

### Practical Labs

- [Active Directory Practical Labs](docs/practical-labs.md) — step-by-step exercises that combine the individual topics into working scenarios.
- [GPO Troubleshooting](docs/gpo-troubleshooting.md) — a structured troubleshooting workflow using `gpupdate`, `gpresult`, policy scope, inheritance, filtering, and event logs.
- [Windows Administration Commands](docs/windows-administration-commands.md) — practical CMD, PowerShell, WMI/CIM, networking, and Group Policy commands.

### Security and SOC

- [Active Directory Security](docs/active-directory-security.md) — identity security, privilege management, common weaknesses, auditing, and defensive practices.
- [AD Security Monitoring and SOC Relevance](docs/ad-soc-monitoring.md) — Windows/AD events, authentication monitoring, suspicious activity, and SIEM/Wazuh integration opportunities.

### Evidence

- [Screenshots and Lab Evidence](screenshots/README.md) — screenshot index, evidence requirements, naming convention, and documentation standards.

## Official Operating System Downloads

Use Microsoft's official sources for installation media and licensing information. Evaluation editions are intended for lab/testing purposes.

- [Windows Server 2022 Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)
- [Windows 10 Download](https://www.microsoft.com/en-us/software-download/windows10)
- [Windows 11 Download](https://www.microsoft.com/en-us/software-download/windows11)

> **Lab note:** Windows 10 reached end of support on October 14, 2025. It may still be useful in an isolated lab for legacy-client testing, but Windows 11 should be preferred for a current client build where practical.

## Lab Documentation Method

Each topic is documented using the following approach:

```text
Concept
   ↓
Lab Objective
   ↓
Environment / Prerequisites
   ↓
Configuration Steps
   ↓
Commands / Tools
   ↓
Screenshot Evidence
   ↓
Expected Result
   ↓
Observed Result
   ↓
Troubleshooting
   ↓
Security Relevance
```

The goal is to maintain **reproducible practical evidence**, not just theoretical notes.

## Repository Structure

```text
Active_Directory/
│
├── README.md
│
├── docs/
│   ├── windows-server-2022-setup.md
│   ├── windows-client-setup.md
│   ├── active-directory-domain-services.md
│   ├── domain-controller.md
│   ├── dns-and-active-directory.md
│   ├── organizational-units.md
│   ├── users-and-groups.md
│   ├── group-policy.md
│   ├── gpo-precedence.md
│   ├── gpo-inheritance.md
│   ├── gpo-security-filtering.md
│   ├── practical-labs.md
│   ├── gpo-troubleshooting.md
│   ├── windows-administration-commands.md
│   ├── active-directory-security.md
│   └── ad-soc-monitoring.md
│
└── screenshots/
    └── README.md
```

## Project Direction

The lab is designed to evolve from a basic Windows domain environment into a security-focused Active Directory environment with auditing, Windows telemetry, Sysmon, event forwarding, SIEM integration, attack detection, and incident-response scenarios.
