# Lab 03 — Verify a Certificate Chain

## Goal

This lab builds operational understanding of how certificate chains establish trust in Public Key Infrastructure.

You will:

- Inspect multiple certificates in a trust hierarchy
- Identify the root, intermediate, and leaf certificates
- Verify a certificate chain using OpenSSL
- Understand how browsers validate certificate trust

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Completion of **Lab 01 and Lab 02**

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Retrieve a Certificate Chain

### Step 1 — Connect to a website and pull the full chain

```bash
openssl s_client -connect github.com:443 -showcerts
```

You will see **multiple certificate blocks** in the output. A typical chain contains:

1. **Leaf certificate** — issued to the website
2. **Intermediate certificate** — issued by the root CA
3. **Root certificate** — may be omitted from the output

Each certificate block looks like:

```
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

### Step 2 — Save each certificate separately

Copy each block and save them as individual files:

| File | Certificate |
|---|---|
| `server.pem` | First block (leaf/website certificate) |
| `intermediate.pem` | Second block (intermediate CA) |
| `root.pem` | Third block, or download from the CA's website if missing |

> If the root certificate is not in the output, look for the Issuer field in the intermediate cert — the root CA's website will have a download link.

> **Mac users — saving certificate files:** Always save `.pem` files using a terminal text editor (`nano`, `vim`) or by piping output directly to a file. Do **not** use TextEdit — it saves as Rich Text Format (RTF) by default, which wraps the certificate in formatting code that OpenSSL cannot read. If you must use a GUI editor, go to Format → Make Plain Text before saving.

---

## Part 2 — Inspect Each Certificate

### Step 3 — Run inspection on each file

```bash
openssl x509 -in server.pem -text -noout
openssl x509 -in intermediate.pem -text -noout
openssl x509 -in root.pem -text -noout
```

For each certificate, locate:

- **Subject** — who the cert is issued to
- **Issuer** — who signed it
- **Basic Constraints** — whether it can issue other certs (`CA:TRUE` or `CA:FALSE`)
- **Key Usage** — what it is allowed to do

### Step 4 — Identify the role of each certificate

| Role | Indicator |
|---|---|
| Root CA | `CA:TRUE` — Issuer = Subject (self-signed) |
| Intermediate CA | `CA:TRUE` — Issuer points to Root CA |
| Leaf Certificate | `CA:FALSE` — Issuer points to Intermediate CA |

---

## Part 3 — Verify the Chain

### Step 5 — Run chain verification

```bash
openssl verify -CAfile root.pem -untrusted intermediate.pem server.pem
```

Expected output:

```
server.pem: OK
```

This confirms the server certificate is trusted through the full chain. If you see an error, check that each `.pem` file contains only one complete certificate block.

> **Windows users — if you get `unable to get issuer certificate`:**
> This is not a setup error. OpenSSL on Windows does not use Windows Certificate Manager to complete a certificate chain. When `root.pem` chains to another CA (Subject ≠ Issuer), Windows OpenSSL cannot resolve the missing root — even if the same command works on Mac. Mac's LibreSSL quietly fills the gap using the system Keychain; Windows OpenSSL does not.
>
> **Fix — download Mozilla's CA bundle and use it as your CAfile:**
>
> ```powershell
> Invoke-WebRequest -Uri "https://curl.se/ca/cacert.pem" -OutFile cacert.pem
> openssl verify -CAfile cacert.pem -untrusted intermediate.pem server.pem
> ```
>
> Expected output: `server.pem: OK`
>
> This is a trusted, maintained bundle of all major root CAs — the same one curl uses internally. Document this in your Challenges / Troubleshooting section. Understanding why the same command behaves differently across platforms is part of what this lab is designed to build.

---

## Part 4 — Record Your Observations

Answer the following in your submission file (see Submission section below):

- Which certificate is the root CA?
- Which certificate is the intermediate CA?
- Which certificate is the leaf certificate?
- How does the Issuer field connect the certificates in the chain?
- Why do intermediate certificates exist instead of the root CA signing everything directly?

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `server.pem` | Leaf certificate from GitHub |
| `intermediate.pem` | Intermediate CA certificate |
| `root.pem` | Root CA certificate |
| `lab-03-certificate-chain.md` | Your completed observations |

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

Create `lab-03-certificate-chain.md` in `labs/week-03/submissions/` and fill in the template:

```markdown
# Lab 03 — Verify a Certificate Chain

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

---

## Environment
- OS:
- Terminal used (Mac Terminal / Git Bash / WSL):
- OpenSSL version (`openssl version`):
- Website used: github.com

---

## Chain Verification Result
Paste the output of your `openssl verify` command:

---

## Certificate Roles

| Certificate | Subject | Issuer | CA:TRUE/FALSE |
|---|---|---|---|
| server.pem | | | |
| intermediate.pem | | | |
| root.pem | | | |

---

## Observations

1. Which certificate is the root CA?
2. Which is the intermediate CA?
3. Which is the leaf certificate?
4. How does the Issuer field connect the chain?
5. Why do intermediate certificates exist?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.
```

---

### Step 3 — Confirm your folder structure

```
labs/
  week-03/
    submissions/
      server.pem
      intermediate.pem
      root.pem
      lab-03-certificate-chain.md
```

---

### Step 4 — Commit and push to GitHub

Choose **one** of the following methods:

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-03/submissions/
git commit -m "Week 3 Lab 03 — Verify certificate chain"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-03/submissions/`
3. Click **Add file → Upload files**
4. Drag and drop `server.pem`, `intermediate.pem`, `root.pem`, and `lab-03-certificate-chain.md` into the upload area
5. Scroll down to **Commit changes**, enter a message like `Week 3 Lab 03 — Verify certificate chain`, and click **Commit changes**

> **Don't see the submissions folder yet?** Click **Add file → Create new file**, then type the full path in the filename field: `labs/week-03/submissions/lab-03-certificate-chain.md`. This creates the folder and file at the same time. Then upload the `.pem` files separately using **Add file → Upload files**.

---

### Step 5 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 3 → Lab 03: Certificate Chain**
3. Click **Copy** next to the file path — confirm your file is at that exact location
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker.

---

## Stretch Goal (Optional)

Try verifying the server certificate **without the intermediate certificate:**

```bash
openssl verify -CAfile root.pem server.pem
```

Consider:

- Does verification fail?
- Why is the intermediate certificate necessary?
- How do browsers obtain missing intermediate certificates?

---

## Why This Matters

Every TLS connection relies on certificate chain validation. Browsers verify that:

1. The server certificate is valid
2. It was issued by a trusted intermediate CA
3. The intermediate traces back to a trusted root CA

If any link in the chain fails, the connection is rejected.

---

*CVI PKI Career Pathway — Foundations Phase*
