# 09 - Enrollment Status Page (ESP) Configuration

## Goal
Configure the **Enrollment Status Page (ESP)** to allow Hybrid Autopilot provisioning to complete without unnecessary timeouts or blocking behavior.

## Why this matters
Hybrid Autopilot includes additional steps (ODJ blob processing, domain join, hybrid registration timing) that can take longer than cloud-only provisioning.
If ESP is too strict (blocking on apps/policies), deployments often fail even when the configuration is correct.

This step tunes ESP to:
- reduce provisioning failures
- avoid blocking on non-critical apps
- allow the user to reach the desktop while background processes complete

---

## Prereqs
- Steps 01–08 completed
- Autopilot deployment profile assigned
- Domain Join (ODJ) profile assigned
- Test device registered as an Autopilot device

---

## What was done
- Created/updated an Enrollment Status Page (ESP) configuration
- Configured ESP to avoid blocking on all apps and long-running steps
- Assigned ESP to the Autopilot device group

---

## ESP Configuration (Lab-Friendly)
Recommended for Hybrid Autopilot labs:
- Show progress during device setup: Enabled
- Show progress during user setup: Enabled (optional)
- Block device use until required apps and profiles are installed: Disabled (preferred for labs)
- Do not enforce “install all apps” requirements during ESP

---

## Validation
- ESP profile exists in Intune
- ESP assigned to `GRP-Autopilot-Hybrid-Join-Lab`
- Device is ready to be reset to OOBE for end-to-end testing

---

## Screenshots to Capture (Step 09)

Minimum:
- `09-esp-profile-created.png`
- `09-esp-settings.png`
- `09-esp-assigned.png`

---

## Notes
- Strict ESP configurations are a common cause of Hybrid Autopilot failures in labs.
- ESP will be validated during the Step 10 Autopilot run.

---

## Done Criteria (to move to Step 10)
- ESP configuration created/updated
- ESP assigned to the Autopilot device group
- Ready to reset the VM and run Autopilot OOBE
