# Lab — Verify a Certificate Chain

## Goal

This lab builds operational understanding of how certificate chains establish trust in Public Key Infrastructure.

You will:
- Inspect multiple certificates in a trust hierarchy
- Identify the root, intermediate, and leaf certificates
- Verify a certificate chain using OpenSSL
- Understand how browsers validate certificate trust

---

## Part 1 — Setup

### Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Completion of **Lab 01 and Lab 02**

All commands must be executed locally.  
GitHub’s web interface cannot run OpenSSL commands.

---

## Part 2 — Execution Steps

### Step 1 — Create Artifact Directory
From the root of your directory on your personal machine:
- mkdir -p lab/03-week-03-certificate-anatomy/submissions/certificate-chain

---

### Step 2 —Obtain a Certificate Chain
Use OpenSSL to retrieve a certificate chain from a website.

Run:

openssl s_client -connect github.com:443 -showcerts

You will see **multiple certificates** returned.

Typically:

1. Leaf certificate (website)
2. Intermediate certificate
3. Root certificate (sometimes omitted)

Each certificate block looks like:

-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----.

---

### Step 3 — Save the Certificates
Create the following files inside:

- lab/03-week-03-certificate-anatomy/submissions/certificate-chain/
    - server.pem
    - intermediate.pem
    - root.pem

**Save each certificate separately.**

If the root certificate is not included in the output, download it from the issuing CA’s website.

---

### Step 4 — Inspect Each Certificate
Run the following command for each certificate:

openssl x509 -in server.pem -text -noout

Repeat for:

intermediate.pem
root.pem

Observe the following fields:
- **Subject**
- **Issuer**
- **Basic Constraints**
- **Key Usage**

These fields help determine the role of each certificate.

---

### Step 5 — Identify Certificate Roles
Determine which certificate represents:

**Root CA**	 - Trust anchor stored in operating systems
**Intermediate CA**	- Certificate authority that issues leaf certificates
**Leaf Certificate** - The certificate used by the website

A key indicator:

  Basic Constraints: CA:TRUE

Certificates with **CA:TRUE** can issue other certificates.

Leaf certificates typically contain:

  CA:FALSE

---

### Step 6 — Verify the Certificate Chain
Now verify the certificate chain using OpenSSL.

Run:

  openssl verify -CAfile root.pem -untrusted intermediate.pem server.pem

Expected output:

server.pem: OK

This confirms that the server certificate can be trusted through the chain.

---

## Part 3 — Observations
Document the following in your **Week 3 lab notes**:
- Which certificate is the root CA
- Which certificate is the intermediate CA
- Which certificate is the leaf certificate
- How the issuer field connects the certificates
- Why intermediate certificates exist

---

### Submission (Portfolio Repo)
Ensure the following files exist:

lab/03-week-03-certificate-anatomy/submissions/certificate-chain/
  - server.pem
  - intermediate.pem
  - root.pem

Update your certificate-chain.md file in your submissions folder with your observations.

Example commit message:

Week 3 Lab 03 — Verified certificate chain

---


## Stretch (Optional)
Try verifying the server certificate **without the intermediate certificate.**

Run:

  openssl verify -CAfile root.pem server.pem

Questions to consider:
- Does verification fail?
- Why is the intermediate certificate necessary?
- How do browsers obtain missing intermediate certificates?

---

## Why This Matters
Every TLS connection relies on **certificate chain validation.**

Browsers must verify that:

1. The server certificate is valid
2. It was issued by a trusted intermediate CA
3. The intermediate traces back to a trusted root CA

If any link in the chain fails, the connection is rejected.

---

CVI PKI Career Pathway — Foundations Phase
