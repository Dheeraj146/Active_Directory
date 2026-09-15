# Active Directory Lab

A hands-on Microsoft Active Directory lab focused on building, administering, and troubleshooting a Windows domain environment. This repository documents the practical configuration and administration work performed in a controlled virtualized lab environment.

## Overview

This project demonstrates the deployment and administration of an Active Directory Domain Services (AD DS) environment using Windows Server as the Domain Controller and a Windows client joined to the domain. The lab focuses on understanding how centralized identity, authentication, authorization, organizational structure, and Group Policy are implemented in an enterprise Windows environment.

The objective is not simply to install Active Directory, but to understand how the individual components interact: DNS enables domain discovery, the Domain Controller provides centralized authentication and directory services, Organizational Units provide administrative structure, security groups simplify access management, and Group Policy provides centralized configuration and security enforcement.

## Lab Objectives

- Deploy a Windows Server-based Active Directory Domain Services environment.
- Configure a Windows Server as a Domain Controller.
- Create and manage an Active Directory domain.
- Understand the relationship between AD DS and DNS.
- Create Organizational Units (OUs) for administrative organization.
- Create and manage domain user accounts.
- Add users to appropriate OUs.
- Understand security groups and group membership.
- Join a Windows client machine to the domain.
- Configure and apply Group Policy Objects (GPOs).
- Understand GPO scope, inheritance, precedence, and processing.
- Practice Block Inheritance and Enforced GPO behavior.
- Configure and understand GPO Security Filtering.
- Verify policy application from the Windows client.
- Use administrative and troubleshooting commands such as `gpupdate`, `gpresult`, and WMIC/WMI-related tooling.
- Develop practical skills relevant to Windows administration, SOC operations, identity security, and enterprise security monitoring.

## Lab Architecture

The environment consists of a small virtualized Windows domain designed to reproduce the core components of an enterprise Active Directory environment.

```text
                         Active Directory Domain
                                  |
                         +----------------+
                         | Domain Controller|
                         |  Windows Server |
                         |     AD DS       |
                         |      DNS        |
                         +--------+-------+
                                  |
                         Domain Authentication
                         Group Policy Processing
                                  |
                         +--------+-------+
                         | Windows Client |
                         | Windows 10/11  |
                         +----------------+
```

The Domain Controller acts as the central authority for the domain. The Windows client communicates with the Domain Controller for domain authentication, directory lookups, and Group Policy processing.

## 1. Active Directory Domain Services (AD DS)

Active Directory Domain Services is Microsoft's directory service for managing identities, computers, groups, policies, and other resources in a Windows domain environment.

Instead of maintaining independent local accounts on every computer, an organization can maintain identities centrally within Active Directory. A user can then authenticate against the domain and receive access according to the permissions and policies assigned to that identity.

AD DS provides several important capabilities:

- Centralized identity management.
- Authentication of domain users and computers.
- Authorization through security groups and permissions.
- Centralized configuration through Group Policy.
- Organizational structure through Organizational Units.
- Directory-based management of computers, users, groups, and other objects.
- Integration with DNS for domain discovery and communication.

In this lab, AD DS forms the core of the Windows enterprise environment.

## 2. Domain Controller

A Domain Controller (DC) is a Windows Server system running Active Directory Domain Services that provides authentication and directory services for the domain.

The Domain Controller maintains the Active Directory database and responds to requests from domain members. When a domain user logs in, the client communicates with the Domain Controller to validate the user's credentials and establish the user's domain security context.

The Domain Controller is therefore a critical security component. Compromise of a Domain Controller can provide an attacker with extensive control over identities, computers, policies, and resources throughout the domain.

### Key responsibilities

- Authenticate domain users.
- Authenticate domain computers.
- Store directory objects.
- Process directory queries.
- Participate in Group Policy processing.
- Provide domain-related DNS functionality.
- Maintain domain security information.

## 3. DNS and Active Directory

DNS is fundamental to Active Directory. Domain clients use DNS to locate services provided by Domain Controllers and other domain resources.

A common misconception is that DNS is simply used to translate names into IP addresses. In an Active Directory environment, DNS also contains service records that help clients locate domain services such as LDAP and Kerberos.

The relationship can be summarized as:

```text
Windows Client
      |
      | DNS query
      v
DNS Server / Domain Controller
      |
      | Locate domain services
      v
Domain Controller
      |
      | Authentication / Directory / GPO
      v
Windows Client
```

Correct DNS configuration is therefore essential when joining a Windows client to an Active Directory domain.

## 4. Organizational Units (OUs)

Organizational Units are logical containers within Active Directory used to organize users, computers, groups, and other directory objects.

OUs are especially important because Group Policy can be linked to them. This allows administrators to apply different configurations to different parts of the organization.

For example, an organization could create:

```text
Domain
|
+-- Sales
|   +-- Users
|   +-- Computers
|
+-- HR
|   +-- Users
|   +-- Computers
|
+-- IT
    +-- Users
    +-- Computers
```

The exact structure can vary depending on organizational requirements. The key principle is that OUs should provide meaningful administrative boundaries rather than simply becoming folders for arbitrary objects.

## 5. Domain Users

A domain user is an identity stored in Active Directory rather than only on an individual Windows computer.

Creating users centrally allows administrators to control authentication and authorization from Active Directory. Users can be placed into appropriate OUs and added to security groups according to their responsibilities.

The lab includes practical work involving creation and management of domain users and placement of users into appropriate Organizational Units.

A domain account can be used to authenticate to domain-joined systems, subject to the account's status, credentials, policies, and permissions.

## 6. Security Groups

Security groups are used to simplify authorization and access management.

Instead of assigning permissions individually to every user, administrators can assign users to groups and grant permissions to the groups. This provides a scalable approach to identity and access management.

Example:

```text
User A ----+
User B ----+----> Sales Group ----> Resource Permission
User C ----+
```

If a new employee joins the Sales department, the administrator can add the user to the Sales security group rather than modifying permissions on every resource individually.

This principle is particularly important in enterprise environments because it reduces administrative overhead and supports consistent access control.

## 7. Domain Joining a Windows Client

A Windows workstation becomes a member of the Active Directory domain through the domain-join process.

The process establishes a trust relationship between the client computer and the domain. Once joined, the computer can authenticate against the domain and receive domain-level configuration through Group Policy.

Conceptually:

```text
Windows Client
      |
      | DNS discovery
      v
Domain Controller
      |
      | Domain join / computer account
      v
Active Directory
      |
      | Computer becomes domain member
      v
Windows Client
```

After joining the domain, the client can use domain credentials and process policies associated with its computer account and the user who logs in.

## 8. Group Policy Objects (GPOs)

Group Policy is one of the most important administrative capabilities in a Windows Active Directory environment.

A Group Policy Object contains configuration settings that can be applied to users and computers. Instead of manually configuring every workstation, administrators can define a policy centrally and allow domain members to process that policy.

GPO settings are broadly divided into:

- Computer Configuration.
- User Configuration.

Computer Configuration applies to computers and is processed in the context of the computer account. User Configuration applies to users and is processed according to the user account and the policies within scope.

Examples of policy settings include:

- Control Panel restrictions.
- Password and account policies.
- Security settings.
- Windows configuration settings.
- Administrative templates.
- Desktop restrictions.
- Software and system configuration.
- Windows Defender and security-related settings.

## 9. GPO Scope and Linking

A GPO does not automatically affect every object in the domain merely because it exists.

For a GPO to affect an object, it must be within the appropriate scope. GPOs can be linked at different levels of the Active Directory hierarchy, including:

- Site.
- Domain.
- Organizational Unit.

This provides administrators with granular control over where policies are applied.

For example, a policy linked to a Sales OU can be designed to affect users or computers within that OU without necessarily applying the same configuration to the entire domain.

## 10. GPO Processing Order and Precedence

Understanding GPO precedence is critical when troubleshooting Group Policy.

The traditional processing order is:

```text
Local
  |
Site
  |
Domain
  |
Organizational Unit
```

This is commonly remembered as **LSDOU**.

When multiple policies configure the same setting, the policy with higher precedence can determine the effective configuration, subject to filtering, inheritance, enforcement, and other processing rules.

A simplified example:

```text
Local GPO
    ↓
Site GPO
    ↓
Domain GPO
    ↓
OU GPO
```

If a Domain-level GPO configures a setting and an OU-level GPO configures the same setting differently, the OU-level configuration generally has higher precedence unless inheritance/enforcement behavior changes the effective processing order.

This is why administrators must understand not only which GPO exists, but also where it is linked and how it interacts with other policies.

## 11. Block Inheritance

Block Inheritance is an Active Directory mechanism that prevents GPOs linked at higher levels of the hierarchy from being inherited by a child container.

For example:

```text
Domain
  |
  +-- Domain GPO
  |
  +-- Sales OU
       |
       +-- Block Inheritance
```

With Block Inheritance enabled on the Sales OU, applicable policies inherited from higher levels can be prevented from flowing into that OU.

However, Block Inheritance does not simply mean that every policy above the OU disappears in every circumstance. An **Enforced** GPO can override normal inheritance blocking behavior.

This distinction is important when designing and troubleshooting enterprise Group Policy.

## 12. Enforced GPOs

An Enforced GPO has special inheritance behavior. It is designed to prevent lower-level containers from overriding or blocking the policy through normal inheritance mechanisms.

A simplified example:

```text
Domain
 |
 +-- Security GPO [Enforced]
 |
 +-- Sales OU [Block Inheritance]
```

The enforced policy can continue to apply despite Block Inheritance at the child OU.

Enforced should therefore be used carefully. Excessive use can make Group Policy troubleshooting difficult and can reduce the flexibility of delegated administration.

## 13. GPO Security Filtering

Security Filtering controls which security principals are eligible to apply a GPO.

A GPO can be scoped using security groups or individual security principals. This provides an additional layer of control beyond simply linking the GPO to an OU.

For example:

```text
GPO: Restrict-Control-Panel
        |
        +---- Linked to: Sales OU
        |
        +---- Security Filter: Sales Users
```

The OU determines the broad location-based scope, while Security Filtering can further restrict which users or computers are allowed to process the policy.

Security Filtering should be understood together with the permissions required for Group Policy processing. Simply adding a user to a security filter does not mean the GPO will apply if the rest of the required scope and permissions are not satisfied.

## 14. Example: Restricting Control Panel Access

One practical policy scenario in the lab involved configuring a GPO to control access to Control Panel functionality.

The scenario demonstrates several important Active Directory concepts simultaneously:

1. Create a GPO.
2. Configure a policy setting.
3. Link the GPO to the appropriate OU.
4. Understand inherited policies.
5. Evaluate GPO precedence.
6. Test Block Inheritance behavior.
7. Understand how Enforced policies interact with inheritance.
8. Use Security Filtering when a policy should affect only selected users or computers.
9. Verify the resulting configuration on the client.

This type of exercise is valuable because it moves beyond simply creating a GPO and demonstrates how multiple Group Policy mechanisms interact in a real administrative scenario.

## 15. Group Policy Update

After changing Group Policy, the client does not necessarily need to wait for the normal background processing interval before testing the new configuration.

The following command can be used to manually request a policy refresh:

```cmd
gpupdate /force
```

The `/force` option requests that policy settings be reapplied.

After executing the command, the administrator can verify whether the expected policy was processed and whether the configuration changed on the client.

## 16. Verifying Applied GPOs with gpresult

`gpresult` is an important troubleshooting utility for understanding the effective Group Policy configuration on a Windows system.

A basic command is:

```cmd
gpresult /r
```

This provides a summary of Resultant Set of Policy information, including applied Group Policy Objects and other relevant policy information.

An HTML report can also be generated:

```cmd
gpresult /h C:\Temp\gpresult.html
```

The generated report provides significantly more detail and is useful when troubleshooting why a policy did or did not apply.

## 17. WMIC and WMI-Based Administration

Windows Management Instrumentation (WMI) provides an interface for querying and managing Windows operating-system information.

WMIC was historically used as a command-line interface to WMI. Although WMIC has been deprecated in modern Windows releases, understanding it remains useful when working with older scripts, administrative environments, and existing Windows infrastructure.

Examples of information historically queried through WMIC include:

- Operating-system information.
- Computer information.
- User and process information.
- Installed software.
- Hardware information.

Modern administration increasingly uses PowerShell and CIM/WMI cmdlets, but knowledge of legacy tooling remains valuable for security analysis and troubleshooting.

## 18. Active Directory from a Security Perspective

Active Directory is not only an administration technology; it is a major security boundary within enterprise networks.

Identity compromise can provide attackers with access to systems and resources throughout a domain. Misconfigured privileges, weak passwords, excessive group membership, insecure service accounts, and poorly controlled Group Policy can significantly increase the attack surface.

Important security concepts associated with Active Directory include:

- Least privilege.
- Privileged account protection.
- Secure authentication.
- Group membership management.
- Delegation of administrative privileges.
- Domain Controller protection.
- GPO security.
- Account lifecycle management.
- Monitoring authentication events.
- Detection of abnormal administrative activity.

These concepts are directly relevant to SOC operations because many Windows security events originate from Active Directory authentication and authorization activity.

## 19. Relevance to SOC Operations

The Active Directory lab provides an important foundation for Windows-based security monitoring.

A SOC analyst frequently investigates events involving:

- User logons and logoffs.
- Failed authentication attempts.
- Account creation.
- Account deletion.
- Group membership changes.
- Privilege escalation.
- Password changes and resets.
- Computer account activity.
- GPO changes.
- Administrative activity.
- Suspicious authentication patterns.

Understanding how Active Directory works makes these events significantly easier to interpret.

For example, an analyst investigating a suspicious group-membership change should understand why membership in a privileged group is important, where the change occurs, which account performed it, and what downstream access the affected account may receive.

## 20. Troubleshooting Approach

A structured troubleshooting methodology is essential when working with Active Directory.

### DNS problems

Check whether the client is using the correct DNS server. Incorrect DNS configuration can prevent domain discovery and cause domain-join or authentication failures.

### Domain authentication problems

Verify:

- Username and password.
- Domain membership.
- Network connectivity.
- DNS resolution.
- Domain Controller availability.
- Account status.

### GPO not applying

Check:

1. Whether the GPO is linked to the correct container.
2. Whether the user/computer belongs to the expected OU.
3. Whether Security Filtering allows the principal to apply the policy.
4. Whether inheritance is blocked.
5. Whether the GPO is enforced.
6. Whether another GPO has higher precedence.
7. Whether the policy has actually refreshed on the client.
8. The output of `gpresult /r` or an HTML `gpresult` report.

### Client not receiving domain configuration

Verify domain connectivity, DNS configuration, computer membership, Group Policy processing, and relevant Windows event logs.

## 21. Practical Skills Demonstrated

This project demonstrates practical experience with:

- Windows Server administration.
- Active Directory Domain Services.
- Domain Controller deployment.
- DNS fundamentals in an AD environment.
- User and computer account management.
- Organizational Units.
- Security groups.
- Domain joining.
- Group Policy administration.
- GPO linking.
- GPO precedence.
- Group Policy inheritance.
- Block Inheritance.
- Enforced GPOs.
- Security Filtering.
- Windows client administration.
- Group Policy troubleshooting.
- `gpupdate` and `gpresult`.
- WMI/WMIC concepts.
- Windows security administration.
- Enterprise identity and access management fundamentals.

## 22. Project Structure

The repository can be expanded as the lab develops. A recommended structure is:

```text
Active_Directory/
|
+-- README.md
|
+-- documentation/
|   +-- domain-controller.md
|   +-- users-and-groups.md
|   +-- organizational-units.md
|   +-- group-policy.md
|   +-- troubleshooting.md
|
+-- screenshots/
|   +-- domain-controller/
|   +-- users-and-groups/
|   +-- gpo/
|   +-- client/
|
+-- scripts/
    +-- powershell/
    +-- cmd/
```

Screenshots and configuration evidence can be added as the lab evolves. Sensitive information such as passwords, private keys, authentication tokens, personal IP information, or other secrets should never be committed to the repository.

## 23. Future Expansion

The lab can be extended into a more security-focused Active Directory environment by adding:

- Multiple Domain Controllers.
- Windows Server hardening.
- Advanced Group Policy security controls.
- Active Directory auditing.
- Windows Event Forwarding.
- Sysmon telemetry.
- PowerShell logging.
- Windows Defender configuration.
- Privileged Access Management concepts.
- Kerberos authentication analysis.
- LDAP security.
- NTLM authentication analysis.
- Active Directory attack-path analysis.
- Detection of suspicious account activity.
- SIEM integration.
- Wazuh-based Windows monitoring.
- Authentication attack detection.
- Privilege escalation detection.
- Lateral movement detection.

These extensions can transform the environment from a basic Windows administration lab into a security-focused enterprise Active Directory detection and response lab.

## Conclusion

This Active Directory lab provides a practical foundation for understanding enterprise Windows identity infrastructure. The project covers the complete path from deploying a Domain Controller and organizing directory objects to implementing and troubleshooting Group Policy.

The most important outcome is understanding how the components work together rather than treating each configuration as an isolated task. Active Directory, DNS, users, groups, OUs, domain-joined computers, and Group Policy form an interconnected identity and management ecosystem.

This foundation is directly applicable to Windows system administration, identity and access management, SOC analysis, incident response, threat detection, and enterprise security engineering.

## Repository

**GitHub:** https://github.com/Dheeraj146/Active_Directory
