# Lab — Inspect X.509 Certificate Fields

## Goal

This lab builds operational understanding of how to read and interpret the core fields of an X.509 certificate.

You will:

- Retrieve a certificate from a live website
- Parse the certificate using OpenSSL
- Identify the core certificate fields
- Understand how certificates represent digital identity in PKI systems

---

## Part 1 — Setup

### Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Your Week 3 portfolio folder created

All commands must be executed locally.  
GitHub’s web interface cannot run OpenSSL commands.

---

## Part 2 — Execution Steps 

### Step 1 — Create Artifact Directory
From the root of your local directory on your personal machine:
mkdir -p lab/03-week-03-certificate-anatomy/submissions/certificate-fields

### Step 2 — Retrieve a Website Certificate
Use OpenSSL to connect to a website and retrieve its certificate.

openssl s_client -connect google.com:443 -showcerts

You will see several certificates displayed in the terminal.

Locate the **first certificate block**, which represents the **leaf certificate.**

It will look similar to:

-----BEGIN CERTIFICATE-----
MIIF...
-----END CERTIFICATE-----

Copy the entire certificate block.

Save it as:

leaf_cert.pem

Place the file in:

lab/03-week-03-certificate-anatomy/submissions/certificate-fields/

### Step 3 — Parse the Certificate
Use OpenSSL to inspect the certificate contents.

openssl x509 -in lab/03-week-03-certificate-anatomy/submissions/certificate-fields/leaf_cert.pem -text -noout

This command converts the encoded certificate into a human-readable format.

### Step 4 — Identify Core Certificate Fields
Locate and record the following fields in the output:
- Version
- Serial Number
- Signature Algorithm
- Issuer
- Subject
- Validity Period (Not Before / Not After)
- Public Key Algorithm

These fields define the **identity and trust attributes** of the certificate.

---

## Part 3 — Observations
Document the following in your Week 3 lab notes:
- Who issued the certificate
- What organization or domain the certificate represents
- What public key algorithm is used
- When the certificate expires
- Why the issuer field is important in PKI

### Submission (Portfolio Repo)
Ensure the following file exists:

lab/03-week-03-certificate-anatomy/submissions/certificate-fields/
  leaf_cert.pem

Commit and push your changes.

Example commit message:

  Week 3 Lab 01 — Inspect Certificate Fields

## Stretch (Optional)
Try retrieving a certificate from a different website:

openssl s_client -connect github.com:443 -showcerts

Compare the certificate fields.

Questions to consider:
- Do the issuer fields match?
- Do both certificates use the same public key algorithm?
- Do the validity periods differ?

---

CVI PKI Career Pathway — Foundations Phase


