# Module 08 — Reporting (SSRS)
**Difficulty:** 7/10 | **Lifecycle:** Stage → Deploy → Verify

---

## What It Is

MECM reporting uses SQL Server Reporting Services (SSRS) via the Reporting Services Point role. 400+ built-in reports available. Custom reports query v_ views in the CM database — never raw tables.

## Key Views
| View | Contains |
|---|---|
| v_R_System | All discovered devices. Core join key: ResourceID. |
| v_GS_INSTALLED_SOFTWARE | All installed software per device |
| v_GS_PROCESSOR | CPU hardware inventory |
| v_GS_PHYSICAL_MEMORY | RAM hardware inventory |
| v_UpdateComplianceStatus | Per-device per-update patch compliance |
| v_DeploymentSummary | Deployment results summary |
| v_Collection | Collection definitions and metadata |

## Useful Queries

```sql
-- All devices and OS version
SELECT Name, OperatingSystemNameandVersion FROM v_R_System

-- Software installed on a specific device
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

## Setup Steps
- STEP 1 — Install SSRS on SQL VM
- STEP 2 — Configure SSRS — set service account, Web Service URL
- STEP 3 — MECM: Administration → Site Configuration → Servers → Add Reporting Services Point
- STEP 4 — Reporting account: db_datareader on CM DB
- STEP 5 — Wait for 400+ reports to deploy — check `srsrp.log`
- STEP 6 — Run reports: Monitoring → Reporting → Reports

## Troubleshooting
| Issue | Fix |
|---|---|
| No reports | RSP not configured or SSRS down. Check `srsrp.log` |
| Report errors | Reporting account needs db_datareader |
| Stale data | Client inventory not running — check inventory schedules |

## Q&A
**Q1 — Role required?** Reporting Services Point (RSP).
**Q2 — Core device view?** `v_R_System`
**Q3 — Where to run reports?** Monitoring → Reporting → Reports.
**Q4 — DB permission for reporting account?** `db_datareader`
**Q5 — Tool to build custom reports?** SQL Server Report Builder — save as .rdl.
**Q6 — Why v_ views not raw tables?** v_ views are supported and stable. Raw tables change between MECM versions.
**Q7 — RSP log file?** `srsrp.log`
**Q8 — Schedule report via email?** SSRS Report Manager → report → Subscribe → configure delivery.
**Q9 — Patch compliance view?** `v_UpdateComplianceStatus`
**Q10 — Number of built-in reports?** 400+
