# Group Policy Objects (GPO)

## Overview

Group Policy provides centralized configuration management for Windows users and computers in an Active Directory environment. A GPO contains policy settings that can be applied according to its scope.

GPO settings are divided broadly into **Computer Configuration** and **User Configuration**.

## GPO Lifecycle

```text
Create GPO
   ↓
Configure settings
   ↓
Link GPO
   ↓
Determine scope
   ↓
Client processes policy
   ↓
Verify effective settings
```

## Practical: Create a GPO

1. Open **Group Policy Management**.
2. Select the required domain or OU.
3. Create a new GPO.
4. Give it a descriptive name.
5. Edit the GPO.
6. Configure a test policy.
7. Link it to the intended OU.
8. Refresh policy on the client.
9. Verify the result.

## Control Panel Practical

The lab includes a Control Panel restriction scenario. Use a dedicated test OU and test account so that the configuration can be safely evaluated.

Document:

- GPO name.
- Policy path.
- Setting selected.
- OU link.
- Security filter.
- Expected behavior.
- Actual client behavior.

## Policy Refresh

On the Windows client, manually refresh Group Policy after a change and then verify the resulting policy. Record both the command output and the visible client-side result.

## Verification

Use Resultant Set of Policy information to determine which GPOs were applied and which were denied or filtered. A successful visible result is useful, but the policy report is the stronger evidence because it explains the processing result.

## Troubleshooting Checklist

If a GPO does not apply, inspect:

1. Object OU placement.
2. GPO link.
3. GPO enabled state.
4. User/computer configuration branch.
5. Security Filtering.
6. Inheritance.
7. Enforcement.
8. Precedence.
9. Policy refresh.
10. Resultant policy information.

## Evidence

Capture the Group Policy Management console, the configured policy setting, the GPO link, filtering configuration, client policy refresh, and final client behavior.
