# GPO Troubleshooting

## Problem

A user or computer is not receiving an expected Group Policy setting.

Do not immediately recreate the GPO. Determine where policy processing is failing.

## Troubleshooting Workflow

```text
1. Is the object in the expected OU?
          ↓
2. Is the GPO linked to that OU or an inherited parent?
          ↓
3. Is the GPO enabled?
          ↓
4. Is the relevant User/Computer configuration enabled?
          ↓
5. Does Security Filtering include the target?
          ↓
6. Is inheritance blocked?
          ↓
7. Is another policy taking precedence?
          ↓
8. Refresh Group Policy
          ↓
9. Inspect resultant policy information
          ↓
10. Check Windows event logs if required
```

## Policy Refresh

Run a manual Group Policy refresh on the client and record the result. A successful refresh only proves that processing was requested; it does not prove that every policy was applied.

## Resultant Policy

Use the Windows policy-result tools to determine:

- Applied GPOs.
- Denied GPOs.
- Security filtering results.
- Effective policy settings.
- Processing information.

## Common Root Causes

### Wrong OU

The user or computer may be located in a different container from the one expected by the administrator.

### Incorrect GPO link

The GPO may exist but not be linked to the required domain, site, or OU.

### Security Filtering

The target may not have the required scope or permissions to process the GPO.

### Block Inheritance

A child OU may prevent normal inherited policies from reaching the object.

### Enforced policy

A higher-level enforced policy can alter the normal inheritance behavior and may prevent expected lower-level behavior.

### Precedence conflict

Another GPO may configure the same setting later in processing and determine the effective result.

## Practical Exercise

Intentionally create a GPO configuration that does not apply. Investigate it without changing multiple variables at once. Identify the root cause, correct it, refresh policy, and verify the final result.

## Evidence

Document:

- Initial symptom.
- Investigation steps.
- Relevant screenshot.
- Resultant policy output.
- Root cause.
- Corrective action.
- Final verification.
