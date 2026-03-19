# Lab 04 — Detect Certificate Misconfigurations

## Goal

This lab builds operational understanding of **common certificate misconfigurations that cause TLS failures.**

You will analyze certificate scenarios and determine why each certificate would fail validation. These types of misconfigurations are responsible for many real-world outages.

You will practice identifying issues involving:
- Missing **Subject Alternative Name (SAN)**
- Incorrect **Extended Key Usage (EKU)**
- **Expired certificates**
- **Missing intermediate certificates**

---

## Prerequisites

- Completion of **Lab 01, Lab 02, and Lab 03**
- Understanding of certificate fields and extensions
- Access to your portfolio repository

> This lab is **analysis-based** — no OpenSSL commands are required. You will read certificate scenarios and document your reasoning.

---

## Scenarios

For each scenario below, analyze the certificate configuration and explain why it would fail or cause problems in a production system. Record your answers in your submission file (see Submission section).

---

### Scenario 1 — Missing Subject Alternative Name

A certificate contains the following:

```
Subject: CN=example.com
```

However, the certificate **does not contain a Subject Alternative Name (SAN)** extension.

**Question:** Would modern browsers trust this certificate for `example.com`?

**In your analysis explain:**
- Why modern browsers require the SAN extension
- Why the Common Name (CN) alone is no longer sufficient
- What error a user would likely see

---

### Scenario 2 — Incorrect Extended Key Usage

A certificate contains the following extension:

```
Extended Key Usage: Client Authentication
```

The certificate is being used on a **web server for HTTPS.**

**Question:** Would a browser accept this certificate for a web server?

**In your analysis explain:**
- What Extended Key Usage defines
- Why `TLS Web Server Authentication` is required for HTTPS
- What error a user would likely see

---

### Scenario 3 — Expired Certificate

The certificate validity period shows:

```
Not Before: May 1 2022
Not After:  May 1 2023
```

The current date is **after May 1, 2023.**

**Question:** What happens if this certificate is used today?

**In your analysis explain:**
- Why expiration causes TLS validation to fail
- Why certificate lifecycle management is critical
- What a user would see in their browser

---

### Scenario 4 — Missing Intermediate Certificate

A server presents a certificate during a TLS handshake, but the **intermediate certificate is not included in the chain.**

**Question:** What error would a browser likely display?

**In your analysis explain:**
- How certificate chains establish trust
- Why servers must include the intermediate certificate
- Why some browsers handle this differently than others

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `lab-04-certificate-misconfigurations.md` | Your analysis of all 4 scenarios |

---

### Step 1 — Confirm your submissions folder exists

```
labs/week-03/submissions/
```

If it does not exist:

```bash
mkdir -p labs/week-03/submissions
```

---

### Step 2 — Complete your analysis file

Create `lab-04-certificate-misconfigurations.md` in `labs/week-03/submissions/` and fill in the template:

```markdown
# Lab 04 — Detect Certificate Misconfigurations

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**
[Your answer]

**Analysis:**
[Explain why SAN is required, why CN is not sufficient, and what error users would see]

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**
[Your answer]

**Analysis:**
[Explain what EKU defines, what value is required for HTTPS, and what error users would see]

---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**
[Your answer]

**Analysis:**
[Explain why expiration fails validation, why lifecycle management matters, and what users would see]

---

## Scenario 4 — Missing Intermediate Certificate

**What error would a browser likely display?**
[Your answer]

**Analysis:**
[Explain how chains establish trust, why servers must include intermediates, and why browser behavior varies]

---

## Key Takeaway
In 2-3 sentences, explain why certificate misconfiguration is one of the most common causes of PKI outages.
```

---

### Step 3 — Confirm your folder structure

```
labs/
  week-03/
    submissions/
      lab-04-certificate-misconfigurations.md
```

---

### Step 4 — Commit and push to GitHub

Choose **one** of the following methods:

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-03/submissions/
git commit -m "Week 3 Lab 04 — Certificate misconfiguration analysis"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-03/submissions/`
3. Click **Add file → Upload files**
4. Drag and drop `lab-04-certificate-misconfigurations.md` into the upload area
5. Scroll down to **Commit changes**, enter a message like `Week 3 Lab 04 — Certificate misconfiguration analysis`, and click **Commit changes**

> **Don't see the submissions folder yet?** Click **Add file → Create new file**, then type the full path in the filename field: `labs/week-03/submissions/lab-04-certificate-misconfigurations.md`. This creates the folder and file at the same time.

---

### Step 5 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 3 → Lab 04: Certificate Misconfigurations**
3. Click **Copy** next to the file path — confirm your file is at that exact location
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker.

---

## Stretch Goal (Optional)

Research a **real-world certificate outage.** Examples:
- Let's Encrypt DST Root CA expiration (2021)
- Microsoft Teams certificate outage
- Slack TLS certificate expiration

Add a short summary to your file explaining:
- What caused the outage
- Which certificate misconfiguration was responsible
- How it was resolved

---

## Why This Matters

Most PKI outages are **not caused by broken cryptography.** They are caused by certificate configuration and management mistakes:
- Expired certificates
- Incorrect extensions
- Missing intermediate certificates
- Invalid hostname configuration

Being able to quickly identify these problems is a core skill for PKI engineers and security professionals.

---

*CVI PKI Career Pathway — Foundations Phase*
