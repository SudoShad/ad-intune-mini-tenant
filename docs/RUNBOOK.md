# Runbook — Phase execution (operator steps)

Use with PROJECT.md. Do not invent extra portals.

## Phase 0
1. Open https://developer.microsoft.com/microsoft-365/dev-program → join / renew.
2. Sign in to https://admin.microsoft.com → confirm tenant.
3. Open https://intune.microsoft.com → land on admin center (if license wall, assign E5 to admin/lab user first).
4. Proxmox: New VM or clone — Windows 11, ≥4 GB RAM (8 GB better), snapshot `clean`.
5. GitHub: repo `SudoShad/ad-intune-mini-tenant` already exists — pull latest docs; add screenshots in Phase 1 (do not recreate the repo).

## Phase 1 — Enroll
1. Entra admin center → Users → New user `lab.endpoint@…onmicrosoft.com` → assign Microsoft 365 E5 Developer (or license that includes Intune).
2. On WIN11 VM (local admin): Settings → Accounts → Access work or school → Connect → **Join this device to Microsoft Entra ID** → sign in as lab user.
3. Admin PowerShell: `dsregcmd /status` → expect `AzureAdJoined : YES`.
4. Intune → Devices → find the device (may take a few minutes).
5. Devices → Compliance policies → Create → Windows 10 and later → require antivirus real-time + firewall → assign to lab user/group.
6. Install Company Portal from Store on the device → Sync.
7. Intune → Apps → add Company Portal (if not present) + one Store app → assign → Sync → confirm install.
8. Screenshot four blades; drop into `docs/screenshots/` with names matching WRITEUP.md; redact tenant strings in an image editor.

## Phase 2 — Publish
1. Replace WRITEUP placeholders (what broke, app name, dates).
2. Copy WRITEUP.md → README.md.
3. Add one sentence + link under linux-homelab “Next steps”.
4. Send Sin the public repo URL for a senior review pass.

## Phase 2b — AD (optional)
1. Only on a VM with working lab DNS to DC01.
2. System Properties → Change → Domain → lab domain → reboot.
3. `whoami` / screenshot → add section 6 in WRITEUP.

## Blocker template (paste to Sin)
```
Phase: 
Step number: 
What I expected: 
What I got (exact error text): 
Screenshot: attached / not yet
```
