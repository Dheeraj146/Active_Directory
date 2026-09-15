# Active Directory Security Monitoring and SOC Relevance

## Overview

Active Directory generates security-relevant activity involving authentication, identity management, authorization, computer accounts, and administrative changes. A SOC analyst needs to understand normal AD behavior before abnormal behavior can be identified reliably.

## Important Monitoring Areas

### Authentication

Monitor successful and failed logons, unusual authentication patterns, repeated failures, unexpected source systems, and suspicious use of privileged accounts.

### Account Management

Monitor user creation, deletion, enable/disable operations, password changes, and other account lifecycle events.

### Group Membership

Monitor changes to security groups, especially privileged groups. A group-membership change can immediately change the effective privileges of an account.

### Administrative Activity

Track administrative actions on Domain Controllers and other critical Windows systems.

### Group Policy

Monitor important GPO creation, modification, linking, and deletion activity. A malicious or accidental policy change can affect many systems simultaneously.

## Investigation Model

```text
Windows / AD Event
        ↓
Log Collection
        ↓
Normalization / Parsing
        ↓
Detection Rule
        ↓
Alert
        ↓
Analyst Triage
        ↓
Investigation
        ↓
Containment / Response
```

## Practical SOC Exercises

Use the lab to generate controlled administrative activity and observe the corresponding Windows events. Examples include test authentication failures, controlled account changes, group-membership changes, and approved policy modifications.

For each exercise document:

- What action was performed.
- Which identity performed it.
- Which system generated the event.
- What event data was produced.
- What a detection rule should look for.
- What would make the activity suspicious.

## Future SIEM Integration

The environment can later be integrated with Wazuh or another SIEM. The goal is to move from simply collecting Windows logs to building detections and investigation workflows around Active Directory activity.

Potential detection themes include:

- Repeated authentication failures.
- Suspicious privileged-group changes.
- Unexpected administrative logons.
- Abnormal account creation.
- Security-policy or GPO changes.
- Suspicious PowerShell activity.
- Lateral-movement indicators.

This creates a direct bridge between Windows/AD administration and SOC analyst workflows.
