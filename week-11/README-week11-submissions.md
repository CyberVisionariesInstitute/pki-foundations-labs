# Week 11 — Student Submission Guide

**PKI Career Pathway · Phase 2 Operations · Cohort 1**

---

## Overview

Week 11 has three lab submissions. All three are markdown files committed to your GitHub portfolio repository.

**Your portfolio repo structure for Week 11:**

```
labs/
└── week-11/
    ├── lab-01-service-account-profile.md
    ├── lab-02-code-signing-certificate.md
    └── lab-03-profile-comparison.md
```

> **Important:** Labs 01 and 02 are independent — you can complete them in either order. Lab 03 requires all three certificates (Week 10 TLS cert + Lab 01 service account cert + Lab 02 code signing cert) to be present and inspectable before you can complete the comparison table.

---

## Lab 01 — Build a Certificate Profile for a Service Account

**Submission file:** `labs/week-11/lab-01-service-account-profile.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab verification | Get-Service CertSvc and certutil -ping outputs in code blocks |
| Part A — Template design | All five design decisions documented with rationale for each choice |
| Part A — Template settings table | Key Usage, EKU, Subject Name, Validity, Enrollment Permissions — each with a reason |
| Part B — Publish and issue | Template published to CVI Issuing CA 1; certificate issued to svc.autoenroll |
| Part B — Enrollment wizard | Steps documented — snap-in context (service account), template selected, issuance outcome |
| Part C — Verification | certutil output pasted in code block; key fields extracted into table |
| Part C — certsrv.msc confirmation | Request ID recorded |
| Part D — Written explanation | Service account cert vs. user cert — what is different and why |

**The rationale in Part A is the most important section.** Do not list settings without reasons. Each setting should be accompanied by a one-to-two sentence explanation of why you chose it for this specific use case.

**Format requirements:**
- All command output in fenced code blocks
- Part A template decisions in structured tables or labeled sections
- Part D in prose paragraphs — not bullet points
- Use the provided lab-01-service-account-profile.md template as your starting structure

---

## Lab 02 — Issue a Code Signing Certificate

**Submission file:** `labs/week-11/lab-02-code-signing-certificate.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab verification | CertSvc and certutil -ping in code blocks |
| Part A — Template design | CVI-CodeSigning template settings documented with rationale |
| Part B — Issue the certificate | Enrollment wizard steps; certificate issued to pki.admin; Request ID recorded |
| Part C — Sign the script | Set-AuthenticodeSignature command and output |
| Part C — Verify the signature | Get-AuthenticodeSignature output — status = Valid |
| Part C — Timestamp check | TimeStamperCertificate field — is a timestamp present? |
| Part C — Hash mismatch test | Script modified; Get-AuthenticodeSignature output — status = HashMismatch |
| Part D — Written explanation | What the Code Signing EKU enforces; what the hash mismatch test demonstrates |

**The hash mismatch test is required.** Paste the actual Get-AuthenticodeSignature output both before and after modification. Do not describe the result — show it.

**Part D is written in prose.** Cover: what the Code Signing EKU enforces at the OS level, and what the hash mismatch test demonstrates about what the signature is actually protecting.

**Format requirements:**
- All command output in fenced code blocks (PowerShell and certutil)
- Part D in prose paragraphs
- Use the provided lab-02-code-signing-certificate.md template as your starting structure

> **Forward note:** The code signing certificate you issue in Lab 02 is one of the three revocation targets in Week 12. Record the Request ID — you will need it.

---

## Lab 03 — Compare Certificate Profiles Side by Side

**Submission file:** `labs/week-11/lab-03-profile-comparison.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab — locate all three certificates | Thumbprints for TLS cert (Week 10), service account cert (Lab 01), code signing cert (Lab 02) |
| Part A — certutil outputs | Full certutil -store My output for all three certificates, in code blocks |
| Part A — Comparison table | Key Usage, EKU, Subject, Subject Source, Validity — all three columns filled |
| Part B — Written analysis | Why each certificate has the Key Usage it has; why each has the EKU it has; why the Subject Source differs |
| Part B — Security question | What would happen if all three EKUs were combined into one certificate? |

**Lab 03 is a synthesis exercise.** The commands are not new. The question is whether you can explain the differences you document. The comparison table provides structure. The Part B written analysis is the substance.

**Part B requirements:**
- Written in prose paragraphs — not bullet points
- Explains *why* each difference exists — not just names it
- Addresses the Subject Source difference: why TLS uses "Supplied in request" while the other two use "Build from AD"
- Answers the security question: what does combining all three EKUs into one certificate create as a risk?

**Format requirements:**
- All certutil outputs in fenced code blocks
- Part B in prose paragraphs
- Use the provided lab-03-profile-comparison.md template

> **Prerequisite:** The Week 10 TLS certificate must be present in the Personal store on PKI-SRV01. If it was deleted, contact the instructor — do not attempt to re-issue it from memory.

---

## Commit Instructions

**Commit message format:**
```
Week 11: <brief description>
```

Examples:
- `Week 11: Service account profile, code signing, and certificate comparison`
- `Week 11: Lab 01–03 complete — CVI-ServiceAccount, CVI-CodeSigning, and profile comparison`

---

### Option 1 — GitHub Web UI

1. Go to your portfolio repository on GitHub.com
2. Navigate to `labs/week-11/` (create the folder if it does not exist — type `labs/week-11/` as part of the file path when creating a new file)
3. Click **Add file → Upload files** or **Add file → Create new file**
4. Upload or paste your lab files
5. Scroll to **Commit changes**
6. Enter your commit message
7. Select **Commit directly to the main branch**
8. Click **Commit changes**

> Upload all three files in one go using **Upload files** and drag-and-drop for a single commit.

---

### Option 2 — VS Code

1. Open VS Code and open your portfolio repo folder
2. Place completed lab files into `labs/week-11/`
3. Open the **Source Control** panel (Ctrl+Shift+G / Cmd+Shift+G)
4. Stage all three files (click **+** next to **Changes**)
5. Enter your commit message
6. Click **✓ Commit**, then **Sync Changes** to push

---

### Option 3 — Git Commands

```bash
cd your-pki-portfolio
mkdir -p labs/week-11
git add labs/week-11/
git commit -m "Week 11: Service account profile, code signing, and certificate comparison"
git push
```

---

## Common Issues

**Lab 01:**

| Issue | Fix |
|-------|-----|
| CVI-ServiceAccount template not visible in service account enrollment wizard | Confirm (1) the template is published to CVI Issuing CA 1 in certsrv.msc; (2) svc.autoenroll has Read permission on the template; (3) you opened the Certificates snap-in for the service account, not your own user account |
| certutil -store returns no results for the service account | Try `certutil -store -service svc.autoenroll My` to target the service account store specifically |
| Template shows as pending instead of issuing | Check the Issuance Requirements tab on CVI-ServiceAccount — "CA certificate manager approval" should be unchecked for Lab 01 |
| certutil -template CVI-ServiceAccount returns "Template not found" | The query uses the internal name (case-sensitive, no spaces). Confirm the exact internal name in the General tab of certtmpl.msc |

**Lab 02:**

| Issue | Fix |
|-------|-----|
| Get-ChildItem -Path Cert:\CurrentUser\My -CodeSigningCert returns nothing | The certificate may not be in the current user store. Try the Certificate snap-in to confirm where it was issued — user account or computer account store. |
| Set-AuthenticodeSignature returns "Cannot sign: the certificate has no private key" | The private key is not accessible to the current user context. Confirm the certificate was issued to pki.admin and you are running PowerShell as pki.admin. |
| Get-AuthenticodeSignature returns "UnknownError" | The signing cert chain may not be trusted in the local store. Run `certutil -verify -urlfetch` on the signing certificate to check chain and revocation. |
| Hash mismatch test does not change the status | Confirm you actually modified and saved the script file before re-running Get-AuthenticodeSignature. Even adding a blank line counts as a modification. |

**Lab 03:**

| Issue | Fix |
|-------|-----|
| Week 10 TLS certificate no longer in the store | Contact the instructor. If the certificate was deleted, a new one can be issued from CVI-WebServer for comparison purposes. |
| certutil -store My returns all certificates, too many to parse | Use `certutil -store My "<thumbprint>"` with the specific thumbprint to target each certificate individually |
| Thumbprint not recorded from previous labs | Open the MMC Certificates snap-in → Personal → Certificates → double-click each cert → Details tab → Thumbprint field |

---

*CyberVisionaries Institute · PKI Career Pathway Phase 2 · Cohort 1 · 2026*
