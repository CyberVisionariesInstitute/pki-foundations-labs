# Lab 05 — Extract a Certificate from a Live Website (Optional)

## Goal

This optional lab demonstrates how to retrieve and inspect a live TLS certificate from a website — mirroring how security engineers analyze certificates during TLS troubleshooting and incident response.

You will:
- Connect to a website using OpenSSL
- Retrieve the certificate presented during the TLS handshake
- Save the certificate locally
- Inspect its fields and extensions

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, PowerShell, Git Bash, or WSL)
- Completion of **Lab 01 and Lab 02 recommended**

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Retrieve a Live Certificate

### Step 1 — Connect to a website

```bash
openssl s_client -connect github.com:443 -showcerts
```

You will see several certificate blocks in the terminal output.

The **first certificate block** is the **leaf certificate** — the one issued directly to the website.

It looks like:

```
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

### Step 2 — Save the certificate

Copy the first certificate block and save it as:

```
github_cert.pem
```

Place the file anywhere accessible on your local machine — you will move it to your repo in the Submission section.

---

## Part 2 — Inspect the Certificate

### Step 3 — Parse the certificate

```bash
openssl x509 -in github_cert.pem -text -noout
```

### Step 4 — Locate and record these fields

| Field | What to look for |
|---|---|
| Subject | Who the cert is issued to |
| Issuer | The CA that signed it |
| Not Before / Not After | Validity window |
| Public Key Algorithm | e.g., `rsaEncryption` |
| Subject Alternative Name | Domains covered |
| Key Usage | Permitted operations |
| Extended Key Usage | Authorized applications |

---

## Part 3 — Record Your Observations

Answer the following in your submission file (see Submission section below):

- What organization does this certificate belong to?
- Which Certificate Authority issued it?
- When does it expire?
- What domains are listed in the SAN field?
- What is the certificate authorized to be used for?

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `github_cert.pem` | The raw certificate retrieved from GitHub |
| `lab-05-extract-live-certificate.md` | Your completed observations |

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

### Step 2 — Complete your observations file

Create `lab-05-extract-live-certificate.md` in `labs/week-03/submissions/` and fill in the template:

```markdown
# Lab 05 — Extract a Certificate from a Live Website

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS:
- Terminal used (Mac Terminal / PowerShell / Git Bash / WSL):
- OpenSSL version (`openssl version`):
- Website used: github.com

---

## Certificate Fields Found

| Field                    | Value from your output |
|--------------------------|------------------------|
| Subject                  |                        |
| Issuer                   |                        |
| Not Before               |                        |
| Not After                |                        |
| Public Key Algorithm     |                        |
| Subject Alternative Name |                        |
| Key Usage                |                        |
| Extended Key Usage       |                        |

---

## Observations

1. What organization does this certificate belong to?
2. Which Certificate Authority issued it?
3. When does it expire?
4. What domains are listed in the SAN field?
5. What is this certificate authorized to be used for?
```

---

### Step 3 — Confirm your folder structure

```
labs/
  week-03/
    submissions/
      github_cert.pem
      lab-05-extract-live-certificate.md
```

---

### Step 4 — Commit and push to GitHub

Choose **one** of the following methods:

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-03/submissions/
git commit -m "Week 3 Lab 05 — Extract and analyze live TLS certificate"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-03/submissions/`
3. Click **Add file → Upload files**
4. Drag and drop `github_cert.pem` and `lab-05-extract-live-certificate.md` into the upload area
5. Scroll down to **Commit changes**, enter a message like `Week 3 Lab 05 — Extract and analyze live TLS certificate`, and click **Commit changes**

> **Don't see the submissions folder yet?** Click **Add file → Create new file**, then type the full path in the filename field: `labs/week-03/submissions/lab-05-extract-live-certificate.md`. This creates the folder and file at the same time. Then upload the `.pem` file separately using **Add file → Upload files**.

---

### Step 5 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 3 → Lab 05: Extract Live Certificate (Optional)**
3. Click **Copy** next to the file path — confirm your file is at that exact location
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker.

---

## Stretch Goal (Optional)

Retrieve certificates from additional websites and compare them:

```bash
openssl s_client -connect google.com:443 -showcerts
openssl s_client -connect cloudflare.com:443 -showcerts
```

Consider:
- Do they use the same Certificate Authority?
- Do they use the same public key algorithm?
- Do they contain similar SAN entries?
- Do their expiration dates differ significantly?

---

## Why This Matters

Security engineers frequently inspect live certificates when troubleshooting:
- TLS connection failures
- Certificate expiration issues
- Misconfigured certificate chains
- Hostname validation errors

Being able to retrieve and analyze certificates directly from a live TLS connection is an essential skill in PKI operations and security engineering.

---

*CVI PKI Career Pathway — Foundations Phase*

