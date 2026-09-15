# Windows 10 and Windows 11 Client Setup

## Objective

Build a Windows workstation that can communicate with the Domain Controller and join the Active Directory domain.

## Download

Use Microsoft's official download pages:

- Windows 10: https://www.microsoft.com/en-us/software-download/windows10
- Windows 11: https://www.microsoft.com/en-us/software-download/windows11

Windows 10 reached end of support on October 14, 2025. For a current lab client, Windows 11 is preferable; Windows 10 remains useful for legacy-client testing in an isolated environment.

## VM Configuration

Recommended starting configuration:

- CPU: 2–4 vCPUs
- RAM: 4–8 GB
- Disk: 50 GB or more
- Network: same virtual network as the Domain Controller

## Install Windows

1. Create a client VM.
2. Mount the Windows ISO.
3. Boot from the ISO.
4. Complete the Windows installation.
5. Create a temporary local administrator account.
6. Install required VM integration tools/drivers.
7. Apply appropriate lab updates.
8. Verify network connectivity.

## Configure Network Connectivity

The client must be able to communicate with the Domain Controller.

Example:

```text
DC01:      192.168.56.10
CLIENT01:  192.168.56.20
Subnet:    255.255.255.0
DNS:       192.168.56.10
```

The important point is that the client's DNS server should resolve the Active Directory domain and locate domain services.

Verify:

```cmd
ipconfig /all
ping <DC-IP>
nslookup cloud.com
```

## Join the Domain

Open:

```text
Settings / System Properties
→ Rename this PC (advanced)
→ Computer Name
→ Change
→ Member of: Domain
```

Enter the lab domain, for example:

```text
cloud.com
```

Provide domain credentials when requested and restart the workstation.

## Verify Domain Membership

After restart, select **Other user** or the domain sign-in option and authenticate with a test domain account.

Run:

```cmd
whoami
```

Example:

```text
CLOUD\SalesUser01
```

Also check:

```cmd
systeminfo
```

and:

```cmd
hostname
ipconfig /all
```

## Test Active Directory Policy Processing

Once logged into the domain:

```cmd
gpupdate /force
gpresult /r
```

This establishes whether the workstation can successfully communicate with the domain and process applicable Group Policy.

## Practical Troubleshooting

### Domain cannot be found

Check DNS first:

```cmd
ipconfig /all
nslookup cloud.com
```

Confirm that the client is using the Domain Controller as its DNS server.

### Authentication fails

Check:

- Correct domain name.
- Account status.
- Credentials.
- Network connectivity.
- DNS resolution.
- Time synchronization.
- Domain membership.

### GPO does not apply

Check OU placement, GPO link, Security Filtering, inheritance, precedence, and `gpresult` output.

## Evidence

Capture:

- Client IP configuration.
- DNS configuration.
- System Properties showing domain membership.
- Successful domain login.
- `whoami` output.
- `gpupdate /force`.
- `gpresult /r`.
- Final visible policy result.
