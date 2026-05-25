# Week 12 — Student Submission Guide

**PKI Career Pathway · Phase 2 Operations · Cohort 1**

---

## Overview

Week 12 has three lab submissions. Labs 01 and 02 are required. Lab 03 is a stretch lab — optional but strongly recommended.

**Your portfolio repo structure for Week 12:**

```
labs/
└── week-12/
    ├── lab-01-revoke-and-crl-propagation.md
    ├── lab-02-ocsp-responder.md
    └── lab-03-crl-failure-simulation.md   (stretch)
```

> **Important: Complete labs in order.** Lab 02 requires a revoked certificate from Lab 01. Lab 03 requires a working OCSP responder from Lab 02. Do not skip ahead.

---

## Lab 01 — Revoke a Certificate and Observe CRL Propagation

**Submission file:** `labs/week-12/lab-01-revoke-and-crl-propagation.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab verification | Get-Service CertSvc, certutil -ping, and certutil -view output in code blocks |
| Part A — Certificate selection | Common Name, Serial Number, and rationale for your choice |
| Part B — Revocation | Reason code selected with written rationale; revocation timestamp from certsrv.msc |
| Part C — CRL publication | certutil -CRL output confirming publication |
| Part D — CRL inspection | CDP URL from the certificate; certutil -dump output showing ThisUpdate and NextUpdate; confirmed serial number in CRL |
| Part E — Verification | certutil -verify output showing the certificate is revoked |
| Part F — Lab report | All five questions answered |

**The revocation reason code rationale is a required field — not optional context.** Explain why you chose the reason you chose. If you chose Superseded, explain what is being superseded and why Key Compromise would have been the wrong choice. If you chose Cessation of Operation, explain what ceased.

**The certutil -dump CRL output must include both ThisUpdate and NextUpdate.** These timestamps are the evidence that the CRL is fresh. Without them, the submission is incomplete.

**Format requirements:**
- All command output in fenced code blocks
- Part F in prose paragraphs — not bullet points
- Use the provided lab-01-revoke-and-crl-propagation.md template

> **Forward note:** The revoked certificate from Lab 01 is the test case for Lab 02. Record the Serial Number before proceeding.

---

## Lab 02 — Configure and Test the OCSP Responder

**Submission file:** `labs/week-12/lab-02-ocsp-responder.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab verification | CertSvc and OCSPSvc running; revoked certificate from Lab 01 confirmed |
| Part A — Role installation | Get-Service OCSPSvc output confirming Online Responder is installed and running |
| Part B — Revocation configuration | Revocation configuration name; CA certificate selected; Online Responder snap-in status indicator (green/yellow/red) |
| Part C — Signing certificate | OCSP signing certificate subject; certutil -store My output; verification table (EKU, NoCheck, Issuer, Validity) |
| Part D — OCSP testing | OCSP URL from AIA extension; certutil -URL output for valid certificate (Good); certutil -URL output for revoked certificate (Revoked); certutil -verify output for both certificates |
| Part E — AIA inspection | CA Issuers URL and OCSP URL identified from AIA extension; one-sentence explanation of each |
| Part F — Lab report | All five questions answered |

**Both OCSP test results are required — Good and Revoked.** The submission is not complete with only one. Paste the full certutil -URL output for each.

**The OCSP signing certificate verification table must have all five properties filled in.** If any property is missing (EKU not present, NoCheck not present), document what you found and what you did to resolve it.

**Format requirements:**
- All command output in fenced code blocks
- Part F in prose paragraphs
- Use the provided lab-02-ocsp-responder.md template

---

## Lab 03 — Simulate a CRL Publication Failure *(Stretch)*

**Submission file:** `labs/week-12/lab-03-crl-failure-simulation.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab | Original CRL publication settings recorded (CRLPeriod, CRLPeriodUnits, Delta values) |
| Part A — Create the failure | Registry change commands and output; certutil -getreg confirming new period; CRL NextUpdate before it expires |
| Part B — Observe the failure | certutil -verify output (hard-fail or soft-fail determination); certutil -URL output showing OCSP behavior after CRL staleness; Event Viewer output with Event IDs and timestamps |
| Part C — Diagnose | Root cause statement; diagnostic steps in order; hard-fail vs. soft-fail determination with evidence |
| Part D — Restore | Registry restoration commands; certutil -CRL output; certutil -dump showing new NextUpdate in the future; certutil -verify confirming restoration |
| Incident report | All three sections complete: Failure / Detection / Resolution |
| Part F — Reflection | All three questions answered |

**The incident report is the primary deliverable for Lab 03.** Each section — Failure, Detection, Resolution — must be complete, in past tense, and written as if for a real operations team. Commands should be listed under Resolution, not implied. Timestamps should be recorded where available.

**Hard-fail vs. soft-fail determination requires evidence.** State what you observed and explain what that observation tells you about the environment's behavior — not just which label applies.

**The original CRL publication settings must be confirmed restored before you submit.** Include certutil -getreg output after restoration.

**Format requirements:**
- All command output in fenced code blocks
- Incident report in prose (past tense; three labeled sections)
- Reflection questions in prose paragraphs
- Use the provided lab-03-crl-failure-simulation.md template

> **Prerequisite:** Lab 03 requires Labs 01 and 02 to be complete. You need a revoked certificate (Lab 01) and a working Online Responder (Lab 02) to observe the full failure mode.

---

## Commit Instructions

**Commit message format:**
```
Week 12: <brief description>
```

Examples:
- `Week 12: CRL revocation, OCSP responder, and failure simulation`
- `Week 12: Labs 01 and 02 complete — revocation and OCSP`
- `Week 12: All three labs complete including CRL failure incident report`

---

### Option 1 — GitHub Web UI

1. Go to your portfolio repository on GitHub.com
2. Navigate to `labs/week-12/` (create the folder if it does not exist — type `labs/week-12/` as part of the file path when creating a new file)
3. Click **Add file → Upload files** or **Add file → Create new file**
4. Upload or paste your lab files
5. Scroll to **Commit changes**
6. Enter your commit message
7. Select **Commit directly to the main branch**
8. Click **Commit changes**

---

### Option 2 — VS Code

1. Open VS Code and open your portfolio repo folder
2. Place completed lab files into `labs/week-12/`
3. Open the **Source Control** panel (Ctrl+Shift+G / Cmd+Shift+G)
4. Stage all files (click **+** next to **Changes**)
5. Enter your commit message
6. Click **✓ Commit**, then **Sync Changes** to push

---

### Option 3 — Git Commands

```bash
cd your-pki-portfolio
mkdir -p labs/week-12
git add labs/week-12/
git commit -m "Week 12: CRL revocation, OCSP responder, and CRL failure simulation"
git push
```

---

## Common Issues

**Lab 01:**

| Issue | Fix |
|-------|-----|
| No certificates appear in certsrv.msc → Issued Certificates | Confirm the correct CA is selected in the console. Expand CVI Issuing CA 1 → Issued Certificates. If none appear, the certificate may have been issued in a prior session under a different account — try filtering by Requester Name. |
| Revoke Certificate option is greyed out | You must right-click the specific certificate, not the folder. Confirm you are logged in as pki.admin on PKI-SRV01. |
| certutil -CRL returns "Access is denied" | Run the command from an elevated (Administrator) PowerShell prompt while logged in as pki.admin. |
| Revoked serial number does not appear in CRL after publication | Confirm you published after revoking (not before). Also confirm you are inspecting the correct CRL file — use the CDP URL from the actual certificate to find the right file name. |
| certutil -verify shows certificate as valid even after revocation | You may need to clear the CRL cache. Run `certutil -urlcache CRL delete` and retry. |

**Lab 02:**

| Issue | Fix |
|-------|-----|
| Online Responder snap-in not available after role installation | Try launching it from Administrative Tools, or add the snap-in manually via mmc → Add/Remove Snap-ins. A server restart may be required. |
| Revocation configuration status shows Red | Check whether the OCSP signing certificate was issued. Open the revocation configuration and look for a signing certificate entry — if blank, the template may not be published or the responder may not have enroll permission. |
| OCSP signing certificate missing OCSP Signing EKU | The wrong template was used. The OCSP Response Signing template (not a custom one) should be selected automatically. Check the revocation configuration wizard — it may have selected a different template. |
| certutil -URL returns timeout | The Online Responder service (OCSPSvc) may not be running, or a Windows Firewall rule is blocking port 80 inbound on PKI-SRV01. Run `Get-Service OCSPSvc` and check Windows Firewall. |
| certutil -URL returns Good for the revoked certificate | The OCSP responder is reading an old CRL that does not include the revocation. Confirm that you published a new CRL in Lab 01 after revoking. Publish again with certutil -CRL and retest. |

**Lab 03:**

| Issue | Fix |
|-------|-----|
| CRL publication period change does not take effect | Confirm you restarted CertSvc after changing the registry values. The registry change alone does not update the running CA configuration. |
| certutil -verify still shows clean verification after CRL expires | Your environment may be soft-fail. This is the expected observation — document it as your hard-fail vs. soft-fail finding. |
| OCSP responder still returns Good after CRL expires | There is a delay between CRL expiry and the Online Responder detecting the stale CRL. Wait a few minutes and retest. Document the delay as part of your failure observation. |
| Event ID 47 not appearing in Event Viewer | Event ID 47 appears when the CA attempts publication and fails, not when the CRL simply expires. The extended publication period means the CA may not attempt publication during the lab session. If Event ID 47 does not appear, document this — it is part of the failure behavior: the CA does not report a problem because it is not trying to publish yet. |
| Cannot restore original CRL settings | If you did not record the original values in the pre-lab section, the defaults are: CRLPeriodUnits = 1, CRLPeriod = Weeks, CRLDeltaPeriodUnits = 1, CRLDeltaPeriod = Days. Restore these values and restart CertSvc. |

---

*CyberVisionaries Institute · PKI Career Pathway Phase 2 · Cohort 1 · 2026*
