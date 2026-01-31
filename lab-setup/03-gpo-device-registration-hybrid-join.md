# 03 - Domain Join the Windows 11 VM (Lab Client)

## Goal
Join the **Windows 11 Hyper-V VM** to the on-prem domain `jmcnairtech.local` so it can:
- Receive GPOs (Step 04)
- Validate hybrid join plumbing before Autopilot/ODJ steps

## Why this matters
Hybrid Entra registration depends on the device being **domain joined** and able to read the SCP/GPO settings.
This step prepares the client for Step 04 (automatic device registration).

---

## What I configured (high level)
- Confirmed the Win11 VM is on the lab network (`10.0.0.0/24`) and using DC DNS (`10.0.0.10`)
- Joined the VM to the domain: `jmcnairtech.local`
- Rebooted and confirmed domain membership
- Confirmed the computer object exists in Active Directory

---

## Configuration Notes
- Domain: `jmcnairtech.local`
- DC: `Lab-DC01` (`10.0.0.10`)
- Lab subnet: `10.0.0.0/24`
- The computer object may land in **Computers** by default unless redirected; OU placement is handled later during Autopilot/ODJ.

---

## Validation

### A) On the Windows 11 VM
Confirm DNS points to the DC:
- `ipconfig /all`
  - DNS Servers should include: `10.0.0.10`

Confirm domain join status:
- Settings → System → About → Domain or workgroup
  - Shows domain: `jmcnairtech.local`

(Optional command validation)
- `whoami`
  - Should show `JMCNAIRTECH\username` when logged in with a domain account

### B) On Lab-DC01 (ADUC)
- Verify the computer account exists:
  - Default location: `CN=Computers,DC=jmcnairtech,DC=local`
  - Or inside your lab device OU if you moved it manually

---

## Screenshots to Capture (Step 03)

Minimum:
- `03-win11-domain-joined.png`
  - Win11 “About” (or System info) showing domain: `jmcnairtech.local`
- `03-ad-computer-object.png`
  - ADUC showing the computer account exists (Computers container or chosen OU)

Optional:
- `03-win11-ipconfig-dns.png`
  - `ipconfig /all` showing DNS Server = `10.0.0.10`

---

## Done Criteria (to move to Step 04)
You’re ready to proceed when:
- Win11 VM is domain joined to `jmcnairtech.local`
- Computer object exists in AD
- Step 03 screenshots are captured
