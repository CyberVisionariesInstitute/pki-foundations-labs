# Week 5 — Certificate Lifecycle Management

## Focus

This week focuses on how certificates are born, managed, and retired — and why getting this wrong causes real outages.

You will trace every step of the certificate issuance workflow from key generation to signed certificate, understand the difference between renewal and replacement, and learn how revocation works in practice. The week closes with an honest look at why certificate expiration still takes down major organizations despite being entirely preventable.

Through hands-on labs, you will generate your own CSR, query a live OCSP responder, and simulate a certificate expiration scenario so you understand what to look for and how to respond.

---

## Outcomes

By the end of this week, you can:

- Walk through the certificate issuance workflow step by step — from key generation to signed certificate
- Create a Certificate Signing Request (CSR) and inspect its fields
- Distinguish between renewal and replacement, and identify when each is appropriate
- Check a certificate's revocation status using OCSP
- Locate the CRL Distribution Point in a certificate's extensions
- Explain why certificate expiration causes enterprise outages despite being predictable
- Describe how organizations inventory and track certificates at scale
- Simulate a certificate expiration scenario and practice the replacement workflow (Stretch)

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab write-ups | `labs/week-05/submissions/` |
| Lesson notes | `notes/week-05-key-concepts.md` |
| Reflection | `reflections/week-05.md` |

Before starting, copy the following templates from the course repository into your portfolio repo:

- `_templates/lab-submission-template.md`
- `_templates/lesson-notes-template.md`
- `_templates/reflection-template.md`

---

## Checklist

- [ ] Review lesson material
- [ ] Lab 01 — Generate a CSR and simulate the certificate issuance workflow
- [ ] Lab 01 — Inspect CSR fields and connect them to X.509 structure from Week 3
- [ ] Lab 02 — Retrieve a live certificate and locate its OCSP extension
- [ ] Lab 02 — Query the OCSP responder and interpret the response
- [ ] Lab 02 — Locate and document the CRL Distribution Point
- [ ] Lab 03 (Stretch) — Create a short-lived certificate and observe expiration behavior
- [ ] Lab 03 (Stretch) — Practice detection and replacement commands
- [ ] Complete lesson notes
- [ ] Write reflection for Week 5
- [ ] Submit all labs in the CVI Lab Tracker
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- CSR fields correctly map to the Subject and SAN fields you identified in Week 3
- Clear explanation of what each CSR field communicates to the CA
- Successful OCSP query with correct interpretation of the response status
- CRL Distribution Point located and documented with an explanation of how CRL checking differs from OCSP
- For the Stretch lab: expired certificate correctly identified using `openssl x509 -checkend`, replacement workflow documented step by step
- Organized repository structure with artifacts in the correct subfolders

---

*CVI PKI Career Pathway — Foundations Phase*
