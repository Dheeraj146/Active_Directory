# Active Directory Practical Labs

This file is the practical index for the Active Directory environment. Each exercise should be completed in the isolated lab and documented with configuration evidence, commands, expected results, actual results, and troubleshooting notes.

## Lab 01 — Windows Server Preparation

Build the Windows Server 2022 VM, configure its hostname and networking, and prepare it for AD DS.

## Lab 02 — Domain Controller Deployment

Install AD DS, create the forest/domain, promote the server, and verify the Domain Controller and DNS services.

## Lab 03 — Active Directory Structure

Create SALES, TECH, and Test OUs and organize test directory objects.

## Lab 04 — User and Group Administration

Create test users, create security groups, modify membership, and verify object placement.

## Lab 05 — Windows Client Domain Join

Build a Windows 10/11 client, configure DNS toward the Domain Controller, join the domain, and verify domain authentication.

## Lab 06 — GPO Creation and Linking

Create a GPO, configure a test setting, link it to an OU, refresh the client, and verify the result.

## Lab 07 — GPO Precedence

Create conflicting settings at different hierarchy levels and determine which policy becomes effective.

## Lab 08 — Block Inheritance

Apply a higher-level GPO, enable Block Inheritance on a child OU, and observe the resulting policy behavior.

## Lab 09 — Enforced GPO

Repeat the inheritance experiment with the higher-level policy marked Enforced and compare the outcome.

## Lab 10 — Security Filtering

Use two test identities in the same OU and configure Security Filtering so that a policy targets only the intended principal.

## Lab 11 — Control Panel Restriction

Create and test a practical GPO that restricts Control Panel access. Record the exact policy path, scope, filter, refresh process, and client result.

## Lab 12 — GPO Troubleshooting

Investigate a deliberately misconfigured policy using a structured workflow involving OU placement, links, filtering, inheritance, precedence, policy refresh, and resultant policy information.

## Lab 13 — Windows Administration

Practice Windows administration through Server Manager, Active Directory consoles, DNS tools, Group Policy Management, CMD, and PowerShell.

## Lab 14 — Security Monitoring Foundation

Identify which Active Directory and Windows activities should be logged and monitored for security operations. This provides the foundation for later SIEM/Wazuh integration.

## Documentation Standard

For every practical, record:

1. Objective.
2. Prerequisites.
3. Lab topology.
4. Configuration steps.
5. Commands/tools used.
6. Screenshot evidence.
7. Expected result.
8. Actual result.
9. Troubleshooting performed.
10. Security relevance.
11. Final state.
