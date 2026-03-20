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

## Deliverables (Submit in Your Portfolio Repo)

Update the following areas of your repository:

- `labs/04-week-04-certificate-formats/`
- `notes/week-04-key-concepts.md`
- `reflections/week-04.md`

Before starting the labs, retrieve the templates from the `pki-foundations-labs/_templates` directory in the course repository.

Copy the following files into your portfolio repository:

Templates to copy:

- `_templates/lab-submission-template.md`
- `_templates/lesson-notes-template.md`
- `_templates/reflection-template.md`

Use them to create the following files in your repo:

**Lesson Notes**
`notes/week-04-key-concepts.md`

**Reflection**
`reflections/week-04.md`

**Lab Submissions**
Create your lab write-ups using the lab submission template inside each lab folder.

Place generated cryptographic artifacts inside:

`labs/04-week-04-certificate-formats/submissions/`

Required artifact folders:

- `submissions/convert-formats/`
- `submissions/trust-store-inspection/`
- `submissions/install-validate/` *(Stretch — Lab 03 only)*

---

## Artifact Rules

- Commit actual certificate files and conversion outputs where applicable.
- Use the `assets/` folder only for documentation images and screenshots.
- Private keys must never be stored in version control — add `*.key` to your `.gitignore`.
- Do not commit `test_key.pem` or any other private key files generated during the labs.
- Clearly label all artifacts by lab in their respective subfolders.

---

## Checklist

- [ ] Review lesson material
- [ ] Lab 01 — Convert a certificate between PEM, DER, and PFX formats
- [ ] Lab 02 — Locate and inspect the trust store on your operating system
- [ ] Lab 02 — Validate a live certificate against your system trust store
- [ ] Lab 03 (Stretch) — Install a self-signed root CA and validate a signed certificate
- [ ] Lab 03 (Stretch) — Remove the test root CA after completing the lab
- [ ] Document findings in notes
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
