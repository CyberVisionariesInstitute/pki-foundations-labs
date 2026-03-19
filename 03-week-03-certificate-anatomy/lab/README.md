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

## Deliverables (Submit in Your Portfolio Repo)

Update the following areas of your repository:

- `labs/week-03/`
- `notes/week-03-key-concepts.md`
- `reflections/week-03.md`

Before starting the labs, retrieve the templates from the `pki-foundations-labs/_templates` directory in the course repository.

Copy the following files into your portfolio repository:

- `_templates/lab-submission-template.md`
- `_templates/lesson-notes-template.md`
- `_templates/reflection-template.md`

Use them to create the following files in your repo:

**Lesson Notes**
`notes/week-03-key-concepts.md`

**Reflection**
`reflections/week-03.md`

**Lab Submissions**
Create your lab write-ups inside each lab folder and place completed files in:

`labs/week-03/submissions/`

Your submissions folder should contain:

```
labs/
  week-03/
    submissions/
      leaf_cert.pem
      lab-01-certificate-fields.md
      lab-02-certificate-extensions.md
      server.pem
      intermediate.pem
      root.pem
      lab-03-certificate-chain.md
      lab-04-certificate-misconfigurations.md
      github_cert.pem                      ← Lab 05 only (optional)
      lab-05-extract-live-certificate.md   ← Lab 05 only (optional)
```

---

## Artifact Rules

- Commit actual certificate files or outputs where applicable.
- Use the `assets/` folder only for documentation images if needed.
- Do not modify provided certificate artifacts unless instructed.
- Clearly label any notes or analysis related to the certificate inspection.
- **Private keys must never be stored in version control.**

---

## Checklist

- [ ] Review lesson material
- [ ] Inspect certificate structure using OpenSSL
- [ ] Identify key certificate fields and extensions
- [ ] Analyze SAN, Key Usage, EKU, and Basic Constraints
- [ ] Verify a certificate chain
- [ ] Document findings in notes
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
