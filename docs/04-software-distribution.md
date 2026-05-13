# Module 04 — Software Distribution
**Difficulty:** 5/10 | **Lifecycle:** Stage → Deploy → Verify

---

## What It Is

Software Distribution covers deploying applications, scripts, and packages to managed devices. Applications (modern) use detection methods; Packages/Programs (legacy) are script-based. Three phases: create content → distribute to DP → deploy to collection.

## Application vs Package
| Type | Use |
|---|---|
| Application | Modern. Detection method = MECM knows if installed. Supports supersedence, requirements, user installs. |
| Package/Program | Legacy. Script-based. No detection — MECM does not track install state. |

## Key Concepts
- **Detection Method** — Registry key, file/folder, or MSI product code. Tells MECM if app is installed. Wrong detection = constant re-installs.
- **Deployment Type** — Actual install logic inside an Application (command, detection, requirements, UX).
- **Requirements** — OS version, disk space, arch. Evaluated before installing.
- **Supersedence** — Links new version to old. Auto-uninstalls old, installs new.

## Steps
- STEP 1 — Software Library → Application Management → Applications → Create Application
- STEP 2 — Set install command, uninstall command, detection method
- STEP 3 — Configure User Experience (System vs User, Software Center behavior, restart)
- STEP 4 — Distribute Content → right-click → Distribute Content → select DP
- STEP 5 — Wait for distribution — Monitoring → Distribution Status
- STEP 6 — Deploy → right-click → Deploy → collection, Required vs Available, schedule
- STEP 7 — Monitor → Monitoring → Deployments

## Key Checks
- [ ] Content distribution complete — all DPs green
- [ ] Detection method validated — Simulate Deployment on test machine
- [ ] Deployment appears in Software Center
- [ ] Success count increasing in Monitoring → Deployments

## Troubleshooting
| Issue | Log |
|---|---|
| Content missing | `datatransferservice.log` |
| App re-installs | `AppDiscovery.log` — detection method wrong |
| Install fails | `AppEnforce.log` — exact error and return code |
| Requirements not met | `AppDiscovery.log` — shows requirements evaluation |

## Q&A
**Q1 — Three detection method types?** File/Folder, Registry, MSI Product Code.
**Q2 — Required vs Available?** Required = forced. Available = user installs from Software Center.
**Q3 — Install enforcement log?** `AppEnforce.log`
**Q4 — Must do before deploying?** Distribute content to DP first.
**Q5 — Per-device deployment status?** Monitoring → Deployments → Asset Details tab.
**Q6 — What is a Deployment Type?** Install logic within an App. Multiple types per app for different scenarios.
**Q7 — What does Simulate Deployment do?** Runs detection only — no install. Validates detection logic.
**Q8 — What is Supersedence?** Links new version to old — auto-uninstalls old when new deploys.
**Q9 — Log for app detection on client?** `AppDiscovery.log`
**Q10 — Where to monitor content distribution?** Monitoring → Distribution Status → Content Status.
