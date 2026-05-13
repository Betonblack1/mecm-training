# MECM Training — Intermediate Reference

Interactive study guide covering the 10 core topics needed for intermediate MECM knowledge.

**Live site:** `https://betonblack1.github.io/mecm-training/`

---

## Modules

| # | Topic | Difficulty |
|---|---|---|
| 01 | Site Hierarchy & Roles | 6/10 |
| 02 | Client Installation Methods | 5/10 |
| 03 | Collections | 4/10 |
| 04 | Software Distribution | 5/10 |
| 05 | Patch Management (SUP/WSUS) | 7/10 |
| 06 | Operating System Deployment (OSD) | 9/10 |
| 07 | Compliance Settings (DCM) | 6/10 |
| 08 | Reporting (SSRS) | 7/10 |
| 09 | Boundaries & Boundary Groups | 6/10 |
| 10 | SQL & the CM Database | 8/10 |

---

## Structure

```
mecm-training/
├── index.html          # Interactive training site (GitHub Pages)
├── README.md           # This file
└── docs/               # Flat markdown reference sheets
    ├── 01-site-hierarchy.md
    ├── 02-client-installation.md
    ├── 03-collections.md
    ├── 04-software-distribution.md
    ├── 05-patch-management.md
    ├── 06-osd.md
    ├── 07-compliance-dcm.md
    ├── 08-reporting.md
    ├── 09-boundaries.md
    └── 10-sql-database.md
```

Each module covers:
- What it is (plain English)
- Lifecycle placement (Pre-stage / Stage / Deploy / Verify)
- Key concepts and roles
- Step-by-step procedures
- Key checks after setup
- Troubleshooting (top failure points + log files)
- 10 Q&A flashcards

---

## Deploy to GitHub Pages

1. Push repo to `betonblack1/mecm-training`
2. Settings → Pages → Source: Deploy from branch → `main` → `/ (root)`
3. Access at `https://betonblack1.github.io/mecm-training/`

---

## Environment Notes

Built for: MECM 2509 | SQL Server 2022 | Windows Server 2022

Domains: `mypdomain.com` (172.16.0.x) | `mybdomain.com` (191.16.0.x)
