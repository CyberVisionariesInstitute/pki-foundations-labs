# Lab — Certificate Inspection (Beginner-Friendly)

## Goal
Learn how to identify what a certificate is asserting and how browsers validate trust.

You will inspect a real TLS certificate and document what you find.

---

## Part 1 — Inspect a Website Certificate
1. Open a browser.
2. Navigate to a TLS-enabled site (examples: a bank site, a major retailer, or your preferred secure site).
3. Click the padlock icon in the address bar.
4. View the certificate details.

Capture screenshots of:
- Subject (who the cert is issued to)
- Issuer (who issued it)
- Validity period (Not Before / Not After)
- Subject Alternative Names (SANs), if shown
- Signature algorithm (if shown)

---

## Part 2 — Record Key Observations
Answer these in your notes:
- What identity is the certificate asserting?
- Does the DNS name you visited appear in the certificate?
- Who is the issuing CA?
- What is the validity period (how long is it valid)?
- What might happen if the certificate is expired?

---

## Part 3 — Trust Chain (High-Level)
If your browser shows a “Certification Path” or “Chain”:
- Note how many certificates are in the chain
- Identify the Root CA vs Intermediate CA (if visible)

You do NOT need to deeply understand the chain yet — just observe it.

---

## Submission (in your Portfolio Repo)
Create:
- `labs/week-01/certificate-inspection.md`

Include:
- Screenshots
- Bullet notes
- A short paragraph: “What this certificate proves (and what it does not prove)”
