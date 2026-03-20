# Lab 01 — Convert Certificate Formats

## Goal

This lab builds operational understanding of PEM, DER, and PFX certificate formats and how to convert between them using OpenSSL.

You will:

- Retrieve a live certificate in PEM format
- Inspect the raw encoding of a PEM file
- Convert the certificate to DER format
- Convert the certificate back to PEM to verify the round-trip
- Bundle a certificate and key into PFX format
- Verify each conversion produces a valid certificate

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Your Week 4 folder already exists in your portfolio repo at `labs/week-04/`

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Retrieve a Certificate in PEM Format

### Step 1 — Connect to a website and retrieve its leaf certificate

```bash
openssl s_client -connect google.com:443 -showcerts
```

Locate the **first certificate block** — this is the leaf certificate issued directly to the website:

```
-----BEGIN CERTIFICATE-----
MIIF...
-----END CERTIFICATE-----
```

Copy the entire block including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.

---

### Step 2 — Save the certificate

Create your artifact directory and save the certificate:

```bash
mkdir -p labs/week-04/submissions/convert-formats
```

Save the copied block as:

```
labs/week-04/submissions/convert-formats/leaf_cert.pem
```

---

### Step 3 — Inspect the PEM file

Open the PEM file in a text editor and observe the contents. Then confirm it parses correctly:

```bash
openssl x509 -in labs/week-04/submissions/convert-formats/leaf_cert.pem -text -noout
```

Record: What do the `BEGIN` and `END` lines tell you about the encoding?

---

## Part 2 — Convert Certificate Formats

### Step 4 — Convert PEM to DER

Convert the PEM certificate to binary DER format:

```bash
openssl x509 \
  -in labs/week-04/submissions/convert-formats/leaf_cert.pem \
  -outform DER \
  -out labs/week-04/submissions/convert-formats/leaf_cert.der
```

Try opening the `.der` file in a text editor. Document what you observe.

Then verify the DER file is valid:

```bash
openssl x509 \
  -in labs/week-04/submissions/convert-formats/leaf_cert.der \
  -inform DER \
  -text -noout
```

---

### Step 5 — Convert DER back to PEM

Confirm you can reverse the conversion:

```bash
openssl x509 \
  -in labs/week-04/submissions/convert-formats/leaf_cert.der \
  -inform DER \
  -outform PEM \
  -out labs/week-04/submissions/convert-formats/leaf_cert_restored.pem
```

Compare the original and restored PEM files:

```bash
diff labs/week-04/submissions/convert-formats/leaf_cert.pem \
     labs/week-04/submissions/convert-formats/leaf_cert_restored.pem
```

Record what the diff output tells you.

---

### Step 6 — Create a PFX bundle

To create a PFX you need a private key paired with a certificate. Generate a test key pair and self-sign a certificate:

```bash
openssl genrsa \
  -out labs/week-04/submissions/convert-formats/test_key.pem 2048

openssl req -new -x509 \
  -key labs/week-04/submissions/convert-formats/test_key.pem \
  -out labs/week-04/submissions/convert-formats/test_cert.pem \
  -days 365 \
  -subj "/CN=Week4-Lab-Test/O=CVI/C=US"
```

Bundle the certificate and key into a PFX file:

```bash
openssl pkcs12 -export \
  -out labs/week-04/submissions/convert-formats/test_bundle.pfx \
  -inkey labs/week-04/submissions/convert-formats/test_key.pem \
  -in labs/week-04/submissions/convert-formats/test_cert.pem
```

You will be prompted to set a password. Use a simple test password and record that you set one.

> **Important:** Do not commit `test_key.pem` to GitHub — this file contains a private key.
> Add it to your `.gitignore` or delete it after completing the lab.

---

### Step 7 — Verify the PFX

Confirm the PFX bundle is valid:

```bash
openssl pkcs12 -info \
  -in labs/week-04/submissions/convert-formats/test_bundle.pfx \
  -noout
```

Enter the password when prompted. Record what information is displayed.

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `leaf_cert.pem` | Raw certificate retrieved from Google |
| `leaf_cert.der` | DER-format conversion |
| `leaf_cert_restored.pem` | PEM restored from DER |
| `test_cert.pem` | Self-signed test certificate |
| `test_bundle.pfx` | PFX bundle |
| `lab-01-convert-certificate-formats.md` | Your completed observations |

> Do NOT commit `test_key.pem` — add `*.key` to your `.gitignore`.

---

### Step 1 — Confirm your artifact directory exists

```bash
ls labs/week-04/submissions/convert-formats/
```

Your folder should contain the files listed above.

---

### Step 2 — Complete your observations file

Open `labs/week-04/submissions/convert-formats/lab-01-convert-certificate-formats.md` and fill in the template below:

```markdown
# Lab 01 — Convert Certificate Formats

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
5.

---

## Results
Include the important outputs or findings from the lab.

- What did the PEM file look like compared to the DER file?
- What happened when you opened the .der file in a text editor?
- What did the diff output show after converting PEM → DER → PEM?
- What information was displayed when you verified the PFX?

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

- Why does a PFX require a password?
- In what real-world scenario would you choose PEM vs DER vs PFX?
- Why is it important never to commit private key files to GitHub?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

---

## Artifacts
List the files generated during this lab.

- leaf_cert.pem
- leaf_cert.der
- leaf_cert_restored.pem
- test_cert.pem
- test_bundle.pfx
```

---

### Step 3 — Commit and push to GitHub

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-04/submissions/convert-formats/
git commit -m "Week 4 Lab 01 — Convert Certificate Formats"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-04/submissions/convert-formats/`
3. Click **Add file → Upload files**
4. Drag and drop your files into the upload area
5. Scroll down to **Commit changes**, enter `Week 4 Lab 01 — Convert Certificate Formats`, and click **Commit changes**

> **Don't see the folder yet?** Click **Add file → Create new file** and type the full path in the filename field: `labs/week-04/submissions/convert-formats/lab-01-convert-certificate-formats.md`. This creates the folder and file at the same time.

---

### Step 4 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 4 → Lab 01: Convert Certificate Formats**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

## Stretch Goal (Optional)

Extract only the certificate from your PFX bundle (without the private key) and verify it:

```bash
openssl pkcs12 \
  -in labs/week-04/submissions/convert-formats/test_bundle.pfx \
  -nokeys \
  -out labs/week-04/submissions/convert-formats/extracted_cert.pem

openssl x509 \
  -in labs/week-04/submissions/convert-formats/extracted_cert.pem \
  -text -noout
```

Questions to consider:

- Does the extracted certificate match the original `test_cert.pem`?
- Why would you extract a certificate from a PFX without the private key?
- In what enterprise scenario would you receive a PFX and need to extract just the certificate?

---

*CVI PKI Career Pathway — Foundations Phase*
