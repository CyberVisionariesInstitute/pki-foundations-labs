# Lab 01 — Diagnose an Expired Certificate

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## The Scenario

Metro General Hospital runs a patient-facing appointment portal at `portal.metrogeneral.org`. The IT help desk has started receiving calls — patients are seeing security warnings when they try to log in. The security team confirms TLS is failing.

You are the on-call PKI engineer. Your first job is not to fix anything yet. Your first job is to diagnose.

You will use the PKI Diagnostic Framework from this week's lessons to work through the failure systematically and document what you find.

---

## Objectives

By the end of this lab, you will have:

- Retrieved a live expired certificate using `openssl s_client`
- Parsed the certificate and identified the exact failure using `openssl x509`
- Worked through all four steps of the PKI Diagnostic Framework
- Documented the remediation path in a structured incident summary

---

## Prerequisites

- Weeks 1–5 complete
- OpenSSL installed and accessible from your terminal
- This lab uses `badssl.com/expired` — a public test site maintained specifically for practicing TLS failure scenarios

---

## The PKI Diagnostic Framework — Your Checklist

Apply each step in order. Do not skip ahead.

| Step | What You Do |
|---|---|
| Step 1 — Retrieve | Connect to the target and capture the certificate |
| Step 2 — Parse | Read the certificate fields — find the failure |
| Step 3 — Validate the chain | Confirm whether the chain itself is intact |
| Step 4 — Check revocation and trust | Rule out other contributing failures |

---

## Step 1 — Retrieve the Certificate

Connect to the target and capture the certificate in PEM format so you can inspect it.

```bash
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | openssl x509 -outform PEM > expired_cert.pem
```

> **Note:** `badssl.com` uses subdomains for each failure type. The expired certificate lives at `expired.badssl.com`. You may see an SSL error in the output — that is expected. The goal is to retrieve the certificate itself, not to establish a trusted connection.

**What to record:**

- Did the command return a certificate? (yes/no)
- Did OpenSSL report an error during the connection? What was it?

---

## Step 2 — Parse the Certificate

Now read the certificate you just retrieved.

```bash
openssl x509 -in expired_cert.pem -text -noout
```

Focus on the following fields in the output:

**Validity block:**

```
Validity
    Not Before: [date]
    Not After:  [date]
```

**Quick expiration check:**

```bash
openssl x509 -in expired_cert.pem -noout -dates
```

**What to record:**

- What are the `Not Before` and `Not After` dates?
- Is the certificate currently within its validity period?
- Who is the Subject (the entity the certificate was issued to)?
- Who is the Issuer?

---

## Step 3 — Validate the Chain

Even though we already know there is an expiration issue, a real incident can have multiple contributing failures. Check the chain.

```bash
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null
```

Scroll through the output and look for:

- How many certificates are in the chain? (look for multiple `-----BEGIN CERTIFICATE-----` blocks)
- Is there an intermediate CA present?
- Does the chain terminate at a trusted root?

**What to record:**

- Is the certificate chain structurally complete — leaf → intermediate → root?
- Any chain-related errors separate from the expiration failure?

---

## Step 4 — Check Revocation and Trust

An expired certificate cannot be used, but it is worth checking whether revocation also played a role.

**Locate the OCSP responder URL in the certificate:**

```bash
openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"
```

**What to record:**

- Is an OCSP URL present in the certificate?
- For an expired certificate, is revocation checking relevant to the remediation? Why or why not?

---

## Your Submission

Use `_templates/lab-submission-w6-template.md` for your write-up. Copy it into your portfolio repo, rename it to `lab-01-expired-certificate.md`, and fill it out as you work through each step above.

For the **Incident Summary** section of the template, use `portal.metrogeneral.org (simulated via expired.badssl.com)` as the target system. The Evidence section should include the `Not Before` and `Not After` dates, how many days ago the certificate expired, and your chain status finding. The Remediation path should be step-by-step — not just "get a new certificate."

---

## Connecting to Prior Weeks

- The `Not Before` / `Not After` fields you read in Step 2 are part of the X.509 structure you studied in **Week 3**
- The PEM format you captured the certificate in was introduced in **Week 4**
- The lifecycle failure you diagnosed here — expiration — is the same failure type discussed in **Week 5, Lesson 3**

This is not a new concept. You are now diagnosing it in a real failure scenario.

---

## Submission

Save your completed write-up as `lab-01-expired-certificate.md` in:

```
labs/week-06/submissions/expired-certificate/
```

Commit your `expired_cert.pem` file alongside the write-up.

Log your submission at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app) under Week 6 — Lab 01.

---

*CVI PKI Career Pathway — Foundations Phase*
