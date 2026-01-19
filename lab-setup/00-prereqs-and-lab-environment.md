# 00 - Prereqs and Lab Environment

## Goal
Define the lab requirements, environment, naming conventions, and success criteria for an **Intune Autopilot + Hybrid Entra Join** build.
This lab simulates enterprise provisioning: Autopilot → Offline Domain Join (ODJ) → Hybrid Entra registration → Intune enrollment → compliance/CA → app deployment.

---

## Requirements (What you need before starting)

### Accounts & Permissions
- **Microsoft Entra tenant** with Intune available
- Account with permissions to:
  - Configure Intune (Intune Admin or Global Admin)
  - Configure Conditional Access (typically requires Entra ID P1 + access)
  - Configure Entra Connect (AD DS Enterprise/Domain Admin + Entra admin)

### Licensing (lab-friendly guidance)
Minimum workable setup typically includes:
- Microsoft Intune license for test users
- Microsoft Entra ID P1 (needed if you want Conditional Access policies)

> Note: exact licensing depends on your tenant plan. This lab focuses on configuration, not licensing deep dive.

### Hardware / VM Requirements
**Domain Controller (DC)**
- Windows Server 2022 (or 2019)
- AD DS + DNS
- Static IP
- 2 vCPU / 4–8 GB RAM / 60GB disk

**Client device for Autopilot testing**
- Windows 10/11 (Windows 11 recommended)
- Option A: Physical device (best for real Autopilot)
- Option B: VM (works for documentation/testing, but Autopilot hash can be tricky)

**Optional member server**
- Windows Server (domain-joined) to host the **Intune Connector for AD (ODJ)**
  - In a lab, the connector can be installed on the DC, but a member server is closer to best practice.

---

## Lab Environment Overview

### On-Prem AD Domain
- Domain: `jmcnairtech.local`
- DC hostname: `DC01` (or your chosen name)
- DC roles: AD DS, DNS

### OU Structure (Recommended)
**Users**
- `OU=Lab Users,DC=jmcnairtech,DC=local`

**Devices**
- `OU=Lab Devices,DC=jmcnairtech,DC=local`
  - `OU=Autopilot Devices,OU=Lab Devices,DC=jmcnairtech,DC=local`

### Test Users
Create at least 2 standard users for enrollment tests:
- `testuser1`
- `testuser2`
  
 ### Identity Prereq (Recommended): UPN suffix for cloud sign-in
Autopilot sign-in is cloud-first. If your users are `@jmcnairtech.local`, sign-in and enrollment can be inconsistent.

**Recommendation:** set test users’ UPN to a domain that exists in your Entra tenant:
- Preferred: your verified custom domain (ex: `@jmcnairtech.com`)
- Lab fallback: your tenant `@<tenant>.onmicrosoft.com`

**Action:**
1. Open **Active Directory Domains and Trusts**
2. Right-click the top node → **Properties**
3. Add an **Alternative UPN suffix**
4. In **ADUC**, edit each test user → **Account** tab → set the new UPN suffix


---

## Network & DNS Assumptions (Critical)
- Client devices must use the **Domain Controller for DNS**
  - If the client uses public DNS (8.8.8.8), domain join + hybrid registration will fail.
- Time sync must be correct (Kerberos + registration depend on time)

---

## Naming Conventions (Keep it consistent)

### Device naming (example)
- Autopilot naming template: `HYB-%SERIAL%` (or similar)

### Autopilot Group Tag (optional but recommended)
- Group Tag: `HYBRID-AP`

### Intune Groups
- `GRP-Autopilot-Hybrid-Devices` (dynamic devices)
- `GRP-Autopilot-Test-Users` (assigned users)

### Profiles / Policies
- Autopilot profile: `AP-Hybrid-UserDriven`
- Domain Join profile: `DJ-ODJ-AutopilotDevicesOU`
- ESP: `ESP-Autopilot-Hybrid`
- Compliance: `CP-Win11-Baseline`
- Conditional Access: `CA-Require-Compliant-Device`

---

## Validation Checklist (End of Lab)
A successful run means the provisioned device appears in all 3 locations:

1. **Active Directory**
   - Computer object exists in:
     `OU=Autopilot Devices,OU=Lab Devices,DC=jmcnairtech,DC=local`

2. **Microsoft Entra ID**
   - Device shows as **Microsoft Entra hybrid joined**

3. **Microsoft Intune**
   - Device is managed and receives policies/apps
   - Device becomes compliant (if compliance is configured)

---

## Screenshots to Capture (Step 00)
Save these in `/screenshots` using a consistent naming scheme.

Minimum for this step:
- `00-aduc-ou-structure.png` (ADUC showing domain + Lab Users + Lab Devices/Autopilot Devices)
- `00-dc-ip-dns-settings.png` (DC network config showing static IP + DNS)
- `00-test-users-in-lab-users.png` (ADUC showing test users inside Lab Users OU)
- `00-upn-suffix-added.png` (Domains & Trusts showing the alternate UPN suffix)
- `00-test-user-upn-set.png` (User account tab showing UPN set to the new suffix)
- `00-lab-topology-note.png` (optional)

---
## Done Criteria (to move to Step 01)
You’re ready to proceed when:
- `Lab Users`, `Lab Devices`, and `Autopilot Devices` OUs exist
- At least 2 test users exist in `Lab Users`
- Test users have an Entra-friendly UPN suffix (custom domain or `*.onmicrosoft.com`)
- DC has static IP, DNS points to the DC, and time is correct
- Step 00 screenshots are captured

## Notes / Scope
- This project documents configuration for a realistic hybrid onboarding flow.
- Later steps will include: Entra Connect (SCP), Intune ODJ connector, GPO device registration, Autopilot profiles, ESP, compliance/CA, Win32 apps, and troubleshooting.
