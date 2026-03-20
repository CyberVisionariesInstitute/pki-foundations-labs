# Week 3 — X.509 Certificate Anatomy

## Focus

This week focuses on learning how to read and interpret X.509 certificates used in Public Key Infrastructure systems.

You will examine the structure of digital certificates and learn how certificate fields and extensions define identity, trust relationships, and permitted usage.

Through hands-on analysis, you will inspect real certificates, analyze certificate extensions, and understand how certificate chains establish trust across systems.

---

## Outcomes

By the end of this week, you can:

- Identify the core fields of an X.509 certificate
- Explain the purpose of certificate extensions
- Interpret Subject Alternative Name (SAN), Key Usage, and Extended Key Usage
- Distinguish between root, intermediate, and leaf certificates
- Validate a certificate chain using OpenSSL
- Recognize common certificate misconfigurations that cause TLS failures

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab write-ups | `labs/week-03/submissions/` |
| Lesson notes | `notes/week-03-key-concepts.md` |
| Reflection | `reflections/week-03.md` |

Before starting, copy the following templates from the course repository into your portfolio repo:

- `_templates/lab-submission-template.md`
- `_templates/lesson-notes-template.md`
- `_templates/reflection-template.md`

---

## Checklist

- [ ] Review lesson material
- [ ] Inspect certificate structure using OpenSSL
- [ ] Identify key certificate fields and extensions
- [ ] Analyze SAN, Key Usage, EKU, and Basic Constraints
- [ ] Verify a certificate chain
- [ ] Complete lesson notes
- [ ] Write reflection for Week 3
- [ ] Submit all labs in the CVI Lab Tracker
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- Correct identification of certificate fields and extensions
- Clear explanations of certificate attributes in your own words
- Successful validation of a certificate chain
- Ability to identify certificate misconfigurations
- Organized repository structure with clearly labeled artifacts

---

*CVI PKI Career Pathway — Foundations Phase*
