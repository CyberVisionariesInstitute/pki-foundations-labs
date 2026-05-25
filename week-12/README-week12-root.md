# Week 12 — Revocation Management & CRL/OCSP Operations

## Focus

Weeks 10 and 11 built the issuance side of PKI operations. Week 12 addresses what comes after issuance — specifically, what happens when a certificate needs to stop being trusted before its natural expiration.

Revocation is how a CA administrator tells the world: this certificate is no longer trustworthy, even though its NotAfter date has not passed. The mechanism that communicates that decision is the Certificate Revocation List — a signed, published list of revoked serial numbers — and the Online Certificate Status Protocol, which provides real-time status queries against the same revocation data. Both are what you build and operate this week.

Lesson 1 covers the CRL workflow: revocation reason codes, the two-step marking-and-publishing process, CRL publication schedules and Delta CRLs, and the CRL Distribution Point extension that tells relying parties where to look. Lesson 2 covers the OCSP responder: what it is, what the OCSP signing certificate requires, how to configure and test it. Lesson 3 covers failure modes — stale CRLs, unreachable responders, soft-fail versus hard-fail behavior — and the diagnostic steps that isolate the root cause. Lesson 4 covers revocation at enterprise scale: the policy framework, automation approaches for CRL publication, and how revocation integrates with the monitoring work coming in Week 14.

Three labs follow. Lab 01 revokes a certificate from Weeks 10 or 11 and traces the full CRL propagation workflow. Lab 02 configures the AD CS Online Responder and tests it against both a valid certificate and the revoked certificate from Lab 01. Lab 03 — the stretch lab — deliberately simulates a CRL publication failure and asks you to document it as a structured incident report.

Week 13 is CA Backup and Recovery. Come to it with all three Certificate Request IDs recorded from this week.

---

## Outcomes

By the end of this week, you can:

- Revoke a certificate in AD CS specifying a reason code — and explain why the reason code matters operationally
- Publish a CRL manually using the CA console and certutil -CRL — and explain why the publication step is distinct from the revocation step
- Locate the CRL Distribution Point extension in a certificate and confirm that your revoked certificate appears in the published CRL
- Configure the AD CS Online Responder and issue the OCSP signing certificate with the correct EKU and NoCheck extension
- Query OCSP status using certutil -URL and interpret Good, Revoked, and Unknown responses
- Diagnose the three common revocation infrastructure failure modes — stale CRL, unreachable OCSP responder, inaccessible distribution point — using certutil and Event Viewer
- Explain the difference between soft-fail and hard-fail revocation checking and describe the security implications of each
- Draft a revocation policy for corp.cvilab.local: triggers, authorization, and response SLA tiers

---

## The Phase 2 Environment

The environment is unchanged from Weeks 9–11. Resolve any environment issues before starting Lab 01.

| Component | Detail | Notes |
|---|---|---|
| Domain | corp.cvilab.local | Windows Active Directory domain |
| DC01 | Domain Controller — runs AD DS and DNS | Start first. Wait for Server Manager to fully load. |
| PKI-SRV01 | Enterprise Issuing CA — runs CVI Issuing CA 1 | Start second. Log in as CORP\pki.admin. |
| Root-CA | Offline Root CA — not domain-joined | Leave powered off. Not required this week. |
| CORP\pki.admin | CA Administrator | All CA console work, certutil commands, Online Responder configuration |
| CORP\cert.manager | Certificate Manager | Not required this week |
| CORP\svc.autoenroll | Service Account | Do not revoke this account's certificate — it is used in Week 15 |
| CA Hierarchy | CVI Root CA (offline) → CVI Issuing CA 1 (PKI-SRV01) | All revocation work happens on PKI-SRV01 |

> **Important for Lab 01:** You need at least one issued certificate from Weeks 10 or 11 to revoke. If the service account certificate (svc.autoenroll) or the code signing certificate (pki.admin) is available in the Issued Certificates view of certsrv.msc, either can be used. Do **not** revoke the svc.autoenroll certificate — use the code signing certificate or a TLS certificate instead, since svc.autoenroll is referenced in Week 15.

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Revoke a Certificate and Observe CRL Propagation | `labs/week-12/lab-01-revoke-and-crl-propagation.md` |
| Lab 02 — Configure and Test the OCSP Responder | `labs/week-12/lab-02-ocsp-responder.md` |
| Lab 03 — Simulate a CRL Publication Failure *(Stretch)* | `labs/week-12/lab-03-crl-failure-simulation.md` |

All are markdown files committed to your GitHub portfolio repo under `labs/week-12/`. Each lab has its own submission template in the `/Labs/` folder — use it.

> **Lab sequencing matters:** Lab 02 uses the revoked certificate from Lab 01 to test the Revoked OCSP response. Lab 03 uses both a valid certificate and a working OCSP responder. Complete labs in order: 01 → 02 → 03.

---

## Checklist

**Pre-lab (all labs):**
- [ ] Start DC01 first and wait for Server Manager to fully load before starting PKI-SRV01
- [ ] Confirm CertSvc is running: `Get-Service CertSvc`
- [ ] Confirm CA is accepting connections: `certutil -ping`
- [ ] Locate at least one issued certificate from Weeks 10 or 11 in certsrv.msc → Issued Certificates

---

**Lab 01 — Revoke a Certificate and Observe CRL Propagation:**
- [ ] Select a certificate to revoke — document the Common Name and Serial Number
- [ ] Do NOT revoke the svc.autoenroll service account certificate (needed for Week 15)
- [ ] Open certsrv.msc → Issued Certificates → right-click → All Tasks → Revoke Certificate
- [ ] Select revocation reason code — document your choice with rationale
- [ ] Confirm certificate appears in Revoked Certificates view — record the revocation timestamp
- [ ] Publish a new CRL: certsrv.msc → right-click CA name → All Tasks → Publish
- [ ] Confirm publication with certutil -CRL — paste output
- [ ] Locate the CDP URL in the certificate using certutil -dump
- [ ] Inspect the CRL at the distribution point using certutil -dump on the CRL file
- [ ] Record ThisUpdate and NextUpdate from the CRL
- [ ] Confirm revoked certificate serial number appears in the CRL
- [ ] Run certutil -verify on the revoked certificate — confirm revocation error
- [ ] Answer all five lab report questions
- [ ] Commit `lab-01-revoke-and-crl-propagation.md` to portfolio repo

---

**Lab 02 — Configure and Test the OCSP Responder:**
- [ ] Install Online Responder role service from Server Manager on PKI-SRV01
- [ ] Confirm OCSPSvc is running: `Get-Service OCSPSvc`
- [ ] Open Online Responder snap-in and create a revocation configuration for CVI Issuing CA 1
- [ ] Confirm OCSP signing certificate is issued — check Online Responder snap-in for green status
- [ ] Verify signing certificate properties: OCSP Signing EKU (1.3.6.1.5.5.7.3.9) and id-pkix-ocsp-nocheck
- [ ] Identify OCSP URL in AIA extension using certutil -dump on a certificate
- [ ] Test valid certificate: certutil -URL → select OCSP → Retrieve → confirm Good response
- [ ] Test revoked certificate (from Lab 01): certutil -URL → select OCSP → Retrieve → confirm Revoked response
- [ ] Run certutil -verify on both certificates — document both outputs
- [ ] Identify CA Issuers URL and OCSP URL from the AIA extension
- [ ] Answer all five lab report questions
- [ ] Commit `lab-02-ocsp-responder.md` to portfolio repo

---

**Lab 03 — Simulate a CRL Publication Failure (Stretch):**
- [ ] Record original CRL publication settings before making any changes
- [ ] Extend CRL publication period in registry + restart CertSvc to create the failure
- [ ] Confirm new extended CRL period: certutil -getreg CA\CRLPeriod
- [ ] Record current CRL NextUpdate — note when it will expire
- [ ] Observe failure behavior: certutil -verify output (hard-fail or soft-fail?)
- [ ] Test OCSP behavior after CRL becomes stale (expect Unknown)
- [ ] Check Event Viewer for relevant event IDs (46, 47, OnlineResponder source)
- [ ] Document root cause in diagnostic section
- [ ] Restore original CRL period settings in registry + restart CertSvc
- [ ] Force CRL publication: certutil -CRL
- [ ] Confirm NextUpdate is in the future
- [ ] Confirm certutil -verify and OCSP return correct results after restoration
- [ ] Write structured incident report: Failure / Detection / Resolution
- [ ] Answer all three reflection questions
- [ ] Commit `lab-03-crl-failure-simulation.md` to portfolio repo

---

## What "Good" Looks Like

- Lab 01's revocation reason choice is explained as an operational decision, not just a selection. "I chose Superseded because I am replacing this certificate, not because the key was compromised. If I had chosen Key Compromise, the response SLA would require immediate CRL publication — Superseded can wait for the next scheduled cycle."
- certutil -dump outputs in Lab 01 and Lab 02 are pasted in full and then interpreted. The key fields — ThisUpdate, NextUpdate, serial number — are called out with explanations of what they mean to a relying party.
- Lab 02's signing certificate inspection shows both required properties — OCSP Signing EKU and id-pkix-ocsp-nocheck — with an explanation of why each is required.
- Lab 03's incident report reads as if it were written for a real operations team — past tense, specific timestamps, specific commands, specific event IDs. "The failure began at [timestamp] when the CRL publication period was extended..."
- Lab 03 clearly identifies whether the environment exhibits hard-fail or soft-fail behavior and explains what observable evidence led to that determination.

---

## Connecting to Prior Weeks

- **Week 5** — In Phase 1, you queried CRLs and checked revocation status from the outside. This week, you operate the infrastructure that produces that information. The certutil commands are the same — the direction of the work has reversed.
- **Week 6** — The PKI incident diagnosis framework from Phase 1 applies directly to Lab 03. The structured incident report uses the same failure-detection-resolution structure.
- **Week 10** — The TLS certificate from Week 10 Lab 02 can be used as the valid certificate in Lab 02 OCSP testing. Do not delete it.
- **Week 11** — The code signing certificate from Lab 02 is a suitable revocation candidate for Lab 01. The service account certificate (svc.autoenroll) should NOT be revoked — it is needed in Week 15.
- **Week 13** — CA Backup and Recovery. The CA Compromise revocation reason code — the most severe — requires a full CA recovery. Week 13 makes that scenario operational. Come to Week 13 with Lab 01 complete.
- **Week 14** — Monitoring and Alerting. CRL expiry alerts and OCSP responder health checks are the monitoring controls that prevent the failure you simulated in Lab 03. Week 14 builds directly on the infrastructure you operated this week.

---

*CVI PKI Career Pathway — Phase 2 Operations*
