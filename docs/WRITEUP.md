# AD + Intune Mini-Tenant Lab

**Author:** Shadman Bari · [shadman.io](https://shadman.io) · [linkedin.com/in/shadman-bari](https://linkedin.com/in/shadman-bari)  
**Stack:** Proxmox · Windows Server (lab AD) · Windows 11 endpoint · Microsoft Entra ID · Microsoft Intune  
**Status:** In progress — walkthrough sections below are the target narrative; replace placeholders and add screenshots after Phase 1.  
**Companion lab:** [linux-homelab](https://github.com/SudoShad/linux-homelab) (Proxmox, AD/GPO, WireGuard, osTicket, Wazuh)

> Screenshots in `/docs/screenshots/` are redacted (no tenant IDs, usernames, or public IPs). This is a personal practice lab — not a customer environment.

---

## Problem

IT Support and junior systems roles in NYC (deskside, managed service, small corporate IT) expect you to speak **both** languages of endpoint identity:

1. **On-prem:** Active Directory join, OU placement, basic GPO  
2. **Cloud:** Entra ID device join, Intune enrollment, compliance, and app deployment  

My existing homelab already covered Proxmox and AD/GPO. The gap was a **documented Intune path** with proof (device object, compliance result, deployed app) — the same loop you’d touch on day one of a modern desktop / jr sysadmin seat.

---

## What this lab builds (target — complete after Phase 1)

A small **endpoint-management mini-tenant** on top of my Proxmox lab:

- Microsoft 365 **Developer** tenant for Entra ID + Intune  
- Windows 11 VM enrolled as an Entra-joined / Intune-managed device  
- **Compliance policy** evaluating antivirus and firewall posture  
- **Company Portal** plus one additional app assignment  
- Written run notes for sync delays and the fixes (the part interviews actually probe)  
- Optional stretch: same lab’s AD domain join documented alongside Intune for a dual-stack story  

---

## Architecture

```
┌──────────────────────────────────────────────┐
│ Proxmox lab                                 │
│  • DC01 — Windows Server (lab AD / GPO)     │
│  • WIN11-ENDPOINT — Intune-managed client   │
│  • WireGuard path for remote lab access     │
└──────────────────┬───────────────────────────┘
                   │ HTTPS management
                   ▼
┌──────────────────────────────────────────────┐
│ Microsoft 365 Developer tenant              │
│  • Entra ID — users, groups, device objects │
│  • Intune — compliance, apps, sync          │
└──────────────────────────────────────────────┘
```

**Design choice:** Ship **Entra + Intune first** (Path A) so the portfolio has cloud endpoint proof quickly. AD coexistence (Path B) is documented as an extension, not a blocker — same way many shops run cloud-managed devices beside classic domain clients.

---

## Tools

| Layer | Tool |
|-------|------|
| Hypervisor | Proxmox VE |
| Endpoint OS | Windows 11 (lab VM) |
| On-prem identity | Windows Server AD (existing lab DC) |
| Cloud identity | Microsoft Entra ID |
| MDM / endpoint | Microsoft Intune |
| Proof commands | `dsregcmd /status`, Company Portal sync |
| Docs | Markdown + redacted screenshots |

---

## Walkthrough

> These steps are the **target procedure**. Tense reads as completed once you finish Phase 1 and drop screenshots; until then treat as the checklist to execute.


### 1. Tenant and licensing
- Enrolled in the Microsoft 365 Developer Program and confirmed Intune access in the admin center.  
- Created a dedicated lab user and assigned the developer E5 / Intune-capable license **before** device join (avoids the classic “joined but not managed” dead end).

### 2. Device join and enrollment
- On `WIN11-ENDPOINT`: **Settings → Accounts → Access work or school → Connect → Join this device to Microsoft Entra ID**.  
- Verified with `dsregcmd /status` (`AzureAdJoined : YES`).  
- Confirmed the device object under **Intune → Devices**.

*(Screenshot pending: Entra join / dsregcmd — add under docs/screenshots/ after Phase 1.)*

*(Screenshot pending: Intune device blade — add under docs/screenshots/ after Phase 1.)*

### 3. Compliance policy
- Created a device compliance policy requiring:
  - Microsoft Defender antivirus real-time protection on  
  - Firewall enabled  
- Assigned to lab users / devices; triggered sync from Company Portal.  
- Recorded first evaluation result (Compliant or Noncompliant → remediations → recheck).

*(Screenshot pending: Compliance policy — add under docs/screenshots/ after Phase 1.)*

### 4. App deployment
- Assigned **Company Portal**.  
- Assigned one additional app (Store or Win32) and confirmed install on the endpoint.  

*(Screenshot pending: App assignment — add under docs/screenshots/ after Phase 1.)*

### 5. What broke (keep this section honest)

_| Fill after you hit a real issue — examples: policy stuck Pending for N hours; license not on user; TPM/BitLocker requirement you backed off; DNS when testing domain join. Interviewers trust labs that admit friction. |_

**Example placeholder:** Policy stayed “Not evaluated” until Company Portal sync + ~20 minutes. Fix: confirmed license on user, re-ran sync, waited — then Compliant. Documented the wait so future-me doesn’t thrash settings.

### 6. Optional — AD coexistence
- Domain-joined a lab Windows client to the existing AD DC (OU placement, basic GPO already practiced in [linux-homelab](https://github.com/SudoShad/linux-homelab)).  
- Short takeaway: **AD** still owns many on-prem identity and legacy app patterns; **Intune** owns modern device compliance and app delivery for cloud-first endpoints. Real deskside work often touches both in one week.

*(Screenshot pending: Domain join — add under docs/screenshots/ after Phase 1.)*

---

## What I’d do in production (junior-level honesty)

- Use **Autopilot / provisioning profiles** instead of hand-joining every device.  
- Scope compliance and apps with **groups**, not “all users,” and pilot before broad assign.  
- Never put real secrets or customer tenants in a public README — redact like this lab.  
- Pair Intune compliance with a ticket workflow (I practice intake in osTicket in the same homelab) so remediations have an audit trail.  
- Grow into Conditional Access and BitLocker recovery key escrow **after** the basics above are solid (and after SC-900 / MD-102 study).

---

## Skills demonstrated

- Entra ID device join and Intune enrollment  
- Compliance policy design and evaluation  
- Intune app assignment (Company Portal + Store/Win32)  
- Endpoint troubleshooting mindset (`dsregcmd`, sync, license checks)  
- Proxmox lab operations and disciplined documentation  
- Continuity with existing AD/GPO practice  

---

## Resume bullet (paste-ready)

> Built a Proxmox-hosted endpoint lab with **Microsoft Entra ID + Intune**: enrolled a Windows 11 VM, enforced Defender/firewall **compliance**, and deployed Company Portal plus a managed app; documented sync/license failure modes alongside existing **Active Directory** lab work.

*(Tighten with real app name and one metric after screenshots exist, e.g. “time-to-compliant ~25 min after sync.”)*

---

## LinkedIn / interview one-liner

> “I already run AD and GPO in Proxmox; this lab adds the Intune side — enroll, compliance, app deploy — so I can talk through the same loop a deskside or jr sysadmin hits on modern Windows fleets.”

---

## What’s next

1. Autopilot / ESP dry-run in the same tenant (separate short writeup).  
2. Graph-powered helpdesk scripts (BitLocker key lookup, stale device report) as a sibling repo.  
3. Surface this under **Labs** on shadman.io (IT support / sysadmin positioning — not SOC-first).  

---

## Connect

- Portfolio: [shadman.io](https://shadman.io)  
- Homelab: [linux-homelab](https://github.com/SudoShad/linux-homelab)  
- LinkedIn: [linkedin.com/in/shadman-bari](https://linkedin.com/in/shadman-bari)  
- Email: shadman@shadman.io  
