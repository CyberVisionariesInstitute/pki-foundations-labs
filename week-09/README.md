# Week 9 — Environment Orientation & AD CS Foundations

## Focus

This week you step inside the infrastructure.

Phase 1 built the diagnostic and conceptual foundation: how certificates work, how to read them, how to diagnose failures, and how to trace a trust chain from endpoint to root. Phase 2 is operational. You are no longer analyzing certificates from the outside — you are responsible for the environment that issues them.

Week 9 is your orientation. You meet the three-VM lab environment running the corp.cvilab.local domain, understand why the two-tier CA hierarchy is designed the way it is, learn the AD CS architecture and the certsrv.msc console you will use throughout Phase 2, and establish the verification habit that every experienced PKI engineer follows: read the environment before you touch it.

Two labs follow the four lessons. Lab 01 confirms the environment is operational. Lab 02 builds your first Phase 2 portfolio artifact — a structured environment summary that you will reference in later weeks.

Week 10 is where issuance begins. Come to it knowing the environment.

---

## Outcomes

By the end of this week, you can:

- Describe the three-VM architecture of corp.cvilab.local and explain the role of each VM and each operational account
- Explain why the CA hierarchy uses two tiers — Root CA offline, Issuing CA online — and describe the security rationale for that design
- Navigate the five nodes of the certsrv.msc console and explain what each one records
- Run the four baseline certutil commands to verify environment state before beginning lab work
- Describe the Phase 2 lab submission format and explain why it differs from Phase 1

---

## The Phase 2 Environment

Before starting, internalize the architecture. You will operate inside this environment every week through Phase 2.

| Component | Detail | Notes |
|---|---|---|
| Domain | corp.cvilab.local | Windows Active Directory domain |
| DC01 | Domain Controller — runs AD DS and DNS | Start first. Wait for Server Manager to fully load. |
| PKI-SRV01 | Enterprise Issuing CA — runs CVI Issuing CA 1 | Start second. Log in as CORP\pki.admin. |
| Root-CA | Offline Root CA — not domain-joined | Leave powered off unless a lab requires it. |
| CORP\pki.admin | CA Administrator | Manages templates, CRLs, CA configuration |
| CORP\cert.manager | Certificate Manager | Requests certificates — cannot manage the CA |
| CA Hierarchy | CVI Root CA (offline) → CVI Issuing CA 1 (PKI-SRV01) | Root signed the Issuing CA cert and stays offline |

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Environment Verification | `labs/week-09/lab-01-environment-verification.md` |
| Lab 02 — Environment Documentation | `labs/week-09/lab-02-environment-documentation.md` |

Both are markdown files committed to your GitHub portfolio repo under `labs/week-09/`. Each lab has its own submission template in the `/Labs/` folder — use it.

> **Note on submission format:** Phase 2 uses per-lab markdown reports. There is no single `reflections.md` entry this week. Lab 02's environment summary is a structured document — write it as a runbook entry, not a worksheet. You will reference it in later weeks.

---

## Checklist

- [ ] Start VMs in the correct order: DC01 first (wait for Server Manager), then PKI-SRV01
- [ ] Confirm CertSvc is running on PKI-SRV01: `Get-Service CertSvc`
- [ ] Open certsrv.msc and confirm CVI Issuing CA 1 shows a green icon
- [ ] Lab 01 — Verify network connectivity between PKI-SRV01 and DC01 using `Test-Connection`
- [ ] Lab 01 — Locate the CA database and log files in `C:\Windows\System32\CertLog`
- [ ] Lab 01 — Commit `lab-01-environment-verification.md` to your portfolio repo
- [ ] Run the four baseline certutil commands: `-store -enterprise Root`, `-store -enterprise CA`, `-ping`, `-URL`
- [ ] Lab 02 — Navigate all five certsrv.msc nodes and document current contents of each
- [ ] Lab 02 — Review CA Properties and locate the CDP, AIA, and storage path settings
- [ ] Lab 02 — Navigate Active Directory and document the PKI Admins OU, pki.admin, and cert.manager accounts
- [ ] Lab 02 — Confirm the same template list appears in certtmpl.msc on both PKI-SRV01 and DC01
- [ ] Lab 02 — Write the five-section environment summary: topology, CA hierarchy, templates, AD structure, one item of interest
- [ ] Lab 02 — Commit `lab-02-environment-documentation.md` to your portfolio repo
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- Lab 01 confirms CertSvc status with actual PowerShell output in a code block — not just "it was running"
- Lab 02's environment topology section reads like a network diagram in prose: what each VM is, what role it plays, and how they relate to each other
- The CA hierarchy section names both CAs, identifies which is the trust anchor, and explains why the Root CA is kept offline
- certutil -store output is pasted in a code block and interpreted — not just pasted with no comment
- The template comparison notes at least one difference between what is visible in certtmpl.msc and what is published to the CA — and explains what that difference means
- Lab 02's "one item of interest" is specific and evidence-based, not "I found it interesting that there were templates"

---

## Connecting to Prior Weeks

Week 9 is where Phase 1 becomes operational context:

- **Week 1** — PKI fundamentals: the trust anchor concept from Week 1 is now the offline Root CA you are responsible for keeping powered off
- **Week 3** — Certificate anatomy: every certutil -store output you read this week uses the field vocabulary you built in Week 3
- **Week 5** — Lifecycle: the validity periods in the CA properties are lifecycle decisions — the same thinking from Week 5 applied at the infrastructure level
- **Week 6** — Diagnostic framework: the pre-lab verification sequence in Lesson 4 is the verify-before-you-change discipline from Week 6 applied to the CA itself

---

*CVI PKI Career Pathway — Phase 2 Operations*

*Last updated: 2026-04-30 · CyberVisionaries Institute · Confidential — Instructor Use*
