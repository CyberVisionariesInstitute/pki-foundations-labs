# Lab 02 — Inspect Your Trust Store

## Goal

This lab builds operational understanding of where trusted root certificates live on an operating system and how the trust store works in practice.

You will:

- Locate the trust store on your operating system
- List trusted root Certificate Authorities
- Inspect a specific root CA certificate
- Connect trust store behavior to real-world certificate validation

---

## Prerequisites

- Access to a local terminal (Mac Terminal, Git Bash, WSL, or PowerShell)
- OpenSSL installed
- Your Week 4 folder already exists in your portfolio repo at `labs/week-04/`

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Setup

Create your artifact directory:

```bash
mkdir -p labs/week-04/submissions/trust-store-inspection
```

---

## Part 2 — Execution Steps

Follow the steps for **your operating system.**

---

### macOS

#### Step 1 — List Root CAs in the System Keychain

```bash
security find-certificate -a -p \
  /System/Library/Keychains/SystemRootCertificates.keychain \
  > labs/week-04/submissions/trust-store-inspection/root_cas.pem
```

Count how many root certificates are in the file:

```bash
grep -c "BEGIN CERTIFICATE" labs/week-04/submissions/trust-store-inspection/root_cas.pem
```

Record the number.

#### Step 2 — Extract and Inspect One Root CA

Use OpenSSL to inspect the first certificate in the file:

```bash
openssl x509 \
  -in labs/week-04/submissions/trust-store-inspection/root_cas.pem \
  -text -noout 2>/dev/null | head -30
```

Record: Subject, Issuer, and expiration date.

#### Step 3 — Open Keychain Access (Visual Inspection)

Open **Keychain Access** from Applications > Utilities.

Navigate to: **System Roots** keychain > **Certificates** category.

Identify three root CAs you recognize and note their names and issuers.

---

### Linux (Debian / Ubuntu)

#### Step 1 — List Trusted Root Certificates

```bash
ls /etc/ssl/certs/ | head -20
```

Count the total number of trusted certificates:

```bash
ls /etc/ssl/certs/*.pem 2>/dev/null | wc -l
```

Record the number.

#### Step 2 — Inspect a Root CA Certificate

Choose any `.pem` file from `/etc/ssl/certs/` and inspect it:

```bash
openssl x509 -in /etc/ssl/certs/DigiCert_Global_Root_CA.pem -text -noout
```

> Replace the filename with one that exists on your system if needed.

Record: Subject, Issuer, and expiration date.

#### Step 3 — Identify the CA Bundle Location

```bash
openssl version -d
```

This shows the default directory OpenSSL uses for CA certificates. Record the path and note what files are present there.

---

### Windows

#### Step 1 — Open Certificate Manager

Press **Win + R**, type `certmgr.msc`, and press Enter.

#### Step 2 — Navigate to Trusted Root CAs

In the left panel, expand:

**Trusted Root Certification Authorities > Certificates**

Count the number of certificates listed and record it.

#### Step 3 — Inspect a Root CA

Double-click any root CA certificate. Navigate to the **Details** tab and record:

- Subject
- Issuer
- Valid From / Valid To
- Public key algorithm

#### Step 4 — Run certutil (Command Line)

Open PowerShell and run:

```bash
certutil -store Root | Select-String "CN="
```

This lists the Common Names of all trusted root CAs in the machine store.

---

### All Platforms — Validate a Certificate Against the Trust Store

Use OpenSSL to validate a live certificate against your system's trusted roots:

```bash
openssl s_client -connect google.com:443 -verify_return_error
```

Look for:

```
Verify return code: 0 (ok)
```

Record what this output tells you about the trust relationship between the certificate and your trust store.

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `root_cas.pem` | Root CA list exported from your system (macOS only) |
| `lab-02-inspect-trust-stores.md` | Your completed observations |

---

### Step 1 — Confirm your artifact directory exists

```bash
ls labs/week-04/submissions/trust-store-inspection/
```

---

### Step 2 — Complete your observations file

Open `labs/week-04/submissions/trust-store-inspection/lab-02-inspect-trust-stores.md` and fill in the template below:

```markdown
# Lab 02 — Inspect Your Trust Store

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept or system behavior were you investigating?

---

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (`openssl version`):

---

## Steps Performed
Summarize the key steps you performed to complete the lab.
Do not copy the lab instructions — describe what you actually did.

1.
2.
3.
4.

---

## Results
Include the important outputs or findings from the lab.

- How many trusted root CAs did you find on your system?
- Name at least one specific root CA you inspected. Include its Subject and expiration date.
- What did the verify return code output tell you?

If you include screenshots, store them in the assets folder and reference them here:
![Description](../../assets/screenshots/week-04/your-screenshot.png)

---

## Key Findings
Document the most important observations from the lab.

•
•
•

---

## Explanation
Explain why the results matter.

- Why does your browser trust a certificate from a website you have never visited before?
- What would happen if an enterprise's internal root CA was NOT in the trust store?
- What surprised you about how many roots are pre-installed on your system?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

---

## Artifacts
List the files generated during this lab.

- root_cas.pem (macOS) or equivalent
- screenshots stored in assets/screenshots/week-04/
```

---

### Step 3 — Commit and push to GitHub

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-04/submissions/trust-store-inspection/
git commit -m "Week 4 Lab 02 — Inspect Trust Stores"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-04/submissions/trust-store-inspection/`
3. Click **Add file → Upload files**
4. Drag and drop your files into the upload area
5. Scroll down to **Commit changes**, enter `Week 4 Lab 02 — Inspect Trust Stores`, and click **Commit changes**

> **Don't see the folder yet?** Click **Add file → Create new file** and type the full path in the filename field: `labs/week-04/submissions/trust-store-inspection/lab-02-inspect-trust-stores.md`. This creates the folder and file at the same time.

---

### Step 4 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 4 → Lab 02: Inspect Trust Stores**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

## Stretch Goal (Optional)

Research what happens when an enterprise pushes an internal root CA to devices using Group Policy or an MDM.

Questions to consider:

- Why would an organization add their own root CA to employee devices?
- What security risk exists if an unauthorized root CA is added to a device?
- How would you detect if an unexpected root CA had been installed on a machine?

Document your findings in your `lab-02-inspect-trust-stores.md` under a `## Stretch` section.

---

*CVI PKI Career Pathway — Foundations Phase*
