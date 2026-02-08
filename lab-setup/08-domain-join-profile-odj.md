# 08 - Domain Join Configuration Profile (Offline Domain Join)

## Goal
Create and assign a **Domain Join configuration profile** so Intune can use the **Intune Connector for Active Directory** to generate **Offline Domain Join (ODJ)** blobs during Hybrid Autopilot provisioning.

## Why this matters
Hybrid Autopilot cannot join devices to on-prem Active Directory directly.
Instead, Intune requests an **ODJ blob**, and the Intune Connector:
- creates the computer object in Active Directory
- generates the domain join package
- applies the domain join during Autopilot OOBE

Without this profile, Hybrid Autopilot will fail even if all other steps are correct.

---

## Prereqs
- Steps 01–07 completed
- Intune Connector for Active Directory shows **Active**
- Autopilot deployment profile assigned to the device
- Target OU exists in AD

---

## What was done
- Verified the OU structure for Autopilot computer objects exists in AD
- Created a Domain Join configuration profile in Intune
- Configured the domain and target OU for computer account creation
- Assigned the profile to the Autopilot device group

---

## Configuration Details

### Target OU (AD)
OU structure:
- `Lab Devices`
  - `Autopilot Devices`

Distinguished Name (DN):
- `OU=Autopilot Devices,OU=Lab Devices,DC=jmcnairtech,DC=local`

### Domain Join Profile (Intune)
- Profile type: Domain Join
- Domain name: `jmcnairtech.local`
- Computer name prefix: `AP-`
- Target OU: `OU=Autopilot Devices,OU=Lab Devices,DC=jmcnairtech,DC=local`
- Assigned to: `GRP-Autopilot-Hybrid-Join-Lab`

---

## Validation
- Domain Join profile created successfully
- Profile assigned to the Autopilot device group
- No assignment errors reported

---

## Screenshots to Capture (Step 08)

Minimum:
- `08-domain-join-profile-created.png`
- `08-domain-join-profile-settings.png`
- `08-domain-join-profile-assigned.png`

Optional:
- `08-ad-ou-autopilot-devices.png`
- `08-domain-join-profile-device-status.png`

---

## Notes
- The Intune Connector is only used during Autopilot OOBE.
- Computer objects are created at provisioning time, not when the profile is assigned.
- Device reset is required to validate the domain join during Autopilot.

---

## Done Criteria (to move to Step 09)
- Domain Join profile created
- Profile assigned to the Autopilot device group
- No assignment errors
