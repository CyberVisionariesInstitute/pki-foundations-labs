## The PKI Diagnostic Framework

Before starting any lab, internalize these four steps. You will use them in every lab this week.

| Step | Action |
|---|---|
| **Step 1 — Retrieve** | Get the actual certificate from the failing system using `openssl s_client` or the browser. Don't assume — inspect. |
| **Step 2 — Parse** | Run `openssl x509 -text -noout`. Check validity dates, Subject, SAN, Issuer, and the certificate chain. |
| **Step 3 — Validate the chain** | Confirm the full chain from leaf to root is present and trusted. Identify any missing intermediates. |
| **Step 4 — Check revocation and trust** | Query OCSP if available. Confirm the root CA is in the system trust store. Verify hostname matches SAN. |

---

## Outcomes

By the end of this week, you can:

- Apply the 4-step PKI diagnostic framework to any PKI failure without guessing
- Diagnose an expired certificate and document the remediation path
- Identify a broken certificate chain caused by a missing intermediate CA and explain the fix
- Detect a hostname/SAN mismatch and explain why reusing the existing certificate is not a valid fix
- Use OCSP and CRL tools as part of a structured diagnostic workflow
- Write a clear incident summary — what failed, why it failed, and what the fix requires

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab write-ups | `labs/week-06/submissions/` |
| Lesson notes | `notes/week-06-key-concepts.md` |
| Reflection | `reflections/week-06.md` |

Before starting, copy the following templates from the course repository into your portfolio repo:

- `_templates/lab-submission-template.md`
- `_templates/lesson-notes-template.md`
- `_templates/reflection-template.md`

---

## Checklist

- [ ] Review lesson material and internalize the 4-step diagnostic framework
- [ ] Lab 01 — Connect to the expired certificate target and retrieve the certificate
- [ ] Lab 01 — Run the 4-step framework and identify the failure
- [ ] Lab 01 — Document the remediation steps in your submission file
- [ ] Lab 02 — Encounter the broken chain error and identify the missing intermediate
- [ ] Lab 02 — Locate the correct intermediate from the CA's public repository
- [ ] Lab 02 — Validate the repaired chain and document your findings
- [ ] Lab 03 — Inspect the SAN mismatch certificate and reproduce the validation error
- [ ] Lab 03 — Explain why the correct fix is a new certificate, not a DNS alias workaround
- [ ] Lab 04 (Stretch) — Receive the multi-failure scenario and apply the full 4-step framework
- [ ] Lab 04 (Stretch) — Identify all failures in diagnostic order and write a structured incident report
- [ ] Complete lesson notes
- [ ] Write reflection for Week 6
- [ ] Submit all labs in the CVI Lab Tracker
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- Every lab submission references the 4-step framework — not just the answer, but how you got there
- Expired certificate correctly identified using `openssl x509 -noout -dates` with clear articulation of the remediation path
- Broken chain: missing intermediate identified, its role in the chain explained, repaired chain validated with `openssl verify`
- SAN mismatch: the specific mismatch documented (what the SAN says vs. what was accessed), and correct remediation explained — not just "get a new cert" but why the existing cert cannot be reused
- Incident summaries are clear enough that a non-PKI engineer could understand what happened and what was done to fix it
- Repository structure is clean with artifacts in the correct subfolders

---

## Connecting to Prior Weeks

The diagnostic framework this week builds on everything you have learned in Phase 1:

- **Week 3** — X.509 anatomy: you need to read certificate fields to diagnose failures
- **Week 4** — Formats: OpenSSL commands you practiced are the primary diagnostic tools
- **Week 5** — Lifecycle: expiration and revocation failures are lifecycle failures at their root

When a PKI failure hits a production system, the engineer who fixes it fastest is the one who has internalized the certificate structure, knows the right commands, and follows a framework rather than guessing. That is what this week builds.

---

*CVI PKI Career Pathway — Foundations Phase*
