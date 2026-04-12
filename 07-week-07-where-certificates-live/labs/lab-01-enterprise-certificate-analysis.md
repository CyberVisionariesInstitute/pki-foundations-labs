# Lab 01 — Analyze a Live Enterprise Certificate Deployment

**Week 7 · PKI in Enterprise Environments & Your Career Path**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## The Scenario

Metro General Hospital is completing their pre-migration certificate audit. The PKI engineer has finished mapping the internal environment — web servers, load balancers, APIs, VPN clients, device certificates, code signing.

Now the CISO wants a second analysis: a structured profile of how other enterprise organizations manage their certificate deployments. Before Metro General modernizes their PKI architecture, it is useful to understand what mature enterprise PKI looks like in production.

Your assignment: choose a well-known enterprise organization — a major bank, a large SaaS platform, or a cloud provider — and produce a complete certificate analysis of their public-facing TLS deployment. This is not a diagnostic exercise. It is an infrastructure assessment.

---

## Objectives

By the end of this lab, you will have:

- Retrieved and inspected a live enterprise TLS certificate deployment using the full diagnostic framework
- Identified the CA type, certificate chain, and certificate profile of a real production deployment
- Determined where TLS appears to terminate — app server, load balancer, or CDN
- Documented your findings in a structured analysis formatted as an infrastructure assessment

---

## Prerequisites

- Weeks 1–6 complete
- OpenSSL installed and accessible from your terminal
- SSL Labs (ssllabs.com) — used for cross-validation alongside OpenSSL
- crt.sh — used for certificate transparency log analysis

---

## Choosing Your Target

Choose a well-known enterprise site from one of these categories:

**Financial services:** A major bank's main web presence (e.g., chase.com, wellsfargo.com, bankofamerica.com)

**Cloud / SaaS:** A major cloud provider's console or a large SaaS platform (e.g., console.aws.amazon.com, app.datadog.com, salesforce.com)

**Healthcare / Enterprise:** A large healthcare organization or enterprise SaaS (e.g., epic.com, workday.com, servicenow.com)

> **Choose one hostname.** You will analyze the primary HTTPS endpoint for that organization thoroughly rather than sampling multiple endpoints shallowly.

Record your chosen hostname here before beginning.

**Target hostname:** _______________________________________________

---

## Step 1 — Retrieve the Certificate

Connect to the live endpoint and capture the certificate.

```bash
openssl s_client -connect [hostname]:443 -showcerts 2>/dev/null \
  | openssl x509 -outform PEM > enterprise_cert.pem
```

Also retrieve the full chain:

```bash
openssl s_client -connect [hostname]:443 -showcerts 2>/dev/null > full_chain_output.txt
```

**What to record:**

- Did the connection succeed?
- How many certificates are in the chain output? (count the `-----BEGIN CERTIFICATE-----` blocks)

---

## Step 2 — Parse the Certificate

Read the certificate and document all key fields.

```bash
openssl x509 -in enterprise_cert.pem -text -noout
```

**What to record:**

**Validity:**
- Not Before: _______________
- Not After: _______________
- Approximate remaining validity (in days): _______________

**Subject:**
- Common Name (CN): _______________
- Organization (O): _______________

**Subject Alternative Names (SAN):**
List all DNS entries in the SAN extension:

```bash
openssl x509 -in enterprise_cert.pem -noout -text | grep -A10 "Subject Alternative Name"
```

- How many SAN entries are there?
- Are there wildcard entries (e.g., `*.example.com`)? What do they suggest about the architecture?

**Issuer:**
- Issuer CN: _______________
- Is this a known public CA? Which one?

**Certificate Type:**
- Is the certificate DV (domain validation), OV (organization validation), or EV (extended validation)? How can you tell?

---

## Step 3 — Analyze the Certificate Chain

Examine the full chain and identify each certificate in the trust path.

```bash
openssl s_client -connect [hostname]:443 -showcerts 2>/dev/null
```

Look for the `Certificate chain` section near the top of the output. Identify each certificate by its Subject.

**What to record:**

- How many certificates are in the chain?
- Is a leaf certificate present?
- Is an intermediate CA certificate present? What is its Subject?
- Does the chain terminate at a known public root? Name it.
- Is the chain complete — leaf → intermediate → root? Or is any layer missing?

---

## Step 4 — Determine Where TLS Terminates

This step is the Week 7 addition. Use what you know from Lesson 3.

**Check for CDN or load balancer indicators:**

```bash
# Check the issuer — is it a CDN-specific CA or a known CDN provider's certificate?
openssl x509 -in enterprise_cert.pem -noout -issuer

# Check for CDN-related SAN entries or wildcard patterns
openssl x509 -in enterprise_cert.pem -noout -text | grep -A10 "Subject Alternative Name"

# Check server response headers for CDN indicators
curl -sI https://[hostname] | grep -i "server\|cf-ray\|x-cache\|via\|x-amz"
```

**What to record:**

- Does the issuer suggest this is a CDN-managed certificate (e.g., issued by Cloudflare, Fastly, DigiCert for a CDN provider)?
- Do the server headers indicate a CDN or load balancer is in the request path (e.g., `CF-Ray` for Cloudflare, `X-Cache` for CloudFront, `Via` header for a proxy)?
- Based on your findings, where do you believe TLS terminates for this deployment — at the application server, a load balancer, or a CDN edge?

---

## Step 5 — TLS Version and Cipher Suite

Run an SSL Labs analysis for a complete TLS configuration report.

1. Go to [https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)
2. Enter your target hostname
3. Wait for the analysis to complete (2–3 minutes)

**What to record:**

- SSL Labs overall grade: _______________
- TLS versions supported (TLS 1.0, 1.1, 1.2, 1.3): _______________
- Are deprecated TLS versions (1.0 or 1.1) still supported?
- Is HSTS (HTTP Strict Transport Security) configured?
- Is OCSP stapling enabled?

---

## Step 6 — Certificate Transparency Analysis

Check the certificate transparency logs to understand how long this organization has been using this CA and whether there are unexpected issuers.

1. Go to [https://crt.sh](https://crt.sh)
2. Search for your target domain
3. Review the most recent certificates listed

**What to record:**

- How many certificates have been issued for this domain (approximately)?
- Is the issuer consistent across recent certificates, or have different CAs been used?
- Are there any unexpected or unfamiliar issuers in the CT log? If yes, what could explain them?
- What is the approximate validity period of recent certificates? (This may indicate whether Let's Encrypt 90-day or a paid CA 1–2 year pattern is in use)

---

## Your Submission

Use `_templates/lab-submission-template.md` for your write-up. This lab does not use the incident summary template — it uses the standard template. Rename your copy to `lab-01-enterprise-certificate-analysis.md`.

Your submission should produce a structured **Certificate Profile** with the following sections:

**Target:** The hostname you analyzed and why you chose it.

**Certificate Summary:** Issuer, validity window, certificate type (DV/OV/EV), SAN count, wildcard usage.

**Chain Analysis:** Number of certificates in chain, intermediate CA identity, root CA identity, chain completeness.

**Termination Analysis:** Where TLS appears to terminate (app server / load balancer / CDN) and the evidence that supports your conclusion.

**TLS Configuration:** SSL Labs grade, TLS versions, HSTS, OCSP stapling.

**CT Log Analysis:** CA consistency, any unexpected issuers, certificate validity period pattern.

**Architecture Assessment:** In 2–3 sentences, describe what this certificate deployment tells you about the organization's PKI architecture and operational approach. This is not a grade — it is an observation.

---

## Connecting to Prior Weeks

- The certificate fields you are reading in Step 2 are the X.509 anatomy you studied in **Week 3**
- The chain validation in Step 3 uses the same trust path concepts from **Week 4**
- The OCSP stapling check in Step 5 connects to the revocation concepts from **Week 5** and **Week 6**
- The TLS termination analysis in Step 4 is the direct application of **Week 7, Lesson 3**

This is not a diagnostic exercise. You are profiling a certificate deployment — the kind of analysis a PKI engineer produces for a security assessment, a pre-migration audit, or a vendor review.

---

## Submission

Save your completed write-up as `lab-01-enterprise-certificate-analysis.md` in:

```
labs/week-07/submissions/enterprise-analysis/
```

Save the `enterprise_cert.pem` file alongside your write-up.

Log your submission at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app) under Week 7 — Lab 01.

---

*CVI PKI Career Pathway — Foundations Phase*
