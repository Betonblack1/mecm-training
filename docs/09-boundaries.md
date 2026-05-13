# Module 09 — Boundaries & Boundary Groups
**Difficulty:** 6/10 | **Lifecycle:** Pre-stage → Stage → Verify

---

## What It Is

Boundaries and Boundary Groups define which site systems a client uses based on network location. Boundary = network definition. Boundary Group = container linking boundaries to site systems. Misconfigured boundary groups are the #1 silent failure — clients register but pull from wrong DP.

## Key Concepts
| Term | Meaning |
|---|---|
| Boundary | IP Subnet, IP Range, AD Site, or IPv6 Prefix |
| Boundary Group | Links boundaries to site systems (MP, DP, SUP) |
| Default BG | Built-in catch-all — clients with no match fall here |
| Fallback | Neighbor boundary group for content fallback after timer |
| Site Assignment | Boundary group can determine client site assignment |

## Configure Boundary Groups
- STEP 1 — Administration → Hierarchy Configuration → Boundaries → Create Boundary → IP Subnet
- STEP 2 — Administration → Boundary Groups → Create Boundary Group
- STEP 3 — Boundaries tab: add subnet boundaries
- STEP 4 — References tab: add MP, DP, SUP
- STEP 5 — Check "Use this boundary group for site assignment"
- STEP 6 — Relationships tab: add neighbor BG with fallback time
- STEP 7 — Test: install client, check `locationservices.log`

## Key Checks
- [ ] Every client subnet in at least one boundary
- [ ] Each boundary group has MP and DP assigned
- [ ] `LocationServices.log` shows correct MP
- [ ] Default BG has no DPs unless intentional

## Troubleshooting
| Issue | Fix |
|---|---|
| No boundary match | `locationservices.log` — check for "No matching boundary" |
| Wrong DP | Overlapping boundary groups — check Administration → Boundaries |
| Content not found | DP not in correct boundary group References tab |
| Slow fallback | Reduce fallback time in Relationships tab |

## Lab Notes (ESP Environment)
- mypdomain.com: subnet 172.16.0.x → Boundary Group P (MP + DP for mypdomain)
- mybdomain.com: subnet 191.16.0.x → Boundary Group B (MP + DP for mybdomain)
- Keep separate — prevents cross-domain content routing

## Q&A
**Q1 — Boundary vs Boundary Group?** Boundary = network definition. Boundary Group = container linking to site systems.
**Q2 — Four boundary types?** IP Subnet, IP Address Range, AD Site, IPv6 Prefix.
**Q3 — No boundary match?** Falls to Default BG. If no site systems there, content fails.
**Q4 — Log for MP selection?** `LocationServices.log` on client.
**Q5 — Where to assign DP to group?** Boundary Group → References tab → Add → select DP.
**Q6 — Default Site Boundary Group?** Built-in catch-all. Don't add DPs unless you want all unmatched clients downloading from them.
**Q7 — Fallback relationship?** Neighbor BG tried for content after time threshold. Configured in Relationships tab.
**Q8 — Boundary in multiple groups?** Allowed but causes overlap. Keep in one group unless intentional.
**Q9 — Setting for site assignment?** General tab → "Use this boundary group for site assignment."
**Q10 — Minimum boundary setup for lab?** One Boundary per subnet, one Boundary Group per domain, each linked to its own MP/DP/SUP.
