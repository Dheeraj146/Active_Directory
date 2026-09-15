# Active Directory Practical Labs

These exercises are designed to be performed in the lab and documented with screenshots and verification commands.

## Lab 01 — Domain Controller Deployment

### Goal
Build a Windows Server 2022 Domain Controller.

### Tasks

- Install Windows Server 2022.
- Rename the machine to `DC01`.
- Configure a static IP.
- Install AD DS.
- Create a new forest.
- Promote the server.
- Verify AD DS and DNS.

### Evidence

Capture Server Manager and the Active Directory Users and Computers console after successful promotion.

---

## Lab 02 — Active Directory Structure

### Goal
Create a manageable directory structure.

### Tasks

Create:

```text
SALES OU
TECH OU
Test OU
```

Create test users and move them into the appropriate OUs.

### Verification

Open Active Directory Users and Computers and confirm the object placement.

---

## Lab 03 — Domain Client Join

### Goal
Join Windows 10/11 to the Active Directory domain.

### Tasks

1. Configure the client network.
2. Point DNS to the Domain Controller.
3. Test connectivity.
4. Join the domain.
5. Restart.
6. Log in with a domain account.

### Verification

```cmd
ipconfig /all
nslookup <domain>
whoami
systeminfo
```

---

## Lab 04 — Create and Link a GPO

### Goal
Understand the complete GPO lifecycle.

### Tasks

1. Open Group Policy Management.
2. Create a GPO.
3. Give it a meaningful name.
4. Link it to the Test OU.
5. Configure a visible policy setting.
6. Log into the client.
7. Run `gpupdate /force`.
8. Verify the result.

---

## Lab 05 — GPO Precedence

### Goal
Understand what happens when multiple GPOs configure the same setting.

### Tasks

Create policies at different levels and configure the same setting differently.

Example:

```text
Domain GPO -> Setting A
OU GPO     -> Setting B
```

Determine the effective configuration and explain why it wins.

### Verification

```cmd
gpresult /r
```

---

## Lab 06 — Block Inheritance

### Goal
Observe the effect of inheritance blocking.

### Tasks

1. Create a Domain-level GPO.
2. Confirm it affects the test client.
3. Create an OU.
4. Enable Block Inheritance on the OU.
5. Place a test user/computer in the OU.
6. Refresh policy.
7. Compare the result.

---

## Lab 07 — Enforced GPO

### Goal
Understand the relationship between Enforced policies and Block Inheritance.

### Tasks

Repeat Lab 06 with the higher-level GPO configured as Enforced.

Compare:

```text
Normal GPO + Block Inheritance
```

with:

```text
Enforced GPO + Block Inheritance
```

Use `gpresult` to verify the outcome.

---

## Lab 08 — Security Filtering

### Goal
Apply a GPO to a selected security principal rather than every object in an OU.

### Tasks

1. Create two test users.
2. Place both users in the same OU.
3. Create a GPO.
4. Configure a visible policy setting.
5. Configure Security Filtering.
6. Refresh both sessions.
7. Compare behavior.
8. Use `gpresult` to investigate the result.

---

## Lab 09 — Control Panel Restriction

### Goal
Build an end-to-end policy scenario.

### Tasks

- Create the GPO.
- Configure the Control Panel restriction.
- Link the GPO.
- Configure Security Filtering if required.
- Refresh the client.
- Verify the restriction.
- Remove or modify the policy and verify the change.

This lab combines GPO creation, scope, inheritance, filtering, client refresh, and validation.

---

## Lab 10 — GPO Troubleshooting

### Scenario
A user reports that a policy is not being applied.

### Investigation

Do not immediately recreate the GPO. Follow the troubleshooting chain:

```text
OU placement
     ↓
GPO link
     ↓
GPO enabled?
     ↓
Security Filtering
     ↓
Inheritance
     ↓
Enforcement
     ↓
Precedence
     ↓
gpupdate /force
     ↓
gpresult /r
```

Document the root cause and corrective action.

---

## Lab 11 — Windows Administration Commands

Practice the following commands from the client:

```cmd
ipconfig /all
hostname
whoami
systeminfo
gpupdate /force
gpresult /r
nslookup <domain>
ping <domain-controller-ip>
```

For modern PowerShell-based administration, practice:

```powershell
Get-CimInstance Win32_OperatingSystem
Get-CimInstance Win32_ComputerSystem
Get-CimInstance Win32_Process
```

---

## Lab 12 — Evidence-Based Documentation

For each completed practical:

1. State the objective.
2. Record the environment.
3. Document the configuration.
4. Capture screenshots.
5. Run verification commands.
6. Record the observed result.
7. Explain why the result occurred.
8. Record troubleshooting steps if anything failed.
9. Document the final state.

This turns the lab into reproducible technical evidence rather than a collection of screenshots.
