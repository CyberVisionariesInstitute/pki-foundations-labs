# Week 15 — Student Submission Guide

## Certificate Lifecycle Management at Scale

---

## What to Submit This Week

| Lab | File to Submit | Path in Your Repo |
|---|---|---|
| Lab 01 — Autoenrollment via Group Policy | `lab-01-autoenrollment-gpo.md` | `labs/week-15/lab-01-autoenrollment-gpo.md` |
| Lab 02 — CLM Discovery and Risk Scoring | `lab-02-clm-discovery-risk.md` | `labs/week-15/lab-02-clm-discovery-risk.md` |
| Lab 03 — CLM Policy Zones and Automation | `lab-03-clm-policy-automation.md` | `labs/week-15/lab-03-clm-policy-automation.md` |
| Lab 04 — CLM Monitoring and Reflection | `lab-04-clm-monitoring-reflection.md` **+** exported `clm-report-<date>.txt` | `labs/week-15/lab-04-clm-monitoring-reflection.md` |

---

## Before You Start

Lab 01 requires a published certificate template from Week 10 and a working PKI-SRV01/CLIENT01 domain join — confirm both before starting.

Labs 02–04 all run inside the same tool: the CLM Simulator in your lab portal, at Select Track → CLM Simulator. This is your first time using it, so Lab 02 opens with an orientation section on exactly how to sign in and get there — you don't need any prior familiarity. Your fleet of 15 certificates is seeded once, personalized to your account, and stays the same every time you return — reloading or re-signing in will not generate a new fleet, so don't worry about "losing" your data between labs.

**Lab order matters.** Labs 02, 03, and 04 are a hard sequence — do them in order. Lab 03 depends on the certificates and risk scores you identified in Lab 02. Lab 04's Monitoring tab and required reflection only fully populate after both the Policy/Automation work from Lab 03 and a completed health check have run.

---

## Lab 01 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Access | CORP\pki.admin on PKI-SRV01; CLIENT01 domain-joined in the CVI Workstations OU |
| Tools | Group Policy Management Console, Certificates MMC, certutil, gpupdate/gpresult |
| Prerequisite | Published certificate template from Week 10 |
| Time | 60–75 minutes |

**What Lab 01 produces:**
- Template permissions (Read, Enroll, Autoenroll) granted to the CVI Workstations OU
- A new GPO configuring Certificate Services Client — Auto-Enrollment, linked to the CVI Workstations OU
- `gpupdate`/`gpresult` output from CLIENT01 confirming the GPO applied
- A certificate visible in the Certificates MMC and confirmed via `certutil -viewstore` on the client, matched against a `certutil -view` record on the CA
- A documented, reusable autoenrollment procedure

---

## Lab 02 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Access | CVI Lab Portal student account with CLM Simulator access |
| Tools | CLM Simulator — Discovery tab |
| Time | 45–60 minutes |

**What Lab 02 requires you to do:**
1. Sign in and navigate to Select Track → CLM Simulator, and identify all four tabs before doing anything else
2. Run the Discovery Scan and let your personalized 15-certificate fleet populate
3. Record your top 5 highest-risk certificates and manually verify the risk-score math behind at least one
4. Count how many certificates fall into each risk band and identify at least one certificate with stacked risk factors
5. Predict — before opening the Policy tab — which zone each of your top 5 certificates belongs to

---

## Lab 03 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Access | Same CLM Simulator session/fleet from Lab 02 |
| Tools | CLM Simulator — Policy and Automation tabs |
| Time | 60–75 minutes |

**What Lab 03 requires you to do:**
1. Open the Policy tab and check your Lab 02 zone predictions against the actual assignments
2. Identify the zone with the lowest pass rate and explain the specific violations driving it
3. Predict which flagged certificates can be safely automated before running remediation
4. Run Automated Remediation and compare actual results against your predictions
5. Document the trust decision behind every certificate held for manual review, and confirm ownership status on each

---

## Lab 04 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Access | Same CLM Simulator session/fleet, with Lab 03's Automation already run |
| Tools | CLM Simulator — Monitoring tab and Reflection section |
| Time | 45–60 minutes |

**Lab 04 is the closing lab of the week.** Its Fleet Health Dashboard, expiration pipeline, and alert feed all reflect the cumulative state of your fleet after Discovery, Policy, and Automation — which is why it depends on Labs 02 and 03 being complete first. The reflection section only appears once both Automation and the Monitoring health check have run. Your final deliverable is the exported compliance report (`clm-report-<date>.txt`) — submit it alongside your completed lab markdown, not as a substitute for it.

---

## What Your Lab Reports Must Include

### Lab 01
- [ ] Logged into PKI-SRV01 as CORP\pki.admin; CA running and responding confirmed
- [ ] CVI Workstations OU confirmed and CLIENT01 membership verified
- [ ] Template permissions (Read, Enroll, Autoenroll) configured and documented
- [ ] GPO created with correct Auto-Enrollment configuration and linked to CVI Workstations OU
- [ ] `gpupdate /force` and `gpresult /r` output included, confirming GPO applied to CLIENT01
- [ ] Certificate verified in Certificates MMC and via `certutil -viewstore` on CLIENT01
- [ ] Certificate record confirmed on the CA via `certutil -view`
- [ ] Reusable autoenrollment procedure documented
- [ ] All lab report questions answered, including why CLM still needs autoenrollment underneath it

### Lab 02
- [ ] CLM Simulator access confirmed and Discovery Scan run
- [ ] Fleet size and column structure recorded
- [ ] Top 5 highest-risk certificates documented with exact scores and violations
- [ ] Risk score calculation shown and verified for at least one certificate
- [ ] Risk band counts (Critical/High/Medium/Low) completed for the full fleet
- [ ] Stacking-violation example identified and explained
- [ ] Zone predictions recorded for all top 5 certificates, made before opening the Policy tab
- [ ] All lab report questions answered

### Lab 03
- [ ] All six policy zones reviewed with pass/fail rates documented
- [ ] Lowest-passing zone identified with its failure pattern
- [ ] Lab 02 zone predictions checked against actual assignments, with misses noted
- [ ] Predictions recorded before running Automated Remediation
- [ ] Automated Remediation results (auto-fixed vs. manual-review counts) recorded
- [ ] Specific trust decision documented for every manual-review certificate
- [ ] Ownership status checked for all manual-review certificates
- [ ] All lab report questions answered, including why a high risk score alone never triggers automated reissuance for an untrusted or weak-key certificate

### Lab 04
- [ ] Fleet Health Dashboard reviewed — CRL freshness and OCSP reachability/accuracy documented
- [ ] Expiration pipeline (30/60/90-day) documented
- [ ] Full alert feed recorded and re-ranked by actual response priority, with top 3 explained
- [ ] Soft-fail OCSP risk explained in the context of your own fleet's scale
- [ ] All three reflection questions answered and submitted in the CLM Simulator
- [ ] Compliance report exported and attached alongside the markdown file
- [ ] All lab report questions answered

---

## Key Commands Reference

```powershell
# --- Lab 01: Autoenrollment via Group Policy ---

# Confirm CA is running before starting
certutil -ping

# Grant Autoenroll/Enroll/Read on the template — do this in the Certificate Templates console
# (Security tab → CVI Workstations OU or target group → Read, Enroll, Autoenroll = Allow)

# On CLIENT01 — apply and verify the GPO
gpupdate /force
gpresult /r

# Confirm the autoenrolled certificate from the client side
certutil -viewstore -enterprise My

# On PKI-SRV01 — confirm the matching issuance record
certutil -view -restrict "RequesterName=CORP\CLIENT01$"
```

> **CLM Simulator labs (02–04) have no command-line component** — all work happens in the browser tool at Select Track → CLM Simulator. There's nothing to paste into a terminal; your lab report captures what you observed and calculated in the UI instead.

> **Order matters:** Discovery (Lab 02) → Policy/Automation (Lab 03) → Monitoring/Reflection (Lab 04). Running Automated Remediation before you've documented your Lab 02/03 predictions defeats the point of the exercise — predict first, then compare.

---

## Submitting Your Work

1. Copy the completed lab file(s) into your GitHub portfolio repository under `labs/week-15/`
2. For Lab 04, also copy the exported `clm-report-<date>.txt` into the same folder
3. Commit with a descriptive message (e.g., `"Week 15 Lab 03 - CLM policy zones and automation"`)
4. Push to your repository
5. Paste the GitHub permalink to your submission in the Kajabi assignment field

**Submission format:** submit the completed markdown file with all findings, scores, and questions answered in full sentences — not a blank template with placeholders left in. Lab 04 submissions without the exported compliance report attached are incomplete.

---

*CyberVisionaries Institute | PKI Career Pathway | Phase 2 Operations*
*Cohort 1 · 2026 | cybervisionariesinstitute.org*
