# Module 03 — Collections
**Difficulty:** 4/10 | **Lifecycle:** Stage → Deploy → Verify

---

## What It Is

Collections are the targeting mechanism for everything in MECM. A collection is a group of devices or users defined by rules. Software deployments, patches, task sequences, compliance baselines — everything targets a collection. Bad collection design = wrong deployments.

## Types & Rules
| Type | Description |
|---|---|
| Device Collection | Groups computers. Target for software, patches, OSD. |
| User Collection | Groups users. Target for user-available apps. |
| Direct Rule | Static — manually added devices. Never auto-updates. |
| Query Rule | Dynamic — WQL query auto-populates on schedule. |
| Include Rule | Pulls in all members of another collection. |
| Exclude Rule | Removes members of another collection. |

## Steps — Query-Based Collection
- STEP 1 — Assets and Compliance → Device Collections → Create Device Collection
- STEP 2 — Set Limiting Collection (appropriate parent, not All Systems for production)
- STEP 3 — Add Membership Rule → Query Rule → Edit Query Statement
- STEP 4 — Write WQL: `SELECT * FROM SMS_R_System WHERE OperatingSystemNameandVersion LIKE '%Server%'`
- STEP 5 — Set Update Schedule (1 day for critical collections)
- STEP 6 — Preview members before saving

## Key Checks
- [ ] Member count matches expectation
- [ ] Limiting collection not too broad
- [ ] Update schedule appropriate for urgency
- [ ] No circular Include/Exclude references

## Troubleshooting
| Issue | Fix |
|---|---|
| Missing members | Check WQL syntax, verify hardware inventory current |
| Stale members | Update schedule not run or incremental updates disabled |
| Wrong deployments | Check all query/include rules. Use Deployments tab on device. |
| Performance | Max 200 incremental collections per site |

## Q&A
**Q1 — Limiting Collection?** Parent boundary restricting membership. Collection only contains devices already in Limiting Collection.
**Q2 — Query language?** WQL — WMI Query Language.
**Q3 — Preview membership?** Collection wizard → after query rule → click Preview.
**Q4 — Direct vs Query rule?** Direct = static, manual. Query = dynamic, WQL, auto-updates.
**Q5 — Incremental Update?** Evaluates only changed records. Cap: ~200 per site.
**Q6 — Exclude devices?** Add Exclude Collection rule pointing to collection of devices to exclude.
**Q7 — See deployments targeting a device?** Device properties → Deployments tab.
**Q8 — Built-in collection for all clients?** All Systems (device) and All Users. Never deploy production directly to All Systems.
**Q9 — Force immediate membership update?** Right-click collection → Update Membership.
**Q10 — Console node for Device Collections?** Assets and Compliance workspace → Device Collections.
