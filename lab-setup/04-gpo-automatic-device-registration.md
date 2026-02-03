# 04 - GPO Automatic Device Registration (Hybrid Join)

## Goal
Enable **automatic device registration** so domain-joined Windows devices register into **Microsoft Entra ID** and show as **Microsoft Entra hybrid joined**.

## Why this matters
- Step 02 (SCP) tells the device **which tenant** to register with.
- Step 04 (GPO) tells the device **to perform** the registration.
This is the trigger that makes hybrid join happen after domain join.

---

## What I configured (high level)
- Created and linked a GPO to enable **Register domain-joined computers as devices**
- Applied policy to the Windows 11 VM
- Validated registration using `gpresult` and `dsregcmd /status`

---

## Configuration

### GPO name
`GPO - Entra Hybrid Join (Auto Device Registration)`

### Setting
Computer Configuration  
→ Policies  
→ Administrative Templates  
→ Windows Components  
→ Device Registration  

**Register domain-joined computers as devices** = **Enabled**

### Scope
- Linked at the domain level (`jmcnairtech.local`) for lab simplicity  
  (Can be scoped later to device OUs in production)

---

## Validation (Windows 11 VM)

### A) Confirm the GPO applied
Run (Admin CMD):
- `gpupdate /force`
- `gpresult /r`

Expected:
- The GPO appears under **Applied Group Policy Objects** (Computer Settings)

### B) Check hybrid join status
Run (Admin CMD):
- `dsregcmd /status`

Expected signals (timing can vary):
- `DomainJoined : YES`
- `AzureAdJoined : YES` 
- Registration indicators present (DeviceId populated / tenant info visible)
- Device appears in Entra as **Microsoft Entra hybrid joined** (may take a few minutes)

---

## Screenshots to Capture (Step 04)

Minimum:
- `04-gpo-device-registration-enabled.png`
  - GPO setting showing **Enabled**
- `04-win11-gpresult-applied-gpo.png`
  - `gpresult /r` showing the GPO is applied (Computer Settings)
- `04-win11-dsregcmd-status.png`
  - `dsregcmd /status` output (DomainJoined + registration info)

Optional:
- `04-entra-device-hybrid-joined.png`
  - Entra device record showing **Microsoft Entra hybrid joined**

---

## Done Criteria (to move to Step 05)
- GPO is linked and confirmed applied on the Win11 VM
- `dsregcmd /status` shows hybrid registration progress/signals
- Step 04 screenshots captured
