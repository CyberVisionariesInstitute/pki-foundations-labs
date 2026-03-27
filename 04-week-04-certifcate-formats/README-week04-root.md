# Week 4 — Certificate Formats and Trust Stores

## Focus

This week focuses on understanding how certificates are encoded, stored, and trusted across different operating systems and environments.

You will work with real certificate files in multiple formats, convert between them using OpenSSL, and explore how your operating system decides which Certificate Authorities to trust by default.

Through hands-on labs, you will gain practical experience with the certificate lifecycle — from format conversion to trust store inspection to installing and validating a self-signed root CA.

---

## Outcomes

By the end of this week, you can:

- Distinguish between PEM, DER, and PFX certificate formats
- Convert certificates between formats using OpenSSL
- Locate and inspect the trust store on your operating system
- Identify trusted root Certificate Authorities installed on your system
- Validate a certificate chain against a trust store
- Explain why trust stores matter in real-world PKI systems
- Install and remove a test root CA from your system trust store (Stretch)

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab write-ups | `labs/week-04/submissions/` |
| Lesson notes | `notes/week-04-key-concepts.md` |
| Reflection | `reflections/week-04.md` |

Before starting, copy the following templates from the course repository into your portfolio repo:

- `_templates/lab-submission-template.md`
- `_templates/lesson-notes-template.md`
- `_templates/reflection-template.md`

---

## Checklist

- [ ] Review lesson material
- [ ] Lab 01 — Convert a certificate between PEM, DER, and PFX formats
- [ ] Lab 02 — Locate and inspect the trust store on your operating system
- [ ] Lab 02 — Validate a live certificate against your system trust store
- [ ] Lab 03 (Stretch) — Install a self-signed root CA and validate a signed certificate
- [ ] Lab 03 (Stretch) — Remove the test root CA after completing the lab
- [ ] Complete lesson notes
- [ ] Write reflection for Week 4
- [ ] Submit all labs in the CVI Lab Tracker
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- Correct conversion between PEM, DER, and PFX with validation at each step
- Clear explanation of when and why each format is used in practice
- Trust store inspection that identifies specific root CAs with Subject and expiration details
- Explanation of how pre-installed root CAs enable browser trust without prior contact
- For the Stretch lab: successful install, validation, and clean removal of the test root CA
- Organized repository structure with artifacts in the correct subfolders

---

*CVI PKI Career Pathway — Foundations Phase*
