# Module 06 — Operating System Deployment (OSD)
**Difficulty:** 9/10 | **Lifecycle:** Pre-stage → Stage → Deploy → Verify

---

## What It Is

OSD automates full OS installations via Task Sequences. A TS is an ordered list of steps executed in WinPE or Windows. Hardest module — chains boot images, OS images, driver packages, PXE, and boundary groups. One broken link = silent failure.

## Key Components
| Component | Purpose |
|---|---|
| Boot Image | WinPE — loads first via PXE, runs TS engine, partitions disk |
| OS Image | Full Windows WIM applied to disk |
| Task Sequence | Ordered steps MECM executes |
| Driver Package | Drivers injected during WinPE or post-OS |
| PXE | Network boot — requires DHCP options 66/67 |
| Unknown Computers | Built-in collection for bare metal (devices not yet in MECM DB) |

## Bare Metal TS Steps
- STEP 0 — Patch all VMs. Confirm ADK version matches MECM before building boot image.
- STEP 1 — Enable PXE on DP → DP Properties → PXE tab
- STEP 2 — Distribute Boot Image to PXE-enabled DP
- STEP 3 — Import OS Image (.wim)
- STEP 4 — Create Driver Package, distribute to DP
- STEP 5 — Create Task Sequence → Install existing image package
- STEP 6 — Edit TS: Partition Disk → Apply OS → Apply Windows Settings → Apply Drivers → Setup Windows and ConfigMgr → Install Software → Domain Join
- STEP 7 — Deploy TS to All Unknown Computers
- STEP 8 — PXE boot target device (F12 physical / VM BIOS)
- STEP 9 — Monitor: `SMSTS.log` at `X:\Windows\Temp\SMSTSLog\`

## Key Checks
- [ ] Boot image on PXE-enabled DP before any PXE boot
- [ ] DHCP options 66 (TFTP server) and 67 (boot file) configured
- [ ] Device shows in MECM after TS with correct site assignment
- [ ] Domain join succeeded — device in correct OU
- [ ] SMSTS.log shows no errors, exit code 0

## Troubleshooting
| Issue | Fix |
|---|---|
| PXE no boot | DHCP 66/67 missing, boot image not on DP. Check `pxecontrol.log` |
| TS fails in WinPE | `SMSTS.log` at `X:\Windows\Temp\SMSTSLog\`. F8 for command prompt. |
| Driver missing | Import INF driver, add Apply Driver Package step with hardware model WMI condition |
| Domain join fails | Service account needs rights to create computer objects in OU |
| Client not registering | Check site code and MP in Setup Windows and ConfigMgr step |

## Q&A
**Q1 — SMSTS.log location in WinPE?** `X:\Windows\Temp\SMSTSLog\SMSTS.log`
**Q2 — DHCP options 66 and 67?** 66 = TFTP server IP, 67 = boot file name.
**Q3 — Command prompt in WinPE?** F8 (must be enabled in boot image properties).
**Q4 — Collection for bare metal?** All Unknown Computers.
**Q5 — Boot Image vs OS Image?** Boot Image = WinPE (loads first). OS Image = Windows WIM (applied to disk).
**Q6 — TS step that installs the MECM client?** Setup Windows and ConfigMgr.
**Q7 — ADK components required?** Windows ADK + WinPE Add-on. Must match MECM version.
**Q8 — TS variable for restart?** `SMSTSRebootRequested = TRUE`
**Q9 — How to inject drivers for specific hardware?** Driver Package + Apply Driver Package step with WMI condition on model.
**Q10 — What is prestaged media?** WIM written to USB/disk — used when PXE unavailable.
