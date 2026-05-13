# Module 05 — Patch Management (SUP/WSUS)
**Difficulty:** 7/10 | **Lifecycle:** Pre-stage → Stage → Deploy → Verify

---

## What It Is

MECM patch management uses the Software Update Point (SUP) to sync, deploy, and enforce patches. MECM owns WSUS entirely — never approve in the WSUS console. Flow: sync → filter → Software Update Group → Deployment Package → deploy to collection.

## Key Concepts
| Term | Meaning |
|---|---|
| SUP | Software Update Point — MECM role wrapping WSUS |
| SUG | Software Update Group — patch bundle container |
| Deployment Package | Holds update binaries downloaded from Microsoft |
| ADR | Automatic Deployment Rule — automates Patch Tuesday |
| Compliance | % of required updates installed per device |

## Deploy Patch Tuesday
- STEP 1 — Software Library → Software Updates → Synchronize Software Updates
- STEP 2 — Filter current month Critical + Security updates
- STEP 3 — Create Software Update Group with filtered updates
- STEP 4 — Download: right-click SUG → Download → create Deployment Package
- STEP 5 — Deploy to Pilot collection (Required, deadline 7 days)
- STEP 6 — Monitor pilot for 48-72 hours
- STEP 7 — Deploy to Production (Required, deadline 14 days, respect maintenance windows)
- STEP 8 — Monitor Monitoring → Software Update Deployments

## Key Checks
- [ ] SUP sync completed — check Last Sync Time
- [ ] Clients scanning — `WUAHandler.log` on client
- [ ] Deployment Package distributed to DPs
- [ ] Compliance % rising

## Troubleshooting
| Issue | Log / Fix |
|---|---|
| Sync fails | `wsyncmgr.log` — WSUS connectivity, SSL cert, WSUS DB |
| Clients not scanning | `WUAHandler.log`, `ScanAgent.log` — check WSUS ports 8530/8531 |
| 0% compliance | `UpdatesDeployment.log` — content on DP? Policy received? |
| WSUS conflict | Never approve/decline in WSUS console when MECM manages it |

## Q&A
**Q1 — What is an ADR?** Auto-creates SUGs and deployments on schedule. Eliminates manual Patch Tuesday.
**Q2 — Log for update scanning?** `WUAHandler.log` and `ScanAgent.log`
**Q3 — What is a SUG?** Software Update Group — container for updates deployed together.
**Q4 — WSUS/SUP default ports?** HTTP 8530, HTTPS 8531.
**Q5 — Approve in WSUS console?** Never — MECM owns WSUS. Direct changes cause sync conflicts.
**Q6 — What is a Deployment Package?** Container for update binaries. Downloads from MS, stored on UNC share.
**Q7 — Log for sync failures?** `wsyncmgr.log` on site server.
**Q8 — Update classifications to sync?** Critical Updates, Security Updates, Update Rollups, Service Packs, Definition Updates.
**Q9 — Per-device compliance in console?** Monitoring → Software Update Deployments → View Status.
**Q10 — Client enforcement log?** `UpdatesDeployment.log` and `WindowsUpdate.log`
