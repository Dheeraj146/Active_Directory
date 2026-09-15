# Users and Security Groups

## Users

A domain user account is an identity stored in Active Directory. Centralized user management allows administrators to control authentication, account status, group membership, and access from the directory.

## Practical: Create a User

1. Open **Active Directory Users and Computers**.
2. Select the required OU.
3. Choose **New → User**.
4. Enter the user's identity information.
5. Configure the account password and account options.
6. Finish the wizard.
7. Verify that the user appears inside the intended OU.

Create test accounts rather than using real credentials in the lab.

## Security Groups

Security groups allow administrators to manage access collectively. Instead of assigning permissions individually to many users, users can be placed into a group and the required permissions can be assigned to the group.

Example:

```text
SalesUser01 ─┐
SalesUser02 ─┼──> Sales Security Group ──> Resource Access
SalesUser03 ─┘
```

## Practical: Group Membership

1. Create a test security group.
2. Add test users to the group.
3. Inspect each user's **Member Of** information.
4. Inspect the group's membership.
5. Remove a user and observe the membership change.

## Why This Matters for Security

Group membership is an important authorization boundary. Membership in privileged groups can grant extensive administrative rights, so group changes should be controlled and monitored.

## Evidence

Capture:

- User objects in the appropriate OU.
- Security group membership.
- User properties showing group membership where appropriate.

Never commit passwords or other authentication secrets to the repository.
