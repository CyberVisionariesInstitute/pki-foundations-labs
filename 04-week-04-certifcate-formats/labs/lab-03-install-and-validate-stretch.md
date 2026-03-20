# Lab 03 — Install a Certificate and Validate Trust (Stretch)

## Goal

This stretch lab builds hands-on experience with the full certificate installation and trust validation workflow.

You will:

- Generate a self-signed root CA certificate
- Install it into your operating system's trust store
- Create a certificate signed by that CA
- Validate that your system now trusts the signed certificate
- Remove the certificate cleanly after the lab

> **This lab requires elevated permissions** — `sudo` on Linux/macOS, or Administrator on Windows.

---

## Prerequisites

- OpenSSL installed
- Terminal with elevated permissions available
- Completed Lab 01 (Convert Certificate Formats)
- Your Week 4 folder already exists in your portfolio repo at `labs/week-04/`

> **Security Note:** All certificates created in this lab are for local testing only.
> You will remove them from your trust store at the end.
> Do not use these certificates for anything outside this lab.

---

## Part 1 — Setup

Create your artifact directory:

```bash
mkdir -p labs/week-04/submissions/install-validate
```

---

## Part 2 — Execution Steps

### Step 1 — Generate a Test Root CA

Create a private key and self-signed root CA certificate:

```bash
openssl genrsa \
  -out labs/week-04/submissions/install-validate/test-root-ca.key \
  2048

openssl req -new -x509 \
  -key labs/week-04/submissions/install-validate/test-root-ca.key \
  -out labs/week-04/submissions/install-validate/test-root-ca.crt \
  -days 30 \
  -subj "/CN=CVI-Lab-Root-CA/O=CyberVisionaries Institute/C=US"
```

Verify the certificate was created correctly:

```bash
openssl x509 \
  -in labs/week-04/submissions/install-validate/test-root-ca.crt \
  -text -noout
```

Record: Subject, Issuer, and expiration date. Note that Subject and Issuer are identical — this is how you identify a root CA (self-signed).

---

### Step 2 — Install the Root CA into Your Trust Store

Follow the steps for your operating system.

**macOS:**
```bash
sudo security add-trusted-cert \
  -d -r trustRoot \
  -k /Library/Keychains/System.keychain \
  labs/week-04/submissions/install-validate/test-root-ca.crt
```

**Linux (Debian/Ubuntu):**
```bash
sudo cp labs/week-04/submissions/install-validate/test-root-ca.crt \
  /usr/local/share/ca-certificates/cvi-lab-root-ca.crt

sudo update-ca-certificates
```

**Linux (RHEL/CentOS/Fedora):**
```bash
sudo cp labs/week-04/submissions/install-validate/test-root-ca.crt \
  /etc/pki/ca-trust/source/anchors/cvi-lab-root-ca.crt

sudo update-ca-trust
```

**Windows (PowerShell — run as Administrator):**
```bash
Import-Certificate \
  -FilePath "labs\week-04\submissions\install-validate\test-root-ca.crt" \
  -CertStoreLocation Cert:\LocalMachine\Root
```

---

### Step 3 — Create a Certificate Signed by Your Test Root CA

Generate a key and CSR for a test certificate:

```bash
openssl genrsa \
  -out labs/week-04/submissions/install-validate/test-signed.key \
  2048

openssl req -new \
  -key labs/week-04/submissions/install-validate/test-signed.key \
  -out labs/week-04/submissions/install-validate/test-signed.csr \
  -subj "/CN=test.cvi-lab.local/O=CyberVisionaries Institute/C=US"
```

Sign the CSR with your test root CA:

```bash
openssl x509 -req \
  -in labs/week-04/submissions/install-validate/test-signed.csr \
  -CA labs/week-04/submissions/install-validate/test-root-ca.crt \
  -CAkey labs/week-04/submissions/install-validate/test-root-ca.key \
  -CAcreateserial \
  -out labs/week-04/submissions/install-validate/test-signed.crt \
  -days 30
```

---

### Step 4 — Validate the Trust Chain

Use OpenSSL to verify the signed certificate chains up to your test root CA:

```bash
openssl verify \
  -CAfile labs/week-04/submissions/install-validate/test-root-ca.crt \
  labs/week-04/submissions/install-validate/test-signed.crt
```

Expected output:

```
test-signed.crt: OK
```

Record the output. If you see an error, document it and investigate before proceeding.

---

### Step 5 — Remove the Test Root CA (Required)

After completing the lab, remove the test root CA from your trust store.

**macOS:**
```bash
sudo security delete-certificate \
  -c "CVI-Lab-Root-CA" \
  /Library/Keychains/System.keychain
```

**Linux (Debian/Ubuntu):**
```bash
sudo rm /usr/local/share/ca-certificates/cvi-lab-root-ca.crt
sudo update-ca-certificates --fresh
```

**Linux (RHEL/CentOS/Fedora):**
```bash
sudo rm /etc/pki/ca-trust/source/anchors/cvi-lab-root-ca.crt
sudo update-ca-trust
```

**Windows (PowerShell):**
```bash
Get-ChildItem Cert:\LocalMachine\Root | Where-Object { $_.Subject -like "*CVI-Lab*" } | Remove-Item
```

Confirm the certificate has been removed by repeating the verify step from Step 4. It should now fail — that is the expected and correct outcome.

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `test-root-ca.crt` | Your self-signed root CA certificate |
| `test-signed.crt` | Certificate signed by your test root CA |
| `lab-03-install-and-validate.md` | Your completed observations |

> Do NOT commit any `.key` files — add `*.key` to your `.gitignore`.

---

### Step 1 — Confirm your artifact directory exists

```bash
ls labs/week-04/submissions/install-validate/
```

---

### Step 2 — Complete your observations file

Open `labs/week-04/submissions/install-validate/lab-03-install-and-validate.md` and fill in the template below:

```markdown
# Lab 03 — Install a Certificate and Validate Trust (Stretch)

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

- What did the certificate output show when you verified test-root-ca.crt?
- What did the verify output return after signing — before and after cleanup?
- What confirmed the trust chain was established?

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

- What made the test root CA self-signed? How did you identify that in the output?
- What changed on your system after you installed the root CA?
- In an enterprise environment, who controls what root CAs are installed on employee machines?
- Why is it a security concern if an attacker can install a root CA on a device?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

---

## Artifacts
List the files generated during this lab.

- test-root-ca.crt
- test-signed.crt
- test-signed.csr
```

---

### Step 3 — Commit and push to GitHub

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-04/submissions/install-validate/
git commit -m "Week 4 Lab 03 — Install and Validate Certificate Trust"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-04/submissions/install-validate/`
3. Click **Add file → Upload files**
4. Drag and drop your files into the upload area
5. Scroll down to **Commit changes**, enter `Week 4 Lab 03 — Install and Validate Certificate Trust`, and click **Commit changes**

> **Don't see the folder yet?** Click **Add file → Create new file** and type the full path in the filename field: `labs/week-04/submissions/install-validate/lab-03-install-and-validate.md`. This creates the folder and file at the same time.

---

### Step 4 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 4 → Lab 03: Install and Validate Certificate Trust**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

*CVI PKI Career Pathway — Foundations Phase*
