# Lab — Detect Certificate Misconfigurations

## Goal

This lab builds operational understanding of **common certificate misconfigurations that cause TLS failures.**

You will analyze certificate scenarios and determine why a certificate would fail validation.

These types of misconfigurations are responsible for many real-world outages.

You will practice identifying issues involving:
- Missing **Subject Alternative Name (SAN)**
- Incorrect **Extended Key Usage (EKU)**
- **Expired certificates**
- Broken certificate validation conditions

---

## Part 1 — Setup

### Prerequisites
- Completion of **Lab 01, Lab 02, and Lab 03**
- Understanding of certificate fields and extensions
- Access to your portfolio repository

This lab focuses on **analysis and troubleshooting**, not generating new cryptographic artifacts.

---

## Part 2 — Execution Steps

### Step 1 — Create Artifact Directory
From the root of your directory on your personal machine:
- mkdir -p lab/03-week-03-certificate-anatomy/submissions/misconfiguration-analysis

---

## Part 3 — Misconfiguration Scenarios
Analyze each scenario and determine **why the certificate would fail or cause problems in production systems.**

Document your analysis in a file named:

  misconfiguration-analysis.md

Save it inside:

  labs/03-week-03-certificate-anatomy/submissions/misconfiguration-analysis/

---

## Scenario 1 — Missing Subject Alternative Name
A certificate contains the following information:

  Subject: CN=example.com

However, the certificate **does not contain a Subject Alternative Name (SAN)** extension.

**Question**

Would modern browsers trust this certificate?

Analysis

Explain:
- Why modern browsers require SAN
- Why the Common Name alone is no longer sufficient

---

## Scenario 2 — Incorrect Extended Key Usage
A certificate contains the following extension:

  Extended Key Usage: Client Authentication

The certificate is being used on a **web server for HTTPS.**

### Question

Would a browser accept this certificate for a web server?

### Analysis

Explain:
- What Extended Key Usage defines
- Why the correct EKU is required for TLS servers

---

## Scenario 3 — Expired Certificate
The certificate validity period shows:

Not Before: May 1 2022
Not After:  May 1 2023

The current date is **after May 1, 2023.**

### Question
What happens if this certificate is used today?

### Analysis
Explain:
- Why expiration causes TLS validation to fail
- Why certificate lifecycle management is critical

---

## Scenario 4 — Missing Intermediate Certificate

A server presents a certificate during a TLS handshake, but the **intermediate certificate is not included in the chain.**

### Question

What error would a browser likely display?

### Analysis

Explain:
- How certificate chains establish trust
- Why intermediate certificates must be provided by servers
  
---

## Part 4 — Observations
Your misconfiguration-analysis.md file should explain:
- Why each certificate configuration fails
- What TLS validation rule is being violated
- How the issue could be fixed

---

## Submission (Portfolio Repo)
Ensure the following file exists:

labs/03-week-03-certificate-anatomy/submissions/misconfiguration-analysis/
  misconfiguration-analysis.md

Commit and push your changes.
Example commit message:

  Week 3 Lab 04 — Certificate misconfiguration analysis

---

## Stretch (Optional)
Research a **real certificate outage.**

Examples include:
- Let’s Encrypt DST Root CA expiration
- Microsoft Teams certificate outage
- Slack TLS certificate expiration

Add a short summary explaining:
- What caused the outage
- What certificate issue occurred
- How it was resolved

---

## Why This Matters
Most PKI outages are **not caused by broken cryptography.**

They are caused by **certificate configuration and management mistakes, including:**
- Expired certificates
- Incorrect extensions
- Missing intermediate certificates
- Invalid hostname configuration

Being able to quickly identify these problems is a core skill for **PKI engineers and security professionals.**

---

CVI PKI Career Pathway — Foundations Phase
