# Organizational Units (OUs)

## Overview

An Organizational Unit is a logical container inside Active Directory used to organize directory objects and establish administrative boundaries. OUs are especially important because Group Policy can be linked to them.

## Lab Structure

The current lab contains:

```text
cloud.com
├── SALES OU
├── TECH OU
└── Test OU
```

The default containers such as Users, Computers, and Domain Controllers are also present.

## Why OUs Matter

OUs should be designed around administration and policy requirements rather than treated as simple folders. For example, users in Sales may need a different desktop policy from users in Technical teams.

OUs can therefore provide:

- Logical organization.
- GPO scope.
- Delegated administration boundaries.
- Separation of users and computers where appropriate.

## Practical: Create an OU

1. Open **Active Directory Users and Computers**.
2. Select the domain.
3. Choose **New → Organizational Unit**.
4. Enter a meaningful name.
5. Confirm creation.
6. Create additional OUs required by the lab.

## Practical: Move an Object

Create a test user and move the account into the appropriate OU. Observe that the user's directory path changes.

The same principle can be applied to computer accounts.

## OU and GPO Relationship

A GPO can be linked to an OU so that users or computers inside the OU become part of the policy scope. This is one of the most important reasons OUs are used in enterprise Active Directory designs.

## Troubleshooting

If a GPO does not affect a user or computer, verify that the object is actually located in the expected OU. Also verify the GPO link, inheritance, Security Filtering, and policy processing.

## Evidence

Capture the Active Directory Users and Computers window showing the domain and the OUs. This screenshot is useful as evidence of the practical directory design.
