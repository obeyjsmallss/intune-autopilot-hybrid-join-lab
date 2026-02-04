# 05 - Intune Connector for Active Directory (ODJ)

## Goal
Install and validate the **Intune Connector for Active Directory** so Intune can generate **Offline Domain Join (ODJ)** blobs for Hybrid Autopilot.

## Why this matters
Hybrid Autopilot cannot join a device to on-prem AD directly from the cloud.
Instead, Intune requests an **ODJ blob**, and the Connector (running on-prem) creates:
- the computer object in AD
- the ODJ package used during Autopilot to join the domain

---

## Prereqs
- Domain Controller reachable: `Lab-DC01` (`10.0.0.10`)
- Entra Connect is configured (SCP completed)
- You have Intune admin access
- Server to install connector on:
  - Lab best practice: **member server**
  - Lab acceptable: install on **Lab-DC01** (not recommended in production)

---

## Install / Configure

### A) Download the Connector
In the Intune admin center:
- Devices → Windows → Windows enrollment → **Intune Connector for Active Directory**
- Download the connector installer

### B) Install on the server
- Run the installer as admin
- When prompted, sign in with an account that can register the connector in Intune

### C) Verify the connector is Active
Back in Intune admin center:
- Devices → Windows → Windows enrollment → **Intune Connector for Active Directory**
- Confirm the connector shows **Active**

---

## Validation
- Connector status in Intune shows **Active**
- Connector service exists and is running on the server

---

## Screenshots to Capture (Step 05)

Minimum:
- `05-intune-connector-download-page.png`
  - Intune page showing where the connector is downloaded
- `05-intune-connector-active.png`
  - Intune page showing connector status **Active**
- `05-server-services-connector-running.png`
  - Services.msc showing the connector service running (or equivalent)

Optional:
- `05-server-programs-connector-installed.png`
  - Installed programs list showing the connector

---

## Done Criteria (to move to Step 06)
- Connector is installed
- Connector shows **Active** in Intune
- Step 05 screenshots captured
