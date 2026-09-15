# DNS and Active Directory

## Why DNS Matters

DNS is a foundational dependency of Active Directory. Domain members use DNS to discover Domain Controllers and other domain services. A client may have network connectivity and still fail to join or authenticate to a domain if DNS is incorrectly configured.

## AD DNS

In a typical small lab, the Domain Controller also provides DNS for the AD namespace. The Windows client should be configured to use the AD DNS server for domain resolution.

## Practical Checks

On the client, inspect the configured DNS server and test name resolution for the lab domain.

Useful commands include:

```cmd
ipconfig /all
nslookup cloud.com
ping <DC-IP>
```

The purpose of each test is different:

- `ipconfig /all` shows addressing and DNS configuration.
- `nslookup` tests DNS resolution.
- `ping` can provide a basic connectivity check, although ICMP availability is not proof that every domain service is functioning.

## DNS Manager

On the Domain Controller:

```text
Server Manager
→ Tools
→ DNS
```

Inspect the forward lookup zone for the AD namespace and review the records created for domain services.

## Troubleshooting Workflow

```text
Client IP configuration
        ↓
DNS server configuration
        ↓
DNS resolution
        ↓
Domain Controller connectivity
        ↓
Domain discovery
        ↓
Authentication / domain join
```

Always establish whether the failure is network connectivity, DNS resolution, domain discovery, authentication, or policy processing before changing multiple settings.

## Evidence

Capture:

- Client `ipconfig /all` output.
- Successful `nslookup` result.
- DNS Manager.
- Relevant domain records.
- Successful domain join or authentication.
