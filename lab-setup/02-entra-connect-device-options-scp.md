# 02 - Entra Connect Device Options + SCP

## Goal
Enable **Microsoft Entra hybrid join** discovery by configuring **Entra Connect device options** and creating the **Service Connection Point (SCP)** in Active Directory.

## Why this matters
Hybrid-joined devices use the **SCP** in AD to discover which Entra tenant to register with. Without it, hybrid join often fails later.

---

## What I configured (high level)
- Ran **Entra Connect** “Device options”
- Enabled **Microsoft Entra hybrid join** for **Windows 10/11**
- Target forest: `jmcnairtech.local`
- Created/updated the **SCP** in AD for device registration

> Enterprise Admin credentials are required because the wizard writes the SCP object into AD.

---

## Validation
- Entra Connect device options completed successfully (no errors)
- SCP configuration applied to the `jmcnairtech.local` forest

(Optional)
- Verified SCP exists in AD via ADSI Edit (Configuration partition)

---

## Screenshots to Capture (Step 02)

Minimum:
- `02-entra-connect-device-options.png`
  - Entra Connect “Additional tasks” showing **Configure device options**
- `02-entra-connect-scp-configuration.png`
  - SCP configuration showing forest `jmcnairtech.local` and Authentication Service = **Microsoft Entra ID**
- `02-entra-connect-complete.png`
  - Success/completion screen

Optional:
- `02-ad-scp-object.png`
  - ADSI Edit showing SCP object exists in AD

---

## Done Criteria (to move to Step 03)
- Entra Connect device options completed successfully
- SCP configured for `jmcnairtech.local`
- Step 02 screenshots captured
