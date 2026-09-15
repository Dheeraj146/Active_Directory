# GPO Precedence and LSDOU

## Processing Order

Group Policy is processed through a hierarchy commonly summarized as **LSDOU**:

```text
Local
  ↓
Site
  ↓
Domain
  ↓
Organizational Unit
```

When multiple policies configure the same setting, the effective result depends on processing order, precedence, inheritance, enforcement, filtering, and the specific policy setting.

## Why Precedence Matters

A policy can exist and be correctly configured but still not produce the expected result because another policy has higher precedence or because the policy is outside the effective scope.

For example, a domain-linked policy may configure one value while an OU-linked policy configures a conflicting value. The OU-level policy normally has later processing precedence in the hierarchy.

## Practical Lab

1. Create a domain-level test GPO.
2. Configure an observable setting.
3. Confirm the client receives it.
4. Create an OU-level GPO that configures the same setting differently.
5. Link it to the OU containing the test object.
6. Refresh policy.
7. Observe the final result.
8. Use the policy report to determine why the winning configuration was selected.

## Troubleshooting Questions

When a setting is unexpected, ask:

- Which GPO configured the setting?
- Where is the GPO linked?
- Is the object in the expected OU?
- Is inheritance blocked?
- Is the GPO enforced?
- Does Security Filtering permit processing?
- Is another GPO configuring the same setting?

## Evidence

Capture the Group Policy Management hierarchy and the resulting policy report. Explain the observed winner rather than merely stating that one GPO has precedence.
