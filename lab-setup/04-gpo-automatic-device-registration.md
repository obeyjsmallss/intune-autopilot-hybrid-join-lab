# 04 - GPO Automatic Device Registration (Hybrid Join)

## Goal
Enable automatic device registration so **domain-joined Windows devices** register into **Microsoft Entra ID** as **Microsoft Entra hybrid joined**.

## Why this matters
- Step 02 (SCP) tells Windows **which tenant** to register with.
- Step 04 (GPO) tells Windows **to actually register**.
Together, they enable Hybrid Join.

---

## What I configured (high level)
- Created and linked a GPO to enable **automatic device registration**
- Applied policy to the Windows 11 VM
- Validated registration using `dsregcmd /status`

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

---

## Validation (Windows 11 VM)
Run:
- `gpupdate /force`
- reboot

Then run:
- `dsregcmd /status`

Expected signals:
- `DomainJoined : YES`
- Device begins/finishes registration to Entra (DeviceId present)
- Device appears in Entra as **Microsoft Entra hybrid joined** (may take a few minutes)

---

## Screenshots to Capture (Step 04)

Minimum:
- `04-gpo-device-registration-enabled.png`
  - GPO setting showing **Enabled**
- `04-win11-dsregcmd-status.png`
  - `dsregcmd /status` showing DomainJoined and registration details

Optional (strong proof):
- `04-gpresult-applied-gpo.png`
  - `gpresult /r` showing the GPO is applied to the computer

---

## Done Criteria (to move to Step 05)
- GPO is linked and applied to the Win11 VM
- Win11 VM shows registration evidence in `dsregcmd /status`
- Step 04 screenshots captured
