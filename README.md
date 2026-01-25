# Intune Autopilot + Microsoft Entra Hybrid Join Lab (Hyper-V)

This repo documents a hands-on lab that simulates an enterprise Windows provisioning workflow using **Microsoft Intune**, **Windows Autopilot**, and **Microsoft Entra hybrid join** (Hybrid Azure AD Join) — validated using a **Windows 11 Hyper-V VM**.

The goal is to demonstrate understanding of hybrid provisioning dependencies (SCP, ODJ, GPO registration), policy enforcement (ESP, compliance, Conditional Access), and validation across **AD / Entra ID / Intune**.

---

## What This Project Demonstrates
- Hybrid identity setup using **Microsoft Entra Connect** (AAD Connect)
- **Service Connection Point (SCP)** configuration for hybrid registration discovery
- **Offline Domain Join (ODJ)** via the Intune Connector for Active Directory
- Device registration via **GPO** (hybrid join enablement)
- Autopilot provisioning flow simulation (OOBE on a Hyper-V VM)
- Enrollment Status Page (ESP)
- Intune configuration + compliance policies
- Conditional Access requiring compliant devices
- Win32 app packaging and deployment
- Troubleshooting + validation methods for hybrid provisioning

> Note: Autopilot device registration via hardware hash is designed for physical devices. This lab is validated in a Hyper-V VM environment; hardware hash import behavior can vary depending on VM identifiers and configuration.

---

## Architecture Overview
**Workflow (lab simulation on Hyper-V):**  
Windows 11 VM reset → OOBE → Autopilot provisioning simulation → Offline Domain Join (ODJ) → **Microsoft Entra hybrid joined** → Intune enrollment → Compliance → Conditional Access → App deployment

Diagrams are stored in: `/architecture-diagrams`


---

## Repo Notes
- Lab prerequisites and environment: `lab-setup/00-prereqs-and-lab-environment.md`
- Screenshot index + naming: `screenshots/README.md`

---

## Blog Post
A full write-up with screenshots and explanations will be published on **jmcnairtech.com**:  
https://jmcnairtech.com
