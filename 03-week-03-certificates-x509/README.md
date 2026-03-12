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

- labs/03-week-03-certificate-anatomy/
- notes/week-03-key-concepts.md
- reflections/week-03.md

Place generated cryptographic artifacts inside:


labs/03-week-03-certificate-anatomy/submissions/


Required artifact folders:


submissions/
certificate-inspection/
certificate-extensions/
certificate-chain/
misconfiguration-analysis/


---

## Artifact Rules

- Commit actual certificate files or outputs where applicable.
- Use the assets/ folder only for documentation images if needed.
- Do not modify provided certificate artifacts unless instructed.
- Cearly label any notes or analysis related to the certificate inspection.
- Private keys must never be stored in version control.

---

## Checklist

- [ ] Review lesson material
- [ ] Inspect certificate structure using OpenSSL
- [ ] Identify key certificate fields and extensions
- [ ] Analyze SAN, Key Usage, EKU, and Basic Constraints
- [ ] Verify a certificate chain
- [ ] Document findings in notes
- [ ] Write reflection for Week 3
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- Correct identification of certificate fields and extensions
- Clear explanations of certificate attributes in your own words
- Successful validation of a certificate chain
- Ability to identify certificate misconfigurations
- Organized repository structure with clearly labeled artifacts

---

CVI PKI Career Pathway — Foundations Phase
