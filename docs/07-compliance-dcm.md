# Module 07 — Compliance Settings (DCM)
**Difficulty:** 6/10 | **Lifecycle:** Stage → Deploy → Verify

---

## What It Is

DCM lets you define settings devices should have and measure compliance. CIs define individual checks. Baselines group CIs. Deploy Baselines to collections. Clients report Compliant/Non-Compliant. Remediation can auto-fix non-compliant settings.

## Key Concepts
| Term | Meaning |
|---|---|
| CI | Configuration Item — single check (registry, file, script, WMI) |
| Baseline | Collection of CIs deployed together |
| Remediation | Auto-applies correct value when non-compliant |
| Setting Types | Registry, File/Folder, Script (PowerShell), WMI, IIS, SQL, XML, LDAP |

## Create CI + Baseline
- STEP 1 — Assets and Compliance → Compliance Settings → Configuration Items → Create
- STEP 2 — Type: Windows Desktops and Servers. Name: e.g., "Check - RDP Enabled"
- STEP 3 — Add Setting: Registry → `HKLM\System\CurrentControlSet\Control\Terminal Server`, value `fDenyTSConnections`
- STEP 4 — Compliance Rule: value = 0. Enable Remediate.
- STEP 5 — Create Configuration Baseline → add CI
- STEP 6 — Deploy Baseline to collection → enable Remediate → set schedule
- STEP 7 — Monitor: Assets and Compliance → Deployments → select baseline → View Status

## Key Checks
- [ ] Baseline deployed and at least one evaluation cycle run
- [ ] Compliant % tracking upward
- [ ] Device properties → Configurations tab shows per-CI results

## Troubleshooting
| Issue | Fix |
|---|---|
| All non-compliant | Test registry path manually. Check `DCMAgent.log` |
| Remediation not working | Enable on BOTH CI setting AND baseline deployment |
| Script CI fails | Check PowerShell output format and execution policy. Check `DCMAgent.log` |

## Q&A
**Q1 — CI vs Baseline?** CI = single check. Baseline = group of CIs. Deploy Baselines, not individual CIs.
**Q2 — Per-device CI results?** Device properties → Configurations tab.
**Q3 — DCM log on client?** `DCMAgent.log`
**Q4 — Setting types?** Registry, File/Folder, Script, WMI, IIS, SQL, XML, LDAP, Assembly.
**Q5 — Enable auto-remediation?** Two places: CI Compliance Rule AND Baseline Deployment settings.
**Q6 — Script CI can remediate?** Yes — separate Discovery script (check) and Remediation script (fix).
**Q7 — Default evaluation schedule?** 7 days. Can go to 1 hour. Force: Control Panel → ConfigMgr → Configurations → Evaluate.
**Q8 — Console node for Baselines?** Assets and Compliance → Compliance Settings → Configuration Baselines.
**Q9 — Import pre-built CIs?** Yes — Microsoft publishes .cab files via Import Configuration Data.
**Q10 — Compliant vs Non-Compliant vs Error?** Compliant = all rules met. Non-Compliant = rule failed. Error = evaluation itself failed.
