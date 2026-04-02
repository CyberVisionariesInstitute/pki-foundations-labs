# Lab 04 (Stretch) — Full Diagnostic Scenario: Multi-Failure Investigation

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

> **This is a stretch lab.** It is optional but highly recommended. This is the closest simulation to real PKI operational work in Phase 1. If you are considering a career in security engineering, PKI administration, or identity engineering, do not skip this one.

---

## The Scenario

**Friday, 4:47 PM. You just got paged.**

Metro General's electronic health records (EHR) system is throwing TLS errors for clinical staff trying to access patient records from a new internal subnet. The helpdesk is getting calls. The clinical operations director has escalated it to the CISO.

The infrastructure team ran a quick check and said: *"The certificate looks fine."*

It does not look fine. Your job is to find out what is actually wrong — and document it clearly enough that the team can fix it without you walking them through every step.

You will receive a scenario description with clues. You will apply the full 4-step PKI Diagnostic Framework, work through each potential failure in diagnostic order, and produce a complete incident report.

---

## Objectives

By the end of this lab, you will have:

- Applied the full 4-step diagnostic framework to a scenario with multiple potential failure types
- Identified the correct failures in the right diagnostic order
- Practiced distinguishing between primary failures and contributing factors
- Written a complete incident report suitable for a real PKI incident

---

## Prerequisites

- Labs 01, 02, and 03 complete — you will draw on all three failure types
- OpenSSL installed
- Familiarity with the incident summary format from prior labs

---

## The Incident Brief

**System:** Metro General EHR portal, `ehr.metrogeneral.org`
**Reported behavior:** TLS failure when connecting from the `10.22.0.0/24` clinical subnet. Staff connecting from the main office network (`10.10.0.0/24`) are unaffected.
**Infrastructure team's note:** "The certificate was renewed last week. It was working fine before. The renewal shouldn't have changed anything."

**What you have been given:**

- The certificate was issued by Metro General's internal CA (`Metro General Internal CA - G2`)
- The new certificate was issued with a 1-year validity window
- The renewal replaced both the certificate and the private key (this was a full replacement, not a renewal)
- The internal CA root certificate (`Metro General Internal CA - G2 Root`) was distributed to all devices via Group Policy six months ago
- The clinical subnet (`10.22.0.0/24`) was added to the network two weeks ago — after the Group Policy push

---

## Your Investigation

Work through the 4-step framework. For each step, document what you checked, what you found, and what it ruled in or ruled out.

### Step 1 — Retrieve

Simulate retrieving the certificate from the EHR server. Since this is an internal system, use what you know from the scenario description to reason about what OpenSSL would return.

**Questions to answer in your write-up:**

- What command would you use to retrieve the certificate from an internal server?
- If the server is only accessible from certain subnets, what does that imply about your diagnostic approach?
- Based on the scenario, is there any reason to think the certificate itself was not successfully deployed?

### Step 2 — Parse

You retrieve the certificate. Based on the scenario, reason through what you would find.

**Questions to answer in your write-up:**

- The certificate was renewed last week with a new key. What field confirms a new key was used?
- The internal CA is `Metro General Internal CA - G2`. What would you check in the Issuer field?
- Is there any information in the scenario that suggests a validity date problem? Explain your reasoning.

### Step 3 — Validate the Chain

This is where the investigation gets interesting.

**Questions to answer in your write-up:**

- The EHR system works on the main office network but fails on the clinical subnet. The certificate and chain are the same in both cases. What does this tell you about where the failure is?
- The internal CA root was pushed via Group Policy six months ago. The clinical subnet was added two weeks ago. What is the likely state of the trust store on devices in the clinical subnet?
- What command would you use to test chain validation from a device on the clinical subnet?
- Is this a certificate problem, a chain problem, or a trust store problem? What is the distinction?

### Step 4 — Check Revocation and Trust

**Questions to answer in your write-up:**

- The certificate was renewed (full replacement). If the old certificate is still valid, should it be revoked? What is the operational risk if it is not?
- How would you verify whether the old certificate has been revoked?
- Is the revocation status of the old certificate relevant to the current incident?

---

## Identify the Failures — In Diagnostic Order

Before writing your incident report, list all the failures or contributing factors you identified, in the order a PKI engineer should address them.

Use this format:

```
1. [Primary failure — what is directly causing the TLS error for clinical subnet users]
   Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
   Evidence: [what in the scenario supports this conclusion]

2. [Contributing factor or secondary issue, if any]
   Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
   Evidence: [what in the scenario supports this conclusion]

3. [Additional issue to address, even if not the direct cause of the current incident]
   Type: [Certificate / Chain / Trust Store / Revocation / Configuration]
   Evidence: [what in the scenario supports this conclusion]
```

---

## Your Submission

Use `_templates/lab-04-incident-report-template.md` for your write-up. Copy it into your portfolio repo, rename it to `lab-04-full-diagnostic-stretch.md`, and complete every section.

This template is the document you would send to the Metro General CISO and IT team — write it so that a technically-informed manager can understand what happened, and so that the IT team can execute the fix without additional explanation from you. Every finding should include a Type, Severity, Detail, and Evidence field. The Root Cause section should go beyond the technical failure and explain the process gap that allowed it to happen.

---

## Reflection Prompt

After completing your incident report, write a short reflection (3–5 sentences) in your `reflections/week-06.md` file:

- Which part of the diagnostic process was most challenging?
- Was there a point in the investigation where you were tempted to jump to a conclusion before finishing the framework?
- What would you do differently if this were a real production incident?

---

## Connecting to Prior Weeks

This lab synthesizes all of Phase 1:

- **Week 2** — Trust chains: understanding why trust store gaps cause failures
- **Week 3** — X.509 anatomy: reading the certificate to confirm key replacement
- **Week 4** — Formats: OpenSSL commands as your primary diagnostic tools
- **Week 5** — Lifecycle: recognizing that certificate replacement creates revocation obligations
- **Week 6 Labs 01–03** — Each individual failure type you practiced is present in this scenario

This is what PKI operational work looks like. Real incidents are rarely a single clean failure. They involve several overlapping issues, and the engineer who resolves them fastest is the one who follows a framework and documents findings clearly.

---

## Submission

Save your completed write-up as `lab-04-full-diagnostic-stretch.md` in:

```
labs/week-06/submissions/full-diagnostic-stretch/
```

Log your submission at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app) under Week 6 — Lab 04.

---

*CVI PKI Career Pathway — Foundations Phase*
