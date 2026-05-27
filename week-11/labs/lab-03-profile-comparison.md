# Lab 03: Compare Certificate Profiles Side by Side

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 11  
**Submission Path:** `labs/week-11/lab-03-profile-comparison.md`

---

## Overview

Lab 03 is a synthesis exercise. You have issued three certificates across Weeks 10 and 11:

1. **TLS certificate** — issued from CVI-WebServer in Week 10, Lab 02
2. **Service account certificate** — issued from CVI-ServiceAccount in Week 11, Lab 01
3. **Code signing certificate** — issued from CVI-CodeSigning in Week 11, Lab 02

In this lab, you inspect all three certificates using certutil, document their settings in a structured comparison table, and write an analytical explanation of why the differences exist.

> **Prerequisite:** All three certificates must be present before you begin. If the Week 10 TLS certificate is no longer available, contact your instructor before proceeding.

---

## Pre-Lab — Locate All Three Certificates

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready.

### Step 1 — List All Certificates in the Personal Store

Open **PowerShell** on PKI-SRV01 as CORP\pki.admin and run:

```powershell
certutil -store My
```

This lists all certificates in the current user's personal store. You should see at least the code signing certificate from Lab 02 and the TLS certificate from Week 10. Scroll through the output and identify each certificate by its Template or Subject field.

**Week 10 TLS certificate present:**
- [ ] Yes — Thumbprint: ________________
- [ ] No — contact instructor before proceeding

### Step 2 — Locate the Service Account Certificate

The service account certificate was enrolled via the runas session in Lab 01. If it does not appear in the output above, run:

```powershell
certutil -store -service svc.autoenroll My
```

This checks the svc.autoenroll service account's certificate store separately.

### Step 3 — Record All Three Thumbprints

From the certutil outputs above, find and record the thumbprint for each certificate. The thumbprint is a 40-character hex string shown near the bottom of each certificate entry.

| Certificate | Template | Thumbprint |
|-------------|----------|------------|
| TLS (Week 10, Lab 02) | CVI-WebServer | |
| Service Account (Lab 01) | CVI-ServiceAccount | |
| Code Signing (Lab 02) | CVI-CodeSigning | |

---

## Part A — Inspect All Three Certificates

### Step 1 — Inspect Each Certificate Using Its Thumbprint

Run the following command for each certificate, replacing the placeholder with the actual thumbprint. Remove any spaces from the thumbprint before running.

**Certificate 1 — TLS (CVI-WebServer)**

```powershell
certutil -store My "<TLS-thumbprint>"
```

**Full certutil output:**

```
(paste output here)
```

---

**Certificate 2 — Service Account (CVI-ServiceAccount)**

```powershell
certutil -store My "<ServiceAccount-thumbprint>"
```

If the service account certificate is in the service account store rather than the personal store, use:

```powershell
certutil -store -service svc.autoenroll My "<ServiceAccount-thumbprint>"
```

**Full certutil output:**

```
(paste output here)
```

---

**Certificate 3 — Code Signing (CVI-CodeSigning)**

```powershell
certutil -store My "<CodeSigning-thumbprint>"
```

**Full certutil output:**

```
(paste output here)
```

---

### Step 2 — Complete the Comparison Table

Using the certutil outputs above, fill in every cell of the table below. Look for each field in the certutil output — Key Usage and EKU are shown in the Extensions section. No cells should be left blank.

> **Finding the EKU OID:** In the certutil output, look for the line starting with `1.3.6.1.5.5.7.3.` under Enhanced Key Usage — this is the OID. If you can't find it, run: `certutil -store My "<thumbprint>" | findstr OID`

| Field | TLS Certificate | Service Account Certificate | Code Signing Certificate |
|-------|----------------|----------------------------|--------------------------|
| Template Name | CVI-WebServer | CVI-ServiceAccount | CVI-CodeSigning |
| Subject | | | |
| Subject Source | Supplied in request | Build from AD | Build from AD |
| Issuer | | | |
| Key Usage | | | |
| EKU | | | |
| EKU OID(s) | | | |
| Validity Period | | | |
| Serial Number | | | |
| Thumbprint | | | |
| Request ID | | | |

---

## Part B — Written Analysis

This section is the substance of Lab 03. Write in **prose paragraphs — not bullet points**. Each answer should explain the *why*, not just describe what you observed.

**1 — Key Usage**

Each of the three certificates has different Key Usage settings. Explain *why* each certificate has the Key Usage it has. For each, trace the setting back to the cryptographic operation the use case requires.

```
(your written analysis — 2–3 paragraphs)
```

---

**2 — Extended Key Usage (EKU)**

Each certificate has a different EKU. Explain why each certificate has the EKU it has. For each, identify the relying party application that enforces that EKU and what it would do if the EKU were absent or wrong.

```
(your written analysis — 2–3 paragraphs)
```

---

**3 — Subject Name Source**

The TLS certificate uses "Supplied in request" for its subject. The service account and code signing certificates use "Build from Active Directory." Explain why the subject name source is different for the TLS certificate — what is it about the TLS use case that makes AD-supplied subject names impractical?

```
(your written analysis — 1–2 paragraphs)
```

---

**4 — Security Question**

Imagine a single certificate that combined all three EKUs: Server Authentication, Client Authentication, and Code Signing. What security risk would this create? Be specific — what could an attacker do with a compromised private key from this combined certificate that they could not do with any one of the three individual certificates?

```
(your written analysis — 1–2 paragraphs)
```

---

## Reflection

**Which of the three certificates would you consider most critical to revoke quickly if the private key were compromised — and why?**

```
(your answer here — consider the blast radius of each)
```

**What would you add to this comparison if you were presenting it to a security team evaluating the PKI environment?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] Pre-lab: All three thumbprints recorded in the table
- [ ] Pre-lab: Service account cert located (personal store or svc.autoenroll store)
- [ ] Part A: certutil output for all three certificates pasted in full
- [ ] Part A: Comparison table fully completed — no blank cells
- [ ] Part B: Key Usage analysis written in prose — traces each setting to a cryptographic operation
- [ ] Part B: EKU analysis written in prose — identifies the relying party for each
- [ ] Part B: Subject Name source difference explained with reasoning
- [ ] Part B: Security question answered — specific risk identified for a combined-EKU certificate
- [ ] Reflection completed
- [ ] File saved as `lab-03-profile-comparison.md`
- [ ] All three Week 11 lab files committed together with a single meaningful commit message
- [ ] Request IDs for all three certificates recorded — needed for Week 12
- [ ] All three Week 11 labs committed with a single meaningful commit message
- [ ] Request IDs for all three certificates recorded (TLS from Week 10, Service Account, Code Signing) — needed for Week 12
