# Project: Intune on Existing Proxmox AD Lab

**Codename:** AD + Intune Mini-Tenant  
**Owner:** Shadman Bari  
**Engineering lead:** Sin (senior)  
**Target roles:** Desktop Support / IT Support / Junior Systems Engineer (Queens–JFK + remote)  
**Status:** Ready to execute — Phase 0  
**Public repo:** [SudoShad/ad-intune-mini-tenant](https://github.com/SudoShad/ad-intune-mini-tenant)  
**Feeds from:** existing [linux-homelab](https://github.com/SudoShad/linux-homelab) (Proxmox + AD DC already documented)  
**Estimated time:** 4–6 focused evenings (faster because AD/Proxmox already exist)

---

## 1. Senior decision: what we are building

You already run **Proxmox**, a **Windows Server AD DC**, GPO practice, WireGuard, osTicket, and Wazuh. Rebuilding AD would waste time.

**This project’s job:** close the gap hiring managers still see — **Microsoft Intune / Entra endpoint management** — and publish one clean public writeup that ties AD + Intune into one “endpoint identity” story.

| Keep (already have) | Build now | Explicitly skip |
|---------------------|-----------|-----------------|
| Proxmox VMs | M365 Developer tenant | Full Autopilot production |
| Lab AD DC + GPO | Entra join / Intune enroll of a Windows client | Azure AD Connect hybrid at scale |
| WireGuard access to lab | Compliance policy + app deploy | SCCM / ConfigMgr |
| linux-homelab README | New focused repo + screenshots | Leading with SOC/Wazuh on this page |

**Hiring signal:** “I domain-join and GPO in the lab **and** I can enroll a device in Intune, prove compliance, and deploy an app.” That sentence is what Atlas / CCS / deskside screens for.

---

## 2. Definition of done

Ship when all are true:

1. M365 Developer tenant is live; Intune admin center reachable.
2. At least one Windows 10/11 VM from your Proxmox lab is **visible in Intune** (Entra-joined or dual-home — Path A below).
3. **One compliance policy** assigned and showing Compliant (or a documented Noncompliant → remediations → Compliant story).
4. **Company Portal** plus **one additional app** (Store or Win32) assigned and installable on that device.
5. Public repo README = polished WRITEUP.md with redacted screenshots.
6. Cross-link from `linux-homelab` README “Next steps” → this repo.
7. One paste-ready resume bullet for Desktop / IT Support.

Optional stretch (Phase 2b, only after 1–6): same client also domain-joined to lab AD; short note on “AD for identity on-prem, Intune for cloud endpoint policy.”

---

## 3. Architecture

```
Proxmox lab (existing)
├── DC01              Windows Server — lab AD (keep as-is)
├── WIN11-ENDPOINT    New or reused Windows VM  ← primary Intune target
└── (optional)        Second VM for “broke it / fixed it” demos

Internet
└── Microsoft 365 Developer tenant
    ├── Entra ID (users, groups, device objects)
    └── Intune
        ├── Compliance policy (Defender + firewall minimum)
        ├── Company Portal
        └── One Store/Win32 app
```

**Path A (primary — ship this week):** Entra-join WIN11-ENDPOINT → Intune enroll → compliance + apps. Fastest portfolio value.  
**Path B (stretch):** Domain-join the same or second VM to lab AD; document coexistence. Do not block publishing on Path B.

---

## 4. Phases

### Phase 0 — Kickoff (today / Night 1, ~45–90 min)
- [ ] Confirm M365 Developer Program signup → tenant admin works
- [ ] Confirm Intune blade loads (Endpoint Manager)
- [ ] On Proxmox: clone or create WIN11-ENDPOINT (4–8 GB RAM); snapshot `clean`
- [x] GitHub repo `ad-intune-mini-tenant` created; docs live under `docs/` (keep this checklist current)
- [ ] Reply to Sin: Dev tenant `ready` / `blocked`, Proxmox VM name, Windows 10 or 11

### Phase 1 — Entra + Intune (Nights 1–2) ← must publish
- [ ] Create `lab.endpoint@<tenant>.onmicrosoft.com` (or similar); license with Dev E5 pack
- [ ] On WIN11: Access work or school → join Entra ID as that user
- [ ] Verify: `dsregcmd /status` shows AzureAdJoined : YES
- [ ] Device appears under Intune → Devices
- [ ] Compliance policy: require antivirus real-time + firewall; assign; Company Portal Sync
- [ ] Deploy Company Portal + one app; screenshot installed
- [ ] Capture one failure story (sync delay, wrong license, TPM/BitLocker if you touch it)

### Phase 2 — Docs + resume (Night 3)
- [ ] Redact screenshots into `/docs/screenshots/`
- [ ] Finalize WRITEUP.md as README
- [ ] Add resume bullet; link from linux-homelab and (later) shadman.io Labs

### Phase 2b — AD coexistence (optional Night 4–5)
- [ ] Domain-join WIN11 (or second VM) to lab AD
- [ ] Screenshot System Properties + `whoami`
- [ ] One paragraph in writeup: when you’d use AD vs Intune in a real shop

---

## 5. How I manage this (RACI)

| Activity | You | Sin |
|----------|-----|-----|
| Proxmox / Windows clicks, screenshots | **Do** | Advise |
| Scope cuts, “stop here” calls | Consult | **Decide** |
| Writeup / README / resume bullet drafts | Correct facts | **Draft** |
| Blocker triage (paste error text) | Report | **Diagnose next step** |

**Cadence:** You finish a phase checklist item → ping me → I mark status and hand the next exact steps. No Autopilot, Conditional Access sprawl, or hybrid connect until Phase 1 is green.

**Kill criteria:** If Dev tenant signup is blocked >48h, we pivot the same writeup structure to **Intune education tenant alternatives** or document AD+GPO deepening only — I will call that pivot; you don’t invent a third stack.

---

## 6. Risks

| Risk | Mitigation |
|------|------------|
| Dev tenant waitlist / expiry | Screenshot early; note renew in README |
| Intune license missing on user | Assign E5 / Intune license before enroll |
| Slow policy sync | Document wait; use Company Portal sync; still a valid “ops patience” story |
| AD DNS fights Entra VM internet | Prefer Path A on a clean VM; Path B on a known-good domain client |
| Scope creep | This PROJECT.md is the contract |

---

## 7. Deliverables

- GitHub: `ad-intune-mini-tenant` (README = writeup)
- `/docs/screenshots/` redacted set (min 4): Entra join, Intune device blade, compliance, app
- Cross-link from linux-homelab
- Resume bullet (in WRITEUP)
- This PROJECT.md kept as internal run plan (can live in `/docs/project-plan.md` publicly if you want transparency)

---

## 8. Your next message to me

Send exactly:

1. M365 Dev tenant: `ready` or `blocked: <reason>`  
2. Hypervisor confirmation: Proxmox (assumed) + WIN11 VM name  
3. Windows version on endpoint VM  

I will reply with Phase 1 click-path only — no extra theory.
