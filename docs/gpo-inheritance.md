# GPO Inheritance, Block Inheritance and Enforced Policies

## Inheritance

GPOs linked at higher levels of the Active Directory hierarchy can normally be inherited by child containers. This allows common policies to be defined once and applied to multiple OUs.

## Block Inheritance

Block Inheritance is configured on a container to prevent applicable inherited GPOs from higher levels from flowing into that container.

Example:

```text
Domain
├── Domain Policy
└── SALES OU
    └── Block Inheritance
```

The block affects normal inherited policies. It does not mean that every possible policy relationship is ignored.

## Enforced

An Enforced GPO has special inheritance behavior and is designed to maintain the policy's influence even where lower-level inheritance blocking would otherwise prevent it.

Example:

```text
Domain
├── Security GPO [Enforced]
└── SALES OU
    └── Block Inheritance
```

This is why Enforced and Block Inheritance should be tested together when learning Group Policy.

## Practical Comparison

Perform two tests with the same observable policy:

### Test A

- Link the policy at a higher level.
- Enable Block Inheritance on the child OU.
- Refresh policy.
- Record the result.

### Test B

- Mark the higher-level GPO as Enforced.
- Keep Block Inheritance on the child OU.
- Refresh policy.
- Record the result.

Compare the two results and explain the difference.

## Evidence

Capture:

- GPO link location.
- Block Inheritance setting.
- Enforced indicator.
- Resultant policy information.
- Final client behavior.

## Administrative Consideration

Enforcement should be used intentionally. Excessive enforcement can make policy relationships difficult to understand and troubleshoot.
