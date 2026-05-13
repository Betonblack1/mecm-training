# Module 01 — Site Hierarchy & Roles
**Difficulty:** 6/10 | **Lifecycle:** Pre-stage → Stage → Deploy → Verify

---

## What It Is

Site Hierarchy defines how MECM is physically and logically organized — how many sites you have, how they relate to each other, and which server roles handle what. Every MECM action flows through this structure: clients check in with their Management Point, pull content from their Distribution Point, and receive updates from the Software Update Point. Design this correctly before anything else — bad hierarchy decisions are expensive to fix.

---

## Lifecycle Placement

| Phase | Activity |
|---|---|
| **Pre-stage** | Design hierarchy, plan SQL instance, allocate VMs, extend AD schema |
| **Stage** | Install Site Server + SQL, configure MP and DP, verify site status |
| **Deploy** | Add SUP, Reporting Point, assign clients via boundary groups |
| **Verify** | Component status green, clients registering, MP publishing to AD |

---

## Core Roles

| Role | Abbr | Purpose |
|---|---|---|
| Site Server | SS | The brain. Processes all site operations. One per site. |
| Management Point | MP | Clients talk here for policy and inventory. Required. |
| Distribution Point | DP | Holds content. Clients pull from here. One per subnet ideally. |
| Software Update Point | SUP | Wraps WSUS. Manages patch sync and deployment. |
| SQL Server | SQL | Stores site DB. Separate VM in production. |
| Reporting Services Point | RSP | Hosts SSRS reports. Add after site is stable. |

---

## Install Order

- **STEP 0** — Patch all VMs fully before touching the installer (standing rule)
- **STEP 1** — Extend AD schema: run `extadsch.exe` from MECM media. Requires Schema Admin. One-time.
- **STEP 2** — Create System Management container in AD, delegate Full Control to Site Server computer account
- **STEP 3** — Install SQL Server 2022 on SQL VM. Collation: `SQL_Latin1_General_CP1_CI_AS`
- **STEP 4** — Install Windows ADK + WinPE add-on on Site Server
- **STEP 5** — Run Prerequisite Checker. Resolve all errors before proceeding.
- **STEP 6** — Install Primary Site via setup wizard. Set site code (3 chars — cannot change after)
- **STEP 7** — Verify: Monitoring → Site Status → all green. Check `sitecomp.log`
- **STEP 8** — Add Distribution Point role. Configure boundary groups.
- **STEP 9** — Install SUP role. WSUS installed first, not configured — let MECM manage it.

---

## Key Checks

- [ ] Monitoring → Site Status → Component Status — all green
- [ ] System Management container in AD with correct permissions
- [ ] DP shows no errors — check `distmgr.log`
- [ ] SUP first sync completes — check `wsyncmgr.log`
- [ ] Test client registers — check `ccmsetup.log` on client

---

## Troubleshooting

| Issue | Cause / Fix |
|---|---|
| MP can't publish | Verify System Management container and Full Control delegation. Check `mpcontrol.log` |
| Install fails | SQL collation wrong — must be `SQL_Latin1_General_CP1_CI_AS`. Rebuild SQL to fix. |
| Prereq failures | Check `ConfigMgrPrereq.log` at C:\ root |
| Red components | Right-click → Show Messages. Logs at `C:\Program Files\Microsoft Configuration Manager\Logs\` |
| Clients not checking in | Verify firewall allows 80/443/10123. Check `mpcontrol.log` |

---

## Q&A

**Q1 — Where do you check site component health?**
Monitoring workspace → Site Status → Component Status.

**Q2 — Log file for MP communication?**
`mpcontrol.log` on the site server.

**Q3 — Required SQL collation?**
`SQL_Latin1_General_CP1_CI_AS` — cannot be changed after install.

**Q4 — What AD container must exist for the MP to publish?**
System Management container under System in AD. Site Server computer account needs Full Control.

**Q5 — What is a site code and can it be changed?**
3-character alphanumeric (e.g., P01). Cannot be changed after installation.

**Q6 — What must be installed before the SUP role?**
WSUS — installed but not configured. MECM manages WSUS configuration.

**Q7 — Default log file location on site server?**
`C:\Program Files\Microsoft Configuration Manager\Logs\` — use CMTrace.exe.

**Q8 — Difference between Primary and Secondary Site?**
Primary has its own SQL DB. Secondary uses parent Primary's DB (for remote/WAN sites).

**Q9 — Tool to run before MECM installation?**
Prerequisite Checker — `prereqchk.exe`. Logs to `ConfigMgrPrereq.log` at C:\ root.

**Q10 — Two Windows components required on Site Server?**
Windows ADK and the WinPE Add-on. Must match MECM version.
