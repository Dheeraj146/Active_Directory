# Active Directory Domain Services (AD DS)

## Overview

Active Directory Domain Services (AD DS) is Microsoft's directory service for Windows domain environments. It provides centralized management of identities, computers, groups, organizational structure, authentication, authorization, and Group Policy.

The purpose of AD DS is to replace isolated workstation administration with a centrally managed identity and security model. A domain user can authenticate to domain-joined systems, while administrators can manage users, computers, groups, and policies from the directory.

## Core Concepts

### Domain

A domain is a logical administrative and security boundary containing directory objects and domain services. The lab uses the domain `cloud.com`.

### Directory Objects

Important objects include users, computers, security groups, and Organizational Units. Each object has attributes that describe its identity and configuration.

### Domain Controller

A Domain Controller hosts AD DS and provides directory and authentication services to domain members. It is a critical security asset because compromise of a privileged Domain Controller can affect the wider domain.

### Authentication

Authentication establishes the identity of a user or computer. In a modern Active Directory environment, Kerberos is the primary authentication protocol, while NTLM remains relevant for compatibility and security investigations.

### Authorization

Authorization determines what an authenticated identity can access. Security groups, permissions, and delegated administration are commonly used to implement authorization.

## AD DS and DNS

Active Directory depends heavily on DNS. Clients use DNS to locate domain services and Domain Controllers. This is why a domain client should normally use the AD DNS infrastructure rather than an unrelated public resolver for domain discovery.

## Practical Work

On the Domain Controller, open Active Directory Users and Computers and inspect:

- Default containers.
- Domain Controllers.
- Users.
- Computers.
- SALES OU.
- TECH OU.
- Test OU.

Create test directory objects and observe where they appear in the directory hierarchy.

## Verification

Verify that the Domain Controller can manage the directory and that the client can authenticate to the domain. Record the result and capture screenshots as evidence.

## Security Relevance

AD DS is a major enterprise identity security boundary. Important defensive priorities include least privilege, privileged-account protection, strong authentication, controlled group membership, Domain Controller hardening, auditing, and monitoring of identity changes.
