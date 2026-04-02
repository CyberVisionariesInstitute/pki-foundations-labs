# Lab 02 — Diagnose a Broken Certificate Chain

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## The Scenario

Metro General's radiology team uses a third-party imaging platform that connects over TLS to an internal server. After a certificate renewal last week, the connection started failing. The vendor is saying the certificate looks fine on their end. The Metro General IT team is stuck.

You pull up the error: `unable to verify the first certificate`.

You know what that means. The chain is broken.

---

## Objectives

By the end of this lab, you will have:

- Observed a certificate chain validation failure caused by a missing intermediate CA
- Identified which certificate is missing and where it belongs in the chain
- Retrieved the missing intermediate from a public CA repository
- Validated the repaired chain using `openssl verify`
- Documented the failure and fix in a structured incident summary

---

## Prerequisites

- Lab 01 complete (you have practiced retrieving and parsing certificates)
- OpenSSL installed
- This lab uses `incomplete-chain.badssl.com` — a test site that serves a certificate without its intermediate CA

---

## Background: Why Certificate Chains Break

A certificate chain runs from the leaf certificate (the one issued to the server) up to a trusted root CA. In between, there is typically at least one intermediate CA.

When a server is configured correctly, it sends:

```
Leaf certificate (for the server)
    ↑ signed by
Intermediate CA certificate
    ↑ signed by
Root CA certificate (trusted by the client's trust store)
```

If the intermediate CA certificate is missing from the server's configuration, the client cannot build the chain to the root. Even if the root is trusted, the connection fails because the client has no way to verify the leaf certificate.

This is one of the most common PKI misconfigurations in enterprise environments — the certificate is valid, the root is trusted, but the intermediate was never installed on the server.

---

## The PKI Diagnostic Framework — Your Checklist

| Step | What You Do |
|---|---|
| Step 1 — Retrieve | Connect to the target and observe the chain (or lack of it) |
| Step 2 — Parse | Read the leaf certificate fields — confirm it is otherwise valid |
| Step 3 — Validate the chain | Reproduce the chain failure, identify the missing piece |
| Step 4 — Check revocation and trust | Confirm this is purely a chain issue, not a revocation or trust store problem |

---

## Step 1 — Retrieve the Certificate

Connect to the test target and capture the leaf certificate:

```bash
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | openssl x509 -outform PEM > leaf_cert.pem
```

Now run the connection again with full output visible so you can see the chain:

```bash
openssl s_client -connect incomplete-chain.badssl.com:443 </dev/null 2>&1
```

Look for the `Certificate chain` section near the top of the output. It will list what the server actually sent.

**What to record:**

- How many certificates did the server send?
- What does the `Verify return code` say at the bottom of the output?

---

## Step 2 — Parse the Leaf Certificate

Read the certificate you captured:

```bash
openssl x509 -in leaf_cert.pem -text -noout
```

Focus on these fields:

**Validity** — is the certificate currently valid?

**Issuer** — who signed this certificate?

```
Issuer: C=..., O=..., CN=...
```

The Issuer field tells you which intermediate CA signed this leaf. That intermediate CA is what is missing.

**Authority Information Access** — this extension may contain a URI where the issuing CA's certificate can be downloaded:

```bash
openssl x509 -in leaf_cert.pem -noout -text | grep -A3 "Authority Information Access"
```

Look for a line starting with `CA Issuers - URI:`. That URL points to the intermediate CA certificate.

**What to record:**

- Is the leaf certificate itself valid (dates, subject, SAN)?
- What is the Issuer CN?
- Is there a CA Issuers URI in the Authority Information Access extension?

---

## Step 3 — Validate the Chain (and Observe the Failure)

Try to verify the leaf certificate against your system's default trust store:

```bash
openssl verify leaf_cert.pem
```

You will get an error — something like `unable to get local issuer certificate` or `unable to verify the first certificate`. This confirms the chain is broken.

**Now retrieve the missing intermediate:**

If you found a CA Issuers URI in Step 2, download the intermediate certificate:

```bash
curl -s "https://[URI from CA Issuers extension]" -o intermediate.der
openssl x509 -inform DER -in intermediate.der -out issuer_cert.pem
```

> **Note:** CA Issuers certificates are sometimes served as DER (binary) format. The second command converts it to PEM so OpenSSL can use it.

If the URI is not available, search for the intermediate CA by its CN name using the CA's public certificate repository. DigiCert, Let's Encrypt, and other public CAs maintain public repositories of their intermediate certificates.

**Now verify with the intermediate included:**

```bash
openssl verify -untrusted issuer_cert.pem leaf_cert.pem
```

**What to record:**

- What error did `openssl verify leaf_cert.pem` produce before you added the intermediate?
- After adding the intermediate with `-untrusted`, did verification succeed?
- What does the `-untrusted` flag mean in this context? Why is the intermediate "untrusted"?

---

## Step 4 — Check Revocation and Trust

Confirm this is purely a chain configuration issue, not a revocation or trust anchor problem.

**Check that the root CA is in your system trust store:**

```bash
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null \
  | grep "Verify return code"
```

**What to record:**

- Once the chain is repaired, is the root CA trusted by your system?
- Is there any indication of a revocation issue?

---

## Your Submission

Use `_templates/lab-submission-w6-template.md` for your write-up. Copy it into your portfolio repo, rename it to `lab-02-broken-chain.md`, and fill it out as you work through each step above.

For the **Incident Summary** section, use `Radiology imaging platform (simulated via incomplete-chain.badssl.com)` as the target system. The Evidence section should include the leaf certificate subject, the Issuer CN (the missing intermediate), the exact `openssl verify` error before the fix, and the result after. The Root Cause field matters here — be specific about whether this was a certificate problem or a server configuration problem, and why that distinction matters.

---

## The Key Insight: Certificate Valid, Chain Broken

This failure type trips up a lot of engineers because the certificate itself is fine. It is not expired. It is not revoked. The hostname matches. But the connection still fails.

The root cause is a server configuration problem, not a certificate problem. The server was not configured to send the full chain. After a certificate renewal, intermediate CA certificates sometimes need to be explicitly re-installed on the server alongside the new leaf certificate — and this step is easy to skip.

This is why the PKI Diagnostic Framework requires you to validate the chain explicitly in Step 3, even if Steps 1 and 2 look clean.

---

## Connecting to Prior Weeks

- The chain structure you validated here — leaf → intermediate → root — was first introduced in **Week 2** (cryptographic trust) and reinforced in **Week 3** (X.509 anatomy)
- The DER-to-PEM conversion you performed on the intermediate certificate used the format conversion skills from **Week 4**
- The Authority Information Access extension you read to find the intermediate URI was first visible in **Week 3's** certificate extension lab

---

## Submission

Save your completed write-up as `lab-02-broken-chain.md` in:

```
labs/week-06/submissions/broken-chain/
```

Commit your `issuer_cert.pem` file alongside the write-up.

Log your submission at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app) under Week 6 — Lab 02.

---

*CVI PKI Career Pathway — Foundations Phase*
