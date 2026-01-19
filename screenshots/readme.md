# Screenshots

This folder contains screenshots that provide evidence of configuration and validation for the **Intune Autopilot + Microsoft Entra hybrid join** lab.

## How screenshots are organized

Screenshots are named to match the step-by-step guides in `/lab-setup`.

**Naming format**
`<step#>-<component>-<short-description>.png`

Examples:
- `01-ad-ou-lab-users.png`
- `02-entra-connect-device-options.png`
- `02-entra-connect-scp-configured.png`
- `04-intune-connector-installed.png`
- `06-autopilot-profile-assigned.png`
- `07-domain-join-profile-settings.png`
- `09-conditional-access-require-compliant.png`

## What to capture (minimum set)

### Identity + Hybrid Join
- Entra Connect “Device options” completed
- SCP created (or confirmation screen)
- GPO enabling automatic device registration
- Device successfully shows **Microsoft Entra hybrid joined** (post-enrollment)

### Autopilot + Intune
- Hardware hash import success / device listed
- Autopilot profile assignment
- Domain Join profile assignment
- Enrollment Status Page (ESP) behavior
- Device appears in Intune with compliance state

### Security + Access
- Compliance policy settings
- Conditional Access policy requiring compliant device
- Sign-in success/failure proving CA is working

## Redaction guidance
Before uploading screenshots:
- Blur user UPNs, tenant IDs, device serial numbers, public IPs, and any secrets.
- It’s okay to leave **domain name (jmcnairtech.local)** and generic OU names.

## Quick index
- Step 01 (AD): `01-*`
- Step 02 (Entra Connect + SCP): `02-*`
- Step 03 (GPO registration): `03-*`
- Step 04 (Intune Connector): `04-*`
- Step 05–07 (Autopilot + Domain Join): `05-*` to `07-*`
- Step 08–10 (ESP/Compliance/CA/Apps): `08-*` to `10-*`
- Step 11 (Troubleshooting): `11-*`

