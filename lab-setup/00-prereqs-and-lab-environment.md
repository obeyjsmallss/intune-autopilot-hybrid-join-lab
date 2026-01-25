# 00 - Prereqs and Lab Environment

## Goal
Define the lab requirements, environment, naming conventions, and success criteria for an **Intune Autopilot + Microsoft Entra Hybrid Join** build.

This lab simulates an enterprise provisioning flow:
**Autopilot → Offline Domain Join (ODJ) → Entra hybrid registration → Intune enrollment → Compliance/Conditional Access → App deployment**

---

## Requirements

### Accounts & Permissions
- **Microsoft Entra tenant** with Intune available
- Account with permissions to:
  - Configure Intune (Intune Admin or Global Admin)
  - Configure Conditional Access (typically requires Entra ID P1)
  - Configure Entra Connect (AD DS Enterprise/Domain Admin + Entra admin)

### Licensing (lab-friendly)
Minimum workable setup typically includes:
- Microsoft Intune license for test users
- Microsoft Entra ID P1 (if you want Conditional Access)

> Licensing varies by tenant. This lab focuses on configuration and validation.

### Hardware / VM Requirements

**Domain Controller (DC)**
- Windows Server 2022 (or 2019)
- AD DS + DNS
- Static IP
- 2 vCPU / 4–8 GB RAM / 60GB disk

**Client device for provisioning tests**
- Windows 10/11 (Windows 11 recommended)
- Option A: Physical device (best for true Autopilot registration)
- Option B: **Hyper-V Windows 11 VM** (validated client in this lab; hardware hash import may vary)

**Optional member server**
- Windows Server (domain-joined) to host the **Intune Connector for Active Directory (ODJ)**
  - In a lab, the connector can be installed on the DC, but a member server is closer to best practice.

---

## Lab Environment Overview (High-Level)

### On-Prem AD Domain (Baseline)
- Domain: `jmcnairtech.local`
- DC hostname: `Lab-DC01`
- DC roles: AD DS, DNS
- DC IP: `10.0.0.10`

> **Directory object build-out (OUs, users, groups) is documented in Step 01:**  
> `lab-setup/01-onprem-ad-dns-ou-structure.md`

---

## Identity Prereq: Entra-Friendly UPN Suffix (Recommended)
Autopilot sign-in is cloud-first. If users are `@jmcnairtech.local`, sign-in and enrollment can be inconsistent.

**Recommendation:** set test users’ UPN to a domain that exists in your Entra tenant:
- Preferred: verified custom domain (example: `@jmcnairtech.com`)
- Lab fallback: tenant domain `@<tenant>.onmicrosoft.com`

**Action**
1. Open **Active Directory Domains and Trusts**
2. Right-click the top node → **Properties**
3. Add an **Alternative UPN suffix**
4. In **ADUC**, edit each test user → **Account** tab → set the new UPN suffix

---

## Network & DNS Assumptions (Critical)

### Lab Networking (Hyper-V)
- Lab subnet: **10.0.0.0/24**
- Hyper-V **Internal vSwitch** with the host providing **NAT (NetNat)** for internet access
- Host gateway on the lab network: **10.0.0.1**
- **DHCP is provided by `Lab-DC01`** for the lab subnet

### DNS
- Clients must use the **Domain Controller for DNS**: **10.0.0.10**
- DC uses a DNS forwarder for internet name resolution (example: `8.8.8.8`)

### Time
- Time sync must be correct (Kerberos + device registration depend on time).

---

## Naming Conventions

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
   - Computer object exists in the correct OU (documented in Step 01)

2. **Microsoft Entra ID**
   - Device shows as **Microsoft Entra hybrid joined**

3. **Microsoft Intune**
   - Device is managed and receives policies/apps
   - Device becomes compliant (if compliance is configured)

---

## Screenshots to Capture (Step 00)
Save these in `/screenshots` using the naming format:
`<step#>-<component>-<short-description>.png`

**Minimum for this step**
- `00-ad-upn-suffix-added.png` (Domains & Trusts showing the alternate UPN suffix)
- `00-ad-test-user-upn-set.png` (User account tab showing UPN set to the new suffix)
- `00-dc-ip-dns-settings.png` (DC IPv4 showing static IP + gateway + DNS)
- `00-dhcp-scope-options.png` (DHCP options: Router 10.0.0.1 + DNS 10.0.0.10 + domain)
- `00-win11-dhcp-lease.png` (Win11 `ipconfig` showing DHCP lease + DNS/DC)

**Optional**
- `00-dc-hostname-lab-dc01.png` (proof of DC hostname)
- `00-dc-dns-forwarders.png` (DNS forwarder configured, e.g., 8.8.8.8)
- `00-host-netnat-labnat.png` (host `Get-NetNat` showing NAT for 10.0.0.0/24)
- `00-lab-topology-note.png` (quick diagram or Hyper-V VM list)

---

## Done Criteria (to move to Step 01)
You’re ready to proceed when:
- UPN suffix is available and at least one test user is set to `@jmcnairtech.com`
- DC has static IP, DNS points to the DC, and time is correct
- DHCP is issuing leases to the Windows 11 VM on `10.0.0.0/24`
- Step 00 screenshots are captured

---

## Notes / Scope
This project documents configuration for a realistic hybrid onboarding flow.  
Next steps include: On-prem AD OU/user structure (Step 01), Entra Connect (SCP), Intune ODJ connector, GPO device registration, Autopilot profiles, ESP, compliance/CA, Win32 apps, and troubleshooting.
