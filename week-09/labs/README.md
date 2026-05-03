# Week 9 — Student Submission Guide

**PKI Career Pathway · Phase 2 Operations · Cohort 1**

---

## Overview

Week 9 has two lab submissions. Both are markdown files committed to your GitHub portfolio repository. This is the Phase 2 submission format — one markdown report per lab, not a single reflections.md entry.

**Your portfolio repo structure for Week 9:**

```
labs/
└── week-09/
    ├── lab-01-environment-verification.md
    └── lab-02-environment-documentation.md
```

---

## Lab 01 — Environment Verification & VM Connectivity Check

**Submission file:** `labs/week-09/lab-01-environment-verification.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| VM startup order | Confirm DC01 → PKI-SRV01 startup; note anything unexpected |
| Login credentials | Table of VMs, accounts used, login success/failure |
| Test-Connection output | Full PowerShell output pasted in a code block |
| Get-Service CertSvc output | Full output in a code block |
| certsrv.msc observations | CA name, status, nodes visible — described in your own words |
| CertLog folder contents | Output of Get-ChildItem "C:\Windows\System32\CertLog" in a code block |
| Reflection | One thing that went well; one thing that was confusing or unexpected |

**Format requirements:**
- All command output in fenced code blocks (` ``` `)
- Reflection in plain prose — not bullet points
- Use the provided lab-01-environment-verification.md template as your starting structure

**What not to include:**
- Passwords or account credentials
- Screenshots (unless CLI output was genuinely unavailable — Lab 01 should produce all needed output via commands)

---

## Lab 02 — AD CS Console Exploration & CA Hierarchy Documentation

**Submission file:** `labs/week-09/lab-02-environment-documentation.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Part A — Console nodes | Table of five certsrv.msc nodes with observations |
| Part A — CA Properties | CDP path, AIA path, storage (database and log path) |
| Part A — Certificate Templates | List of templates visible in certtmpl.msc |
| Part B — certutil output | Full output of certutil -store -enterprise Root and -store -enterprise CA in code blocks |
| Part B — Observations | What you saw — Subject, Issuer, Thumbprint — in your own words |
| Part C — AD Users and Computers | PKI Admins OU accounts, pki.admin group memberships, cert.manager group memberships, domain-joined computers |
| Part C — Sites and Services | Server registered under Default-First-Site-Name |
| Part C — certtmpl.msc on DC01 | Confirmation that templates appear — same as on PKI-SRV01 |
| Part D — Environment Summary | Five areas, in your own words: topology, CA hierarchy, certificate templates, AD structure, one observation of interest |

**The Environment Summary (Part D) is the most important section.** Write it as if a qualified colleague will use it to navigate the environment without any other documentation. Cover:

1. **Environment Topology** — the three VMs, their roles, and IP addresses
2. **CA Hierarchy** — Root CA and Issuing CA, who signed what, why the Root is offline
3. **Certificate Templates** — which templates are published to CVI Issuing CA 1, what one or two of them are designed to issue
4. **Active Directory Structure** — the PKI Admins OU, the pki.admin and cert.manager accounts, what each can do
5. **One thing you found interesting or unexpected** — a genuine observation, not a generic statement

**Format requirements:**
- All command output in fenced code blocks
- Part D written in prose paragraphs — not bullet lists
- Screenshots accepted where certsrv.msc console or AD views cannot be captured via CLI

**Forward note:** The environment summary in Part D is referenced in the Week 15 backup and recovery lab. Treat it as a runbook entry — it has operational value beyond Week 9.

---

## Commit Instructions

```bash
# Navigate to your portfolio repo
cd your-pki-portfolio

# Create the week-09 folder if it does not exist
mkdir -p labs/week-09

# Move or copy your completed lab files into the folder
# Then stage, commit, and push

git add labs/week-09/
git commit -m "Week 9: Environment verification and documentation labs"
git push
```

**Commit message format:**
```
Week 9: <brief description>
```

Examples:
- `Week 9: Environment verification and documentation labs`
- `Week 9: Lab 01 and 02 — corp.cvilab.local baseline documentation`

---

## Common Issues

**Lab 01:**

| Issue | Fix |
|-------|-----|
| PKI-SRV01 login fails | DC01 may not have fully loaded. Wait 60 seconds and retry. |
| CertSvc shows Stopped | Run `Start-Service CertSvc` in PowerShell as Administrator. |
| certsrv.msc shows red X | CertSvc is stopped or the CA database is in an error state. Check Event Viewer → Application and Services Logs → AD CS. |
| Test-Connection DC01 fails | Confirm DC01 is running and fully loaded. Check IP configuration with `Get-NetIPAddress`. |

**Lab 02:**

| Issue | Fix |
|-------|-----|
| certutil -store -enterprise Root shows 0 entries | Root CA certificate was not deployed via Group Policy. Notify instructor — this is a lab environment build issue. |
| certtmpl.msc on DC01 shows no templates | AD replication may not be complete. Wait 5 minutes and refresh. If it persists, notify instructor. |
| Cannot find PKI Admins OU in AD | Look in the domain root — the OU may be directly under corp.cvilab.local, not nested under another OU. |

---

*CyberVisionaries Institute · PKI Career Pathway Phase 2 · Cohort 1 · 2026*
