# Lab 01 — Generate a CSR and Simulate the Issuance Workflow

## Goal

This lab builds operational understanding of how a certificate is born — from private key generation through CSR creation to a signed certificate — and connects that workflow to the X.509 structure you studied in Week 3.

You will:

- Generate a private key
- Create a Certificate Signing Request (CSR) and inspect its fields
- Self-sign a certificate to simulate what a CA does when it issues a certificate
- Inspect the signed certificate and map its fields back to the CSR
- Connect the CSR Subject fields to the X.509 certificate anatomy from Week 3

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Your Week 5 folder already exists in your portfolio repo at `labs/week-05/`

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Generate a Private Key

### Step 1 — Create your artifact directory

```bash
mkdir -p labs/week-05/submissions/generate-csr
```

### Step 2 — Generate an RSA private key

```bash
openssl genrsa \
  -out labs/week-05/submissions/generate-csr/test_key.pem 2048
```

Confirm the key was created:

```bash
openssl rsa \
  -in labs/week-05/submissions/generate-csr/test_key.pem \
  -text -noout | head -5
```

Record: What is the key type and size shown in the output?

> **Important:** Do not commit `test_key.pem` to GitHub — this file contains a private key.
> Add `*_key.pem` to your `.gitignore` before committing.

---

## Part 2 — Create a Certificate Signing Request (CSR)

### Step 3 — Generate a CSR

```bash
openssl req -new \
  -key labs/week-05/submissions/generate-csr/test_key.pem \
  -out labs/week-05/submissions/generate-csr/test_csr.pem \
  -subj "/CN=lab01.cvi.internal/O=CyberVisionaries Institute/OU=PKI Career Pathway/C=US/ST=California/L=San Francisco"
```

### Step 4 — Inspect the CSR

Read the CSR in human-readable form:

```bash
openssl req \
  -in labs/week-05/submissions/generate-csr/test_csr.pem \
  -text -noout
```

Locate and record each of the following fields in the Subject:

| Field | Abbreviation | Value in Your CSR |
|---|---|---|
| Common Name | CN | |
| Organization | O | |
| Organizational Unit | OU | |
| Country | C | |
| State | ST | |
| Locality | L | |

> **Connect to Week 3:** These Subject fields are the same fields you identified inside an X.509 certificate in Week 3. A CSR carries the Subject information the CA will embed into the issued certificate — the CA adds its own Issuer field and signs the result.

---

## Part 3 — Sign the Certificate (Simulating a CA)

In a real workflow, you would submit the CSR to a Certificate Authority. The CA verifies your identity, approves the request, and returns a signed certificate. In this lab, you will self-sign the certificate to simulate that step.

### Step 5 — Self-sign the certificate

```bash
openssl x509 -req \
  -in labs/week-05/submissions/generate-csr/test_csr.pem \
  -signkey labs/week-05/submissions/generate-csr/test_key.pem \
  -out labs/week-05/submissions/generate-csr/test_cert.pem \
  -days 365 \
  -set_serial 01
```

### Step 6 — Inspect the signed certificate

```bash
openssl x509 \
  -in labs/week-05/submissions/generate-csr/test_cert.pem \
  -text -noout
```

Locate and record the following in the output:

- **Subject:** — does it match the fields from your CSR?
- **Issuer:** — what is the Issuer for a self-signed certificate, and why?
- **Validity: Not Before** and **Not After** — how do these connect to the `-days 365` flag?
- **Public Key Algorithm** and **RSA Public-Key** — where did this public key come from?

---

## Part 4 — Compare CSR to Signed Certificate

### Step 7 — Confirm the public key matches

The public key embedded in the certificate must match the public key from your private key. Verify this:

```bash
# Extract the public key from the certificate
openssl x509 \
  -in labs/week-05/submissions/generate-csr/test_cert.pem \
  -pubkey -noout > /tmp/cert_pubkey.pem

# Extract the public key from the private key
openssl rsa \
  -in labs/week-05/submissions/generate-csr/test_key.pem \
  -pubout > /tmp/key_pubkey.pem

# Compare them
diff /tmp/cert_pubkey.pem /tmp/key_pubkey.pem
```

Record: What does an empty diff result tell you about the relationship between the private key and the certificate?

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `test_csr.pem` | Your generated Certificate Signing Request |
| `test_cert.pem` | Your self-signed certificate |
| `lab-01-generate-csr.md` | Your completed observations |

> Do NOT commit `test_key.pem` — add `*_key.pem` to your `.gitignore`.

---

### Step 1 — Confirm your artifact directory exists

```bash
ls labs/week-05/submissions/generate-csr/
```

Your folder should contain the files listed above (plus `test_key.pem` locally, which should not be committed).

---

### Step 2 — Complete your observations file

Open `labs/week-05/submissions/generate-csr/lab-01-generate-csr.md` and fill in the template below:

```markdown
# Lab 01 — Generate a CSR and Simulate the Issuance Workflow

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

- What Subject fields did you include in your CSR, and what does each one represent?
- What is the Issuer field in a self-signed certificate, and why does it match the Subject?
- What did the public key comparison (diff) show, and what does that mean?
- How do the fields in your signed certificate connect to what you learned about X.509 in Week 3?

If you include screenshots, store them in the assets folder and reference them here:
![Description](../../assets/screenshots/week-05/your-screenshot.png)

---

## Key Findings
Document the most important observations from the lab.

•
•
•

---

## Explanation
Explain why the results matter.

- What is the purpose of a CSR in the real certificate issuance workflow?
- Why does the CA receive a CSR rather than a private key?
- What would it mean if the public key in the certificate did NOT match the private key?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

---

## Artifacts
List the files generated during this lab.

- test_csr.pem
- test_cert.pem
```

---

### Step 3 — Commit and push to GitHub

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-05/submissions/generate-csr/
git commit -m "Week 5 Lab 01 — Generate a CSR and Simulate the Issuance Workflow"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-05/submissions/generate-csr/`
3. Click **Add file → Upload files**
4. Drag and drop your files into the upload area
5. Scroll down to **Commit changes**, enter `Week 5 Lab 01 — Generate a CSR and Simulate the Issuance Workflow`, and click **Commit changes**

> **Don't see the folder yet?** Click **Add file → Create new file** and type the full path in the filename field: `labs/week-05/submissions/generate-csr/lab-01-generate-csr.md`. This creates the folder and file at the same time.

---

### Step 4 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 5 → Lab 01: Generate a CSR**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

## Stretch Goal (Optional)

Add a Subject Alternative Name (SAN) extension to your CSR and re-sign the certificate:

```bash
# Create a config file for the SAN extension
cat > /tmp/san.cnf << EOF
[req]
distinguished_name = req_distinguished_name
req_extensions = v3_req
prompt = no

[req_distinguished_name]
CN = lab01-san.cvi.internal
O = CyberVisionaries Institute
C = US

[v3_req]
subjectAltName = DNS:lab01-san.cvi.internal, DNS:www.lab01-san.cvi.internal
EOF

# Generate a new CSR with SAN
openssl req -new \
  -key labs/week-05/submissions/generate-csr/test_key.pem \
  -out labs/week-05/submissions/generate-csr/test_csr_san.pem \
  -config /tmp/san.cnf

# Verify the SAN appears in the CSR
openssl req \
  -in labs/week-05/submissions/generate-csr/test_csr_san.pem \
  -text -noout | grep -A3 "Subject Alternative"
```

Questions to consider:

- Where did you see SANs in Week 3, and why are they required for modern TLS certificates?
- What is the difference between the CN and a SAN entry for the same domain name?
- Why would a certificate include multiple SAN entries?

---

*CVI PKI Career Pathway — Foundations Phase*
