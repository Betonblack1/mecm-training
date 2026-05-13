# Module 10 — SQL & the CM Database
**Difficulty:** 8/10 | **Lifecycle:** Pre-stage → Stage → Verify

---

## What It Is

The MECM site database stores everything — inventory, deployments, compliance, collections, patch status. SQL health = MECM health. Key areas: collation, memory limits, key views, maintenance tasks, and backup.

## SQL Requirements & Sizing
| Setting | Value |
|---|---|
| Collation | `SQL_Latin1_General_CP1_CI_AS` — set at install, cannot change |
| Max Memory | Total RAM minus 4-8GB for OS |
| DB Name | CM_[SiteCode] e.g., CM_P01 — never rename |
| SQL Version | SQL Server 2022 for MECM 2509 |
| TempDB Files | One per CPU core, max 8, equal size |
| DB Files | Separate .mdf and .ldf to different disks |

## Key Views
| View | Contains |
|---|---|
| v_R_System | All discovered devices. Join key: ResourceID. |
| v_GS_INSTALLED_SOFTWARE | Installed software per device |
| v_GS_PROCESSOR | CPU inventory |
| v_GS_PHYSICAL_MEMORY | RAM inventory |
| v_GS_DISK | Disk inventory |
| v_UpdateComplianceStatus | Patch compliance per device per update |
| v_DeploymentSummary | Deployment results |
| v_ClientDeploymentState | Client health state |

## Useful Queries

```sql
-- All devices + OS
SELECT Name, OperatingSystemNameandVersion FROM v_R_System

-- Software on a device
SELECT ProductName, ProductVersion
FROM v_GS_INSTALLED_SOFTWARE
WHERE ResourceID = (SELECT ResourceID FROM v_R_System WHERE Name = 'HOSTNAME')

-- Non-compliant patch count per device
SELECT sys.Name, COUNT(*) AS NonCompliant
FROM v_UpdateComplianceStatus ucs
JOIN v_R_System sys ON ucs.ResourceID = sys.ResourceID
WHERE ucs.Status = 2
GROUP BY sys.Name
ORDER BY NonCompliant DESC
```

## Maintenance Tasks
- STEP 1 — Administration → Site Configuration → Sites → right-click → Site Maintenance
- STEP 2 — Enable: Backup Site Server + all Delete Aged Data tasks
- STEP 3 — SQL maintenance: weekly index rebuild, nightly reorganize, daily stats update
- STEP 4 — Monitor DB size. Set transaction log to Simple recovery if full backups configured.
- STEP 5 — Review SQL Error Log weekly for severity 17+ errors

## Key Checks
- [ ] SQL collation confirmed in SSMS: Server Properties → General
- [ ] SQL max memory set: Server Properties → Memory
- [ ] Backup Site Server task enabled and running
- [ ] Delete Aged Data tasks all enabled

## Troubleshooting
| Issue | Fix |
|---|---|
| Console slow | Index fragmentation — run index maintenance |
| DB growing fast | Aged data tasks not running. Enable all Delete Aged Data tasks. |
| Backup fails | Monitoring → Site Status → Backup Site Server component |
| SQL memory | SSMS → Server Properties → Memory → set Max Server Memory |

## Q&A
**Q1 — Required collation?** `SQL_Latin1_General_CP1_CI_AS` — set at SQL install, cannot change without rebuild.
**Q2 — Default CM DB name?** `CM_[SiteCode]` e.g., CM_P01. Never rename after creation.
**Q3 — Why cap SQL memory?** Prevents SQL from starving OS. Set max = total RAM minus 4-8GB.
**Q4 — Primary join key?** `ResourceID` in `v_R_System`.
**Q5 — Where to enable maintenance tasks?** Administration → Site Configuration → Sites → right-click → Site Maintenance.
**Q6 — Why console gets slow?** Index fragmentation + aged data not purged.
**Q7 — v_GS_INSTALLED_SOFTWARE contents?** ProductName, ProductVersion, Publisher, InstallDate per device.
**Q8 — v_ views or raw tables?** Always v_ views — supported and stable. Raw tables change between versions.
**Q9 — TempDB config?** One file per CPU core, max 8, equal size.
**Q10 — How to back up MECM?** Enable Backup Site Server maintenance task + SQL native backups. Test restores.
