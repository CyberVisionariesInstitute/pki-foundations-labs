# Mini Lab — Trust Chain Validation

## Goal
Reinforce how digital trust is verified by tracing a certificate chain to a trusted root (trust anchor).

This mini-lab connects directly to Lesson 4: How Digital Trust Is Verified.

---

## Part 1 — Context

In Lesson 4, you learned:
- Trust is delegated through Certificate Authorities
- Validation walks the certificate chain upward
- Trust anchors live in the OS/browser root store
- Systems verify trust using a checklist

This exercise allows you to observe that validation process in a real HTTPS connection.

---

## Part 2 — Execution Steps
1. Visit any HTTPS website (e.g., a major public site).
2. Open the certificate details in your browser.
3. Locate the Certification Path (certificate chain).
4. Identify:
  - The Leaf (server) certificate
  - The Intermediate CA
  - The Root CA
5. Confirm whether the Root CA is marked as trusted by your system.

---

## Part 3 — Observations

Students should document:
- The names of the Leaf, Intermediate, and Root certificates
- Whether the Root CA is trusted by the system
- How trust flows from the Root down to the server certificate
- What would happen if the Root CA were not trusted

Keep your explanation clear and concise.

---

## Submission (Portfolio Repo)

Create or update:

labs/week-01/trust-chain-validation.md

Include:
- A screenshot of the certification path
- Brief notes identifying each certificate in the chain
- A 1-3 sentence explanation of:
  - Why the Root certificate is called a trust anchor
  - How the browser determines whether to trust the site
    
Commit with a clear message, for example:

Add Week 1 mini lab: trust chain validation

## Insight Check
You should be able to clearly explain:
- Why the Root CA is the trust anchor
- Why the Intermediate CA is trusted
- How validation walks the chain upward
- Why trust fails if any link in the chain breaks

---

## Stretch (Optional)

Inspect a second HTTPS site and compare:
- Do they use the same Root CA?
- Do they use the same Intermediate?
- What does that tell you about certificate issuance at scale?
