# Lab 01 — Inspect X.509 Certificate Fields

## Goal

This lab builds operational understanding of how to read and interpret the core fields of an X.509 certificate.

You will:

- Retrieve a certificate from a live website
- Parse the certificate using OpenSSL
- Identify the core certificate fields
- Understand how certificates represent digital identity in PKI systems

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Your Week 3 folder already exists in your portfolio repo at `labs/week-03/`

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Retrieve a Website Certificate

---

### Step 1 — Connect to a website and pull its certificate

Run the following command in your terminal:

```bash
openssl s_client -connect google.com:443 -showcerts
```

You will see several certificate blocks in the output.

Locate the **first certificate block** — this is the **leaf certificate** (the one issued directly to the website).

It will look like this:

```
-----BEGIN CERTIFICATE-----
MIIF...
-----END CERTIFICATE-----
```

Copy the entire block including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.

---

### Step 2 — Save the certificate

Save the copied block as a file named `leaf_cert.pem` on your Desktop or anywhere easy to find locally. You will use it in the next step.

---

## Part 2 — Parse the Certificate

Run this command to convert the certificate into a human-readable format:

```bash
openssl x509 -in leaf_cert.pem -text -noout
```
---

### Step 3 — Locate and record these fields in the output

| Field | What to look for |
|---|---|
| Version | `Version: 3` |
| Serial Number | Long hex number |
| Signature Algorithm | e.g., `sha256WithRSAEncryption` |
| Issuer | The CA that signed it |
| Subject | Who the cert is issued to |
| Not Before | Start of validity window |
| Not After | Expiration date |
| Public Key Algorithm | e.g., `rsaEncryption` |

---

## Part 3 — Record Your Observations

Answer the following in your submission file (see Submission section below):

- Who issued this certificate?
- What domain or organization does it represent?
- What public key algorithm is used?
- When does the certificate expire?
- Why does the Issuer field matter in a PKI system?

---

## Submission

### What you need to submit

You will submit **two files** to your GitHub portfolio repo:

| File | Description |
|---|---|
| `leaf_cert.pem` | The raw certificate you retrieved from Google |
| `lab-01-certificate-fields.md` | Your completed observations (template below) |

---

### Step 1 — Create the submissions folder

In your terminal, navigate to your local repo and run:

```bash
mkdir -p labs/week-03/submissions
```

> If the folder already exists, this command is safe to run — it will not overwrite anything.

---

### Step 2 — Copy your files into the folder

Move or copy both files into:

```
labs/week-03/submissions/
```

Your folder should now look like this:

```
labs/
  week-03/
    submissions/
      leaf_cert.pem
      lab-01-certificate-fields.md
```

---

### Step 3 — Complete your observations file

Open `lab-01-certificate-fields.md` and fill in the template below:

```markdown
# Lab 01 — Inspect X.509 Certificate Fields

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS:
- Terminal used (Mac Terminal / Git Bash / WSL):
- OpenSSL version (`openssl version`):

---

## Certificate Fields

| Field                | Value from your output |
|----------------------|------------------------|
| Version              |                        |
| Serial Number        |                        |
| Signature Algorithm  |                        |
| Issuer               |                        |
| Subject              |                        |
| Not Before           |                        |
| Not After            |                        |
| Public Key Algorithm |                        |

---

## Observations

1. Who issued the certificate?
2. What domain or organization does it represent?
3. When does it expire?
4. What public key algorithm is used?
5. Why does the Issuer field matter in a PKI system?
```

---

### Step 4 — Commit and push to GitHub

Choose **one** of the following methods:

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-03/submissions/
git commit -m "Week 3 Lab 01 — Inspect Certificate Fields"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-03/submissions/` — if the folder doesn't exist yet, see the note below
3. Click **Add file → Upload files**
4. Drag and drop `leaf_cert.pem` and `lab-01-certificate-fields.md` into the upload area
5. Scroll down to **Commit changes**, enter a message like `Week 3 Lab 01 — Inspect Certificate Fields`, and click **Commit changes**

> **Don't see the submissions folder yet?** GitHub won't let you create empty folders through the web UI. Instead, click **Add file → Create new file**, then type the full path in the filename field: `labs/week-03/submissions/lab-01-certificate-fields.md`. This creates the folder and file at the same time. Upload your `.pem` file separately using **Add file → Upload files** once the folder exists.


---

### Step 5 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 3 → Lab 01: Certificate Fields**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

## Stretch Goal (Optional)

Try retrieving a certificate from a different website:

```bash
openssl s_client -connect github.com:443 -showcerts
```

Compare it to the Google certificate and consider:

- Do the Issuer fields match?
- Do both use the same public key algorithm?
- Do the validity periods differ?
- Are the Subject fields structured the same way?

---

*CVI PKI Career Pathway — Foundations Phase*


