# Week 3 — X.509 Certificate Anatomy Labs

#### Focus: Certificate Structure, Extensions, and Trust Chains

This week moves from cryptographic primitives to the identity layer of PKI.

You will work directly with X.509 certificates, examining how certificates represent identity, define usage rules, and establish trust relationships between systems.

By completing these labs, you will demonstrate practical understanding of:

- **Certificate Structure** → Identity representation
- **Certificate Extensions** → Usage and validation rules
- **Certificate Chains** → Trust relationships
- **Certificate Misconfigurations** → TLS troubleshooting

---

## Lab Overview

| Lab                 | Topic                                       | PKI Concept                         |
|---------------------|---------------------------------------------|-------------------------------------|
| Lab 01              | Inspect Certificate Fields                  | Certificate Structure               |
| Lab 02              | Investigate Certificate Extensions          | SAN, EKU, Key Usage                 |
| Lab 03              | Verify Certificate Chains                   | Root → Intermediate → Leaf          |
| Lab 04              | Detect Certificate Misconfigurations        | TLS Failure Analysis                |
| Lab 05 (Optional)   | Extract Certificate from a Live Website     | Real-World Certificate Inspection   |


---

## Lab Files

Complete each lab in order:

- **Lab 01 — Inspect Core Certificate Fields**
- **Lab 02 — Investigate Certificate Extensions**
- **Lab 03 — Build and Verify a Certificate Chain**
- **Lab 04 — Detect Certificate Misconfigurations**

Optional challenge:
- **Lab 05 — Extract a Certificate from a Live Website**

Each lab builds on the previous one, moving from **reading certificates → understanding certificate usage → validating trust → troubleshooting certificate failures.**

---

## Submission Instructions

### 1. Use the Week 2 submissions folder

Place all generated artifacts inside:
03-week-03-certificate-anatomy/submissions/


Create the following structure if it does not already exist:
submissions/
certificate-inspection/
certificate-extensions/
certificate-chain/
misconfiguration-analysis/
live-certificate-analysis/


---

### 2. Commit Required Artifacts

You must commit:

- Certificate files used for inspection
- Command outputs where applicable
- Notes explaining certificate attributes
- Evidence of certificate chain verification
- Observations about certificate misconfigurations

---

### 3. Artifact Placement Rules

- Use the submissions/ folder for generated certificate artifacts.
- Use the assets/ folder only for screenshots or documentation images.
- Do not submit screenshots unless explicitly requested.
- Clearly label all analysis or explanation files.

---

### 4. Security Requirement

Even when working with certificates:

**Do NOT commit private keys to version control.**

Private keys must always remain protected.

---

## Completion Criteria

You are considered complete when:

- All required artifacts are present
- Certificate fields and extensions are correctly identified
- Certificate chains validate successfully
- You can clearly explain:
  - The purpose of core certificate fields
  - How certificate extensions control usage
  - How trust is established through certificate chains
  - How misconfigured certificates cause TLS failures

---

## What "Good" Looks Like 

- Clear explanations written in your own words
- Correct identification of certificate fields and extensions
- Successful verification of certificate chains
- Ability to recognize certificate misconfigurations
- Clean and organized repository structure

---

## Why This Matters
Certificates are the foundation of **secure identity on the internet.**

Understanding how to read and validate certificates is a critical skill for:

- PKI Engineers
- Security Engineers
- Cloud Security Architects
- DevSecOps Engineers

Everything from **HTTPS websites to cloud authentication systems** depends on certificates working correctly.

---

CVI PKI Career Pathway — Foundations Phase
