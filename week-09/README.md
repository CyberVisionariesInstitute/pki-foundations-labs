# Week 9 — Environment Orientation & AD CS Foundations

**PKI Career Pathway · Phase 2 Operations · Cohort 1**

---

## Overview

Week 9 is the Phase 2 orientation week. Students meet the lab environment, understand the CA hierarchy design, learn the AD CS architecture and console, and practice the certutil-based environment verification they will use before every lab throughout Phase 2.

Phase 1 built the conceptual and diagnostic foundation. Phase 2 is operational — students are inside the environment that issues certificates, not analyzing certificates from the outside.

---

## Environment

- **Domain:** corp.cvilab.local
- **VMs:** DC01 (domain controller), PKI-SRV01 (enterprise Issuing CA), Root-CA (offline Root CA — powered off by default)
- **CA Hierarchy:** CVI Root CA (offline, standalone) → CVI Issuing CA 1 (enterprise, domain-joined, on PKI-SRV01)
- **Operational Accounts:** CORP\pki.admin (CA Administrator), CORP\cert.manager (Certificate Manager)
- **Startup Order:** DC01 (wait for Server Manager to fully load) → PKI-SRV01 → Root-CA off unless required

---

## Lesson Summary

| Lesson | Title | Estimated Time |
|--------|-------|----------------|
| Lesson 1 | Welcome to Phase 2: The Operations Layer | 18–20 min |
| Lesson 2 | CA Hierarchy Design — Why Two Tiers? | 18–20 min |
| Lesson 3 | AD CS Architecture — The Windows PKI Stack | 18–20 min |
| Lesson 4 | Reading the Environment Before You Touch It | 18–20 min |

---

## Lab Summary

| Lab | Title | Portfolio Submission |
|-----|-------|----------------------|
| Lab 01 | Environment Verification & VM Connectivity Check | `labs/week-09/lab-01-environment-verification.md` |
| Lab 02 | AD CS Console Exploration & CA Hierarchy Documentation | `labs/week-09/lab-02-environment-documentation.md` |

---

## File Inventory

### Lesson Scripts (PDF — Instructor Use)
- `CVI_Week9_Lesson1_Script.pdf` — Lesson 1 script
- `CVI_Week9_Lesson2_Script.pdf` — Lesson 2 script
- `CVI_Week9_Lesson3_Script.pdf` — Lesson 3 script
- `CVI_Week9_Lesson4_Script.pdf` — Lesson 4 script

### Gamma Prompts — Slide Decks (MD)
- `Week9-Lesson1-GammaPrompt.md` — Slide deck prompt, Lesson 1
- `Week9-Lesson2-GammaPrompt.md` — Slide deck prompt, Lesson 2
- `Week9-Lesson3-GammaPrompt.md` — Slide deck prompt, Lesson 3
- `Week9-Lesson4-GammaPrompt.md` — Slide deck prompt, Lesson 4

### Gamma Prompts — Resource Packs (MD)
- `Week9-Lesson1-ResourcePack-GammaPrompt.md` — Resource pack, Lesson 1
- `Week9-Lesson2-ResourcePack-GammaPrompt.md` — Resource pack, Lesson 2
- `Week9-Lesson3-ResourcePack-GammaPrompt.md` — Resource pack, Lesson 3
- `Week9-Lesson4-ResourcePack-GammaPrompt.md` — Resource pack, Lesson 4

### Kajabi
- `Week9-Kajabi-Descriptions.md` — Lesson + lab + live session descriptions for Kajabi

### Setup & Supplemental
- `CVI_Phase2_Week9_AppleSilicon_Guide.docx/.pdf` — Apple Silicon (UTM) setup guide
- `CVI_Phase2_Week9_Loom_Scripts.docx/.pdf` — Instructor Loom recording guide (Mac + Windows setup videos)

### Labs (in `/Labs/`)
- `lab-01-environment-verification.md` — Lab 01 student submission template
- `lab-02-environment-documentation.md` — Lab 02 student submission template
- `CVI_Phase2_Week9_Lab01_Student.docx/.pdf` — Lab 01 student guide
- `CVI_Phase2_Week9_Lab02_Student.docx/.pdf` — Lab 02 student guide
- `CVI_Phase2_Week9_Instructor_Lab_Guide.docx/.pdf` — Instructor reference (not for student distribution)

---

## Build Scripts (in `/Build Scripts/`)

Build scripts used to construct the lab VM environment are in the `Phase 2/Build Scripts/` folder. These are instructor-use only — students receive pre-built OVA files distributed via Google Drive.

| Script | Purpose |
|--------|---------|
| `Step2-DC01-ADPromotion.ps1` | Promotes DC01 to domain controller |
| `Step3-RootCA-Configure.ps1` | Configures the offline Root CA |
| `Step3b-RootCA-SignCSR.ps1` | Signs the Issuing CA CSR at the Root CA |
| `Step4a-PKI-SRV01-DomainJoin.ps1` | Joins PKI-SRV01 to the domain |
| `Step4b-PKI-SRV01-SubCA-Install.ps1` | Installs and configures the enterprise Issuing CA |
| `Step4c-PKI-SRV01-InstallCert.ps1` | Installs the signed CA certificate on PKI-SRV01 |
| `Step5-AD-Objects.ps1` | Creates PKI Admins OU, pki.admin, cert.manager |

---

## Continuity Notes

- **Phase 1 narrative (Metro General Hospital):** Ends at Week 7. Phase 2 uses corp.cvilab.local as the operational environment — no scenario wrapper. Students are the PKI operations engineer.
- **Submission format change:** Phase 2 uses per-lab markdown reports (not a single reflections.md). Lab submission templates are in `/Labs/`.
- **Lab 02 forward reference:** Lab 02's environment summary is referenced in the Week 15 backup and recovery lab. Students should treat it as a runbook entry.
- **HSM introduction:** Week 10, Lesson 4 + Lab 03 (instructor demo only, no student hands-on in Phase 2).
- **Phase 2 arc:** Weeks 9–16. Week 9 = orientation. Weeks 10–14 = operations. Week 15 = backup and recovery. Week 16 = Phase 2 mini capstone.

---

*Last updated: 2026-04-30 · CyberVisionaries Institute · Confidential — Instructor Use*
