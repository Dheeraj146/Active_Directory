# GPO Security Filtering

## Overview

Security Filtering controls which security principals are eligible to process a GPO. It adds an authorization-oriented layer to the broader scope established by the GPO link and directory hierarchy.

## Scope Model

A useful way to reason about GPO application is:

```text
GPO Link
   ↓
Object in applicable hierarchy
   ↓
Security Filtering / permissions
   ↓
Inheritance and precedence
   ↓
Policy processing
```

A user being inside an OU does not automatically mean every GPO linked to that OU will apply. Scope and permissions must also allow processing.

## Practical Lab

1. Create two test users.
2. Place both in the same OU.
3. Create a test GPO.
4. Configure an observable policy setting.
5. Link the GPO to the OU.
6. Configure Security Filtering for one test security principal or group.
7. Refresh both client sessions.
8. Compare the results.
9. Inspect the policy report for each user.

## Expected Outcome

The filtered identity should receive the GPO when all required scope and permissions conditions are satisfied. The other test identity should not receive the policy if it is outside the effective security scope.

## Troubleshooting

If filtering appears not to work, verify:

- The correct user or group is in the filter.
- The GPO is linked to the correct OU.
- The target object is actually in that OU.
- Required Group Policy permissions are present.
- Inheritance is not changing the effective scope.
- Another policy is not producing a conflicting setting.

## Security Relevance

Security Filtering is useful for controlled deployment of security and configuration policies. It should be used deliberately and documented clearly so that administrators can understand why a policy applies to one identity but not another.
