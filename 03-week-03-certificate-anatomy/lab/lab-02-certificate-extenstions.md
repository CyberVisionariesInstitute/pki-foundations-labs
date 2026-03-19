# Lab 02 — Investigate Certificate Extensions

## Goal

This lab builds operational understanding of X.509 certificate extensions, which define how a certificate can be used and validated.

You will:
- Inspect certificate extensions using OpenSSL
- Identify common certificate extensions
- Understand how extensions control certificate behavior
- Analyze how extensions impact TLS validation

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Completion of **Lab 01 — Inspect Certificate Fields**
- Your `leaf_cert.pem` file saved from Lab 01

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Inspect Certificate Extensions

### Step 1 — Use your certificate from Lab 01

You will reuse the `leaf_cert.pem` file saved in Lab 01. Confirm it exists on your machine before continuing.

### Step 2 — Run the inspection command

```bash
openssl x509 -in leaf_cert.pem -text -noout
```

Scroll through the output until you find the **X509v3 extensions** section.

---

## Part 2 — Identify Key Extensions

Locate and record the following four extensions:

### Subject Alternative Name (SAN)
Defines which domains the certificate is valid for.

Example output:
```
X509v3 Subject Alternative Name:
    DNS:google.com, DNS:www.google.com
```

---

### Key Usage
Defines which cryptographic operations the certificate can perform.

Example output:
```
X509v3 Key Usage:
    Digital Signature, Key Encipherment
```

---

### Extended Key Usage (EKU)
Defines what the certificate is allowed to authenticate.

Example output:
```
X509v3 Extended Key Usage:
    TLS Web Server Authentication
```

---

### Basic Constraints
Determines whether the certificate can act as a Certificate Authority.

Example output:
```
X509v3 Basic Constraints:
    CA:FALSE
```

---

## Part 3 — Record Your Observations

Answer the following in your submission file (see Submission section below):

- What domains appear in the SAN field?
- What operations are listed in Key Usage?
- What applications are listed in Extended Key Usage?
- Can this certificate issue other certificates? How do you know?
- Why are these extensions important for TLS validation?

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `lab-02-certificate-extensions.md` | Your completed observations |

> You do not need to submit `leaf_cert.pem` again — it was already submitted in Lab 01.

---

### Step 1 — Confirm your submissions folder exists

Your folder should already exist from Lab 01 at:

```
labs/week-03/submissions/
```

If it does not exist, create it:

```bash
mkdir -p labs/week-03/submissions
```

---

### Step 2 — Complete your observations file

Create `lab-02-certificate-extensions.md` in `labs/week-03/submissions/` and fill in the template below:

```markdown
# Lab 02 — Investigate Certificate Extensions

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS:
- Terminal used (Mac Terminal / Git Bash / WSL):
- OpenSSL version (`openssl version`):

---

## Extensions Found

### Subject Alternative Name (SAN)
Paste the value from your output:

### Key Usage
Paste the value from your output:

### Extended Key Usage (EKU)
Paste the value from your output:

### Basic Constraints
Paste the value from your output:

---

## Observations

1. What domains appear in the SAN field?
2. What operations are permitted by Key Usage?
3. What applications are authorized by EKU?
4. Can this certificate issue other certificates? How do you know?
5. Why are these extensions important for TLS validation?
```

---

### Step 3 — Confirm your folder structure

```
labs/
  week-03/
    submissions/
      leaf_cert.pem          ← from Lab 01
      lab-01-certificate-fields.md    ← from Lab 01
      lab-02-certificate-extensions.md  ← this lab
```

---

### Step 4 — Commit and push to GitHub

Choose **one** of the following methods:

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-03/submissions/
git commit -m "Week 3 Lab 02 — Investigate Certificate Extensions"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-03/submissions/`
3. Click **Add file → Upload files**
4. Drag and drop `lab-02-certificate-extensions.md` into the upload area
5. Scroll down to **Commit changes**, enter a message like `Week 3 Lab 02 — Investigate Certificate Extensions`, and click **Commit changes**

> **Don't see the submissions folder yet?** Click **Add file → Create new file**, then type the full path in the filename field: `labs/week-03/submissions/lab-02-certificate-extensions.md`. This creates the folder and file at the same time.

---

### Step 5 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 3 → Lab 02: Certificate Extensions**
3. Click **Copy** next to the file path — confirm your file is at that exact location
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker.

---

## Stretch Goal (Optional)

Retrieve a certificate from a different website:

```bash
openssl s_client -connect github.com:443 -showcerts
```

Compare its extensions to the Google certificate and consider:
- Do both certificates contain similar SAN entries?
- Do both allow the same Extended Key Usage?
- Are there any additional extensions present?

---

*CVI PKI Career Pathway — Foundations Phase*

