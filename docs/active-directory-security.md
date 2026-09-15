# Active Directory Security

## Why Active Directory Is Security-Critical

Active Directory controls identities, computers, groups, authentication, and policy across many Windows environments. A compromise of a privileged identity can therefore have consequences far beyond a single workstation.

## Security Priorities

### Least Privilege

Users and administrators should receive only the permissions required for their responsibilities. Privileged memberships should be limited and reviewed.

### Privileged Accounts

Administrative accounts should be protected separately from ordinary user accounts. Avoid using highly privileged identities for routine workstation activity.

### Group Membership

Membership changes can materially change access. Privileged-group modifications should be controlled and monitored.

### Domain Controller Protection

Domain Controllers should be hardened, patched, access-controlled, and closely monitored.

### Group Policy Security

GPOs can enforce security configuration across many systems. Poorly controlled GPO permissions or malicious policy changes can therefore create a large blast radius.

## Practical Security Exercises

- Review privileged and non-privileged group membership in the lab.
- Identify which OUs receive security policies.
- Review which identities can administer the Domain Controller.
- Create a controlled policy-change scenario and document the expected security impact.
- Review Windows security events associated with authentication and account changes.

## SOC Perspective

Important activities to monitor include:

- Successful and failed authentication.
- New user creation.
- Account enable/disable activity.
- Password changes and resets.
- Security-group membership changes.
- Privileged-account use.
- Computer-account changes.
- Group Policy modifications.
- Suspicious administrative activity.

The lab can later be connected to a SIEM so that these activities become detection and investigation exercises.
