# 03 - GPO Device Registration (Hybrid Join)

## Goal
Enable automatic device registration so domain-joined Windows devices can become **Microsoft Entra hybrid joined**.

## Why this matters
After Step 02 (SCP), devices can discover the tenant.
This step enables Windows to automatically register the device in Entra (Hybrid Join) after domain join.

---

## What I configured (high level)
- Created and linked a GPO to enable **automatic device registration**
- Forced policy update on the Windows 11 VM
- Validated scheduled tasks / dsregcmd status

---

## Configuration (GPO)

### 1) Create a GPO
Name:
- `GPO - Entra Hybrid Join (Auto Device Registration)`

### 2) Link the GPO
Recommended: link at the **domain level** (`jmcnairtech.local`)
- This ensures all domain-joined devices get the setting (lab-friendly)

(If you want to scope it later, link to `OU=Lab Devices` instead.)

### 3) Enable auto-registration
In the GPO, set:

**Computer Configuration**
→ Policies
→ Administrative Templates
→ Windows Components
→ Device Registration

- **Register domain-joined computers as devices** = **Enabled**

---

## Validation

### On the Windows 11 VM
Run:
- `gpupdate /force`
- Reboot the VM

Then check:
- `dsregcmd /status`

Expected (eventually):
- `AzureAdJoined : NO`
- `DomainJoined : YES`
- `DeviceId` present
- Hybrid join may take a few minutes after reboot + network is stable

Optional checks:
- Task Scheduler → Microsoft → Windows → Workplace Join → look for registration activity

---

## Screenshots to Capture (Step 03)

Minimum:
- `03-gpo-device-registration-setting.png`
  - Screenshot of the GPO setting **Enabled**
- `03-win11-dsregcmd-status.png`
  - Screenshot of `dsregcmd /status` output (DomainJoined = YES)

Optional:
- `03-gpo-linked-scope.png`
  - Screenshot showing the GPO is linked to the domain or Lab Devices OU

---

## Done Criteria (to move to Step 04)
- GPO is linked and applied
- Win11 VM shows policy applied and `dsregcmd /status` indicates the device is registering/registered (DomainJoined = YES at minimum)
- Step 03 screenshots captured
