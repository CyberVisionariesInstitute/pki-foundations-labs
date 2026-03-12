# Lab — Investigate Certificate Extensions

## Goal

This lab builds operational understanding of X.509 certificate extensions, which define how a certificate can be used and validated.

You will:
- Inspect certificate extensions using OpenSSL
- Identify common certificate extensions
- Understand how extensions control certificate behavior
- Analyze how extensions impact TLS validation

---

## Part 1 — Setup

### Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Completion of **Lab 01 — Inspect Certificate Fields**

All commands must be executed locally.  
GitHub’s web interface cannot run OpenSSL commands.

---

## Part 2 — Execution Steps

### Step 1 — Create Artifact Directory
From the root of your directory:
mkdir -p labs/03-week-03-certificate-anatomy/submissions/certificate-extensions

---

### Step 2 — Use the Certificate From Lab 01
You will reuse the certificate retrieved in **Lab 01.**

Expected location:

labs/03-week-03-certificate-anatomy/submissions/certificate-inspection/leaf_cert.pem

Confirm the file exists before continuing.

---

### Step 3 — Inspect the Certificate Extensions
Run the following command:

openssl x509 -in labs/03-week-03-certificate-anatomy/submissions/certificate-inspection/leaf_cert.pem -text -noout

Scroll through the output until you find the **X509v3 extensions section.**

---

### Step 4 — Identify Key Extensions
Locate and record the following extensions:

**Subject Alternative Name (SAN)**

Example:

X509v3 Subject Alternative Name:
DNS:google.com, DNS:www.google.com

This extension defines **which domains the certificate is valid for.**

---

### Key Usage

Example:

X509v3 Key Usage:
Digital Signature, Key Encipherment

This extension defines **which cryptographic operations the certificate can perform.**

---

### Extended Key Usage (EKU)
Example:

  X509v3 Extended Key Usage:
  TLS Web Server Authentication

This extension defines **what the certificate is allowed to authenticate.**

---

### Basic Constraints
Example:

  X509v3 Basic Constraints:
  CA:FALSE

This field determines whether the certificate can act as a **Certificate Authority.**

---

## Part 3 — Observations
Document the following in your **Week 3 lab notes**:
- What domains appear in the SAN field
- What operations are listed in Key Usage
- What applications are listed in Extended Key Usage
- Whether the certificate can issue other certificates
- Why these extensions are important for TLS validation

---

### Submission (Portfolio Repo)
Ensure the following file exists:

labs/03-week-03-certificate-anatomy/submissions/certificate-extensions/

Create a file documenting your observations.

Example:

  extensions-analysis.md

Your file should summarize the extensions you identified and what they mean.

---

### Example Artifact Structure

labs

   03-week-03-certificate-anatomy
   
        submissions
        
          certificate-inspection
          
            leaf_cert.pem
            
          certificate-extensions
          
            extensions-analysis.md

---

## Stretch (Optional)
Retrieve a certificate from a different website:

  openssl s_client -connect github.com:443 -showcerts

Inspect the extensions and compare them to the certificate used in this lab.

Questions to consider:
- Do both certificates contain similar SAN entries?
- Do both certificates allow the same Extended Key Usage?
- Are there any additional extensions present?

---

CVI PKI Career Pathway — Foundations Phase

