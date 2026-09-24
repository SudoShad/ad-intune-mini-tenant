# AD + Intune Mini-Tenant Lab

**Author:** Shadman Bari · [shadman.io](https://shadman.io) · [LinkedIn](https://linkedin.com/in/shadman-bari)  
**Status:** In progress (Phase 0–1) — docs and plan are live; Intune enrollment screenshots land after the lab VM is enrolled.  
**Companion:** [linux-homelab](https://github.com/SudoShad/linux-homelab) (Proxmox, AD/GPO, WireGuard, osTicket, Wazuh)

Personal practice lab that adds **Microsoft Entra ID + Intune** endpoint management on top of an existing Proxmox + Active Directory homelab. Built for Desktop Support / IT Support / Junior Systems Engineer interviews (Queens–NYC + remote).

> No customer data. Screenshots will be redacted (no tenant IDs, secrets, or public IPs).

---

## Problem

Deskside and junior sysadmin roles expect both:

1. **On-prem:** AD join, OUs, basic GPO (already covered in [linux-homelab](https://github.com/SudoShad/linux-homelab))
2. **Cloud:** Entra device join, Intune enrollment, compliance, app deploy

This repo closes the Intune gap with a small, documented mini-tenant — not a rebuild of the AD lab.

---

## Target architecture

```
Proxmox lab (existing)
├── DC01              Windows Server — lab AD / GPO
├── WIN11-ENDPOINT    Windows 11 VM — Intune target
└── WireGuard         Remote access to lab

Microsoft 365 Developer tenant
├── Entra ID
└── Intune — compliance policy + Company Portal + one app
```

**Path A (shipping first):** Entra-join + Intune enroll + compliance + apps.  
**Path B (optional):** Document AD domain-join coexistence after Path A is proven.

---

## Repo map

| Path | Purpose |
|------|---------|
| [docs/PROJECT.md](docs/PROJECT.md) | Scope, phases, RACI, definition of done |
| [docs/RUNBOOK.md](docs/RUNBOOK.md) | Operator click-path (Phase 0→2) |
| [docs/WRITEUP.md](docs/WRITEUP.md) | Full portfolio narrative (becomes the polished story as screenshots land) |
| [docs/screenshots/](docs/screenshots/) | Redacted evidence (added during Phase 1) |

---

## Definition of done

- [ ] M365 Developer tenant + Intune admin access
- [ ] Windows 11 lab VM visible in Intune
- [ ] One compliance policy evaluated (Compliant, or Noncompliant → fix → Compliant with notes)
- [ ] Company Portal + one additional app assigned and installed
- [ ] Redacted screenshots in `docs/screenshots/`
- [ ] Resume bullet updated with real app name / outcome
- [ ] Cross-link from `linux-homelab` README

---

## Resume bullet (draft — finalize after Phase 1)

> Building a Proxmox-hosted endpoint lab with **Microsoft Entra ID + Intune**: enroll a Windows 11 VM, enforce Defender/firewall **compliance**, and deploy Company Portal plus a managed app; documented alongside existing **Active Directory** lab work.

---

## Skills this lab is meant to demonstrate

- Entra ID device join and Intune enrollment  
- Compliance policy design and evaluation  
- Intune app assignment  
- Endpoint troubleshooting (`dsregcmd /status`, sync, license checks)  
- Continuity with existing AD/GPO + Proxmox practice  

---

## Connect

- Portfolio: [shadman.io](https://shadman.io)  
- Homelab: [linux-homelab](https://github.com/SudoShad/linux-homelab)  
- Ops toolkit: [linux-ops-toolkit](https://github.com/SudoShad/linux-ops-toolkit)  
- Email: shadman@shadman.io  
