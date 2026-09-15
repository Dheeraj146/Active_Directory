# Windows Administration Commands

This is a practical command reference for the Windows client and Domain Controller used in the lab.

## Identity and Host Information

```cmd
hostname
whoami
systeminfo
```

Use these to identify the machine, logged-on security context, and operating-system information.

## Network Troubleshooting

```cmd
ipconfig /all
ping <IP-address>
nslookup <domain>
```

`ipconfig /all` is especially useful for checking whether the client is using the expected DNS server.

## Group Policy

```cmd
gpupdate /force
gpresult /r
```

Generate an HTML policy report when detailed investigation is required.

## PowerShell / CIM

Modern Windows administration commonly uses PowerShell and CIM rather than legacy WMIC. Examples:

```powershell
Get-CimInstance Win32_OperatingSystem
Get-CimInstance Win32_ComputerSystem
Get-CimInstance Win32_Process
```

## Active Directory PowerShell

When the Active Directory module is installed, administrators can query directory objects through PowerShell. Examples include retrieving domains, users, groups, and computers.

## Practical Method

Do not run commands without a purpose. For each command in the lab, document:

- What the command does.
- Why it is being used.
- Expected output.
- Actual output.
- What the result tells the administrator.

## Security Relevance

These commands are useful to administrators and defenders, but many are also common in attacker discovery activity. SOC analysts should understand the normal administrative use of these tools so that unusual execution can be investigated in context.
