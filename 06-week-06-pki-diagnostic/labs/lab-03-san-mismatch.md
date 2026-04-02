# Lab 03 — Diagnose a Hostname and SAN Mismatch

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## The Scenario

Metro General recently reorganized its internal web properties. The staff scheduling portal was moved from `scheduling.metrogeneral.org` to `staff.metrogeneral.org`. The infrastructure team updated the DNS record. What they did not do was request a new certificate.

Now staff members connecting to `staff.metrogeneral.org` are getting browser security errors. Someone on the IT team suggests "just adding a CNAME alias back to the old name." That would be wrong — and your job is to explain why.

---

## Objectives

By the end of this lab, you will have:

- Inspected a certificate with a SAN mismatch and reproduced the validation error
- Identified the exact mismatch between what the certificate says and what the client requested
- Explained why the SAN is the field that matters for hostname validation (not the Subject CN)
- Articulated why the correct fix is a new certificate — not a DNS workaround
- Documented your findings in a structured incident summary

---

## Prerequisites

- Labs 01 and 02 complete
- OpenSSL installed
- This lab uses `wrong.host.badssl.com` — a test site with a hostname that does not match its certificate's SAN

---

## Background: How Hostname Validation Works

When a TLS client connects to a server, it checks that the hostname it typed into the address bar matches what the certificate says. The field it checks is the **Subject Alternative Name (SAN)**, specifically the `DNS:` entries.

The Subject CN (`Common Name`) was historically used for hostname validation, but modern clients rely on the SAN. If the SAN is present and does not include the hostname being accessed, the connection fails — regardless of what the CN says.

A certificate that says `DNS: scheduling.metrogeneral.org` in its SAN cannot be used for `staff.metrogeneral.org`. These are cryptographically different identities. The CA signed the certificate for a specific hostname. That signature cannot be transferred.

---

## The PKI Diagnostic Framework — Your Checklist

| Step | What You Do |
|---|---|
| Step 1 — Retrieve | Connect to the target and capture the certificate |
| Step 2 — Parse | Read the SAN and Subject fields — find the mismatch |
| Step 3 — Validate the chain | Confirm whether the chain itself is valid |
| Step 4 — Check revocation and trust | Rule out other contributing failures |

---

## Step 1 — Retrieve the Certificate

Connect to the test target:

```bash
openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com \
  </dev/null 2>/dev/null | openssl x509 -outform PEM > mismatch_cert.pem
```

> **Note on `-servername`:** This flag sets the SNI (Server Name Indication) header, which tells the server which certificate to serve when a server hosts multiple certificates. It is good practice to always include this when using `openssl s_client`.

Also run the connection with full output to see the error:

```bash
openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com </dev/null 2>&1
```

**What to record:**

- What does the `Verify return code` say?
- Did OpenSSL report a hostname mismatch error?

---

## Step 2 — Parse the Certificate

Read the full certificate:

```bash
openssl x509 -in mismatch_cert.pem -text -noout
```

**Find the Subject line:**

```
Subject: C=..., ST=..., O=..., CN=...
```

**Find the Subject Alternative Name extension:**

```bash
openssl x509 -in mismatch_cert.pem -noout -text | grep -A5 "Subject Alternative Name"
```

This will show you the `DNS:` entries in the SAN. These are the hostnames this certificate is authorized to represent.

**What to record:**

- What is the Subject CN?
- What `DNS:` entries are listed in the SAN?
- What hostname were you connecting to (`wrong.host.badssl.com`)?
- Do any SAN entries match the hostname you connected to?

---

## Step 3 — Validate the Chain

Check whether the certificate chain itself is valid, independent of the hostname issue:

```bash
openssl verify mismatch_cert.pem
```

If the chain verifies, the problem is purely a hostname mismatch — not a chain or trust store issue.

**What to record:**

- Does the chain validate successfully?
- Is this a certificate problem, a chain problem, or a configuration/deployment problem?

---

## Step 4 — Check Revocation and Trust

```bash
openssl x509 -in mismatch_cert.pem -noout -text | grep -A2 "OCSP"
```

**What to record:**

- Is there an OCSP URL present?
- Is revocation relevant to this failure? Why or why not?

---

## The DNS Alias Question — Why It Would Not Work

Before writing your incident summary, make sure you can answer this clearly:

Someone on the Metro General IT team suggests: *"Instead of getting a new certificate, can we just add a CNAME alias so that `staff.metrogeneral.org` points back to `scheduling.metrogeneral.org`?"*

Work through why this does not fix the problem:

1. What does the browser check when validating a TLS certificate?
2. Does a CNAME change what hostname the browser thinks it is connecting to?
3. If the browser resolves `staff.metrogeneral.org` via a CNAME to `scheduling.metrogeneral.org`, which hostname does it validate the certificate against?
4. Does the existing certificate's SAN include `staff.metrogeneral.org`?

Your incident summary should include a clear, brief explanation of this logic — written so that a non-technical manager could understand why the workaround would not work.

---

## Your Submission

Use `_templates/lab-submission-w6-template.md` for your write-up. Copy it into your portfolio repo, rename it to `lab-03-san-mismatch.md`, and fill it out as you work through each step above.

For the **Incident Summary** section, use `Staff scheduling portal (simulated via wrong.host.badssl.com)` as the target system. The Evidence section should document the hostname accessed, the Subject CN, and the exact SAN entries from the certificate — make the mismatch explicit. This lab has an additional section not in the standard template: add a **"Why a DNS CNAME alias would not fix this"** subsection to your Remediation path and explain it clearly enough for a non-technical manager to follow.

---

## The Key Insight: You Cannot Rename a Certificate

A certificate is not a label you can move around. It is a cryptographic assertion — signed by a CA — that a specific public key belongs to a specific identity (hostname). When the hostname changes, the assertion is no longer valid. You need a new assertion, which means a new certificate.

The Subject CN may look like just a text field, but the SAN entries are what TLS clients actually enforce. Understanding this distinction — and being able to explain it clearly — is a fundamental PKI skill.

---

## Connecting to Prior Weeks

- The SAN extension you inspected was first introduced in **Week 3** (X.509 anatomy, extension types)
- In **Week 3 Lab 02**, you located the SAN in a certificate — this lab applies that skill to a real failure diagnosis
- The concept of the CA signing a certificate for a specific identity connects back to **Week 2** (trust and digital signatures)

---

## Submission

Save your completed write-up as `lab-03-san-mismatch.md` in:

```
labs/week-06/submissions/san-mismatch/
```

Log your submission at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app) under Week 6 — Lab 03.

---

*CVI PKI Career Pathway — Foundations Phase*
