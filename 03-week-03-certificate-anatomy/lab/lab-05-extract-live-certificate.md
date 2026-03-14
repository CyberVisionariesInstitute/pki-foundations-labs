# Lab — Extract a Certificate from a Live Website (Optional)

## Goal

This optional lab demonstrates how to retrieve and inspect a live TLS certificate from a website.

You will:
- Connect to a website using OpenSSL
- Retrieve the certificate presented during the TLS handshake
- Save the certificate locally
- Inspect the certificate fields and extensions

This process mirrors how security engineers analyze certificates during **TLS troubleshooting and incident response.**

---

## Part 1 — Setup

### Prerequisites
- OpenSSL installed
- Access to a local terminal (Mac Terminal, PowerShell, Git Bash, or WSL)
- Completion of **Lab 01 and Lab 02 recommended**

All commands must be executed locally.

GitHub’s web interface cannot run OpenSSL commands.

---

## Part 2 — Execution Steps

### Step 1 — Create Artifact Directory
From the root of your repository on your personal machine:

mkdir -p lab/03-week-03-certificate-anatomy/submissions/live-certificate-analysis

---

### Step 2 — Connect to a Website
Use OpenSSL to establish a TLS connection and retrieve the certificate chain.

Run:

  openssl s_client -connect github.com:443 -showcerts

You will see several certificate blocks printed in the terminal.

Each certificate block will look like:

-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----

The **first certificate in the list** is the **leaf certificate** used by the website.

---

### Step 3 — Save the Certificate
Copy the first certificate block and save it as:

  github_cert.pem

Place the file inside:

lab/03-week-03-certificate-anatomy/submissions/live-certificate-analysis/

---

### Step 4 — Inspect the Certificate
Run the following command to view the certificate details:

  openssl x509 -in lab/03-week-03-certificate-anatomy/submissions/live-certificate-analysis/github_cert.pem -text -noout

This command converts the encoded certificate into a human-readable format.

---

### Step 5 — Analyze the Certificate
Identify the following information:
- **Subject**
- **Issuer**
- **Validity Period**
- **Public Key Algorithm**
- **Subject Alternative Name (SAN)**
- **Key Usage**
- **Extended Key Usage**

These fields define the **identity and usage rules** of the certificate.

---

## Part 3 — Misconfiguration Scenarios
In your repo create a file named:

  live-certificate-analysis.md

Inside the same folder:

  lab/03-week-03-certificate-anatomy/submissions/live-certificate-analysis/

Document:
- The organization the certificate belongs to
- Which certificate authority issued the certificate
- The expiration date of the certificate
- The domains listed in the SAN field
- What the certificate is allowed to be used for

---

## Submission (Portfolio Repo)
Ensure the following files exist:

labs/03-week-03-certificate-anatomy/submissions/live-certificate-analysis/
  github_cert.pem
  live-certificate-analysis.md

Example commit message:

  Week 3 Lab 05 — Extract and analyze live TLS certificate

---

## Stretch (Optional)
Retrieve certificates from additional websites:

  openssl s_client -connect google.com:443 -showcerts
  openssl s_client -connect cloudflare.com:443 -showcerts

Compare the certificates and consider:
- Do they use the same certificate authority?
- Do they use the same public key algorithm?
- Do they contain similar SAN entries?

---

## Why This Matters
Security engineers frequently inspect certificates from live systems when troubleshooting:
- TLS connection failures
- Certificate expiration issues
- Misconfigured certificate chains
- Hostname validation errors

Being able to retrieve and analyze certificates directly from a TLS connection is an essential skill in **PKI operations and security engineering.**

---

CVI PKI Career Pathway — Foundations Phase
