# Lab 02 — Build Your PKI Toolkit (Stretch)

**Week 7 · PKI in Enterprise Environments & Your Career Path**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## What This Lab Is

This lab is different from every other lab in Phase 1.

You are not connecting to a failing endpoint. You are not running a diagnostic framework against a broken certificate. You are not writing an incident summary.

You are building a career artifact.

The output of this lab is a personal PKI reference document — a structured inventory of every tool you used across all seven weeks of Phase 1 labs, with a description of what each tool does, when to use it, and a real example command from your own lab history.

This document lives in your GitHub portfolio. It signals two things to any hiring manager who reviews your repository: that you completed seven weeks of hands-on work, and that you are the kind of engineer who documents what they know.

---

## Objectives

By the end of this lab, you will have:

- Documented every tool used across Phase 1 labs with descriptions, use cases, and real examples
- Produced a personal PKI toolkit reference committed to your GitHub portfolio
- Reflected on the full arc of Phase 1 technical work as preparation for the Week 8 reflection project

---

## Prerequisites

- Weeks 1–7 complete, or substantially complete
- Your Phase 1 lab submissions — you will pull real commands from your actual work

---

## What to Include

Your PKI Toolkit should cover every tool you used across Phase 1. Work through your lab submissions from Weeks 1–7 and identify every distinct tool, command, or service you used.

The minimum set for a complete PKI Toolkit includes:

### Core Tools (Required)

**openssl**
The foundational PKI command-line toolkit. You used it in every week from Week 3 onward.

Document each subcommand you used as a separate entry:
- `openssl x509`
- `openssl s_client`
- `openssl req`
- `openssl verify`
- `openssl ocsp`
- `openssl pkcs12`
- Any others from your lab history

**curl**
Used for TLS inspection and certificate header analysis in labs.

**Browser-based inspection**
You may have used browser certificate inspection in early weeks. Document the workflow.

### Reference and Analysis Services (Required)

**SSL Labs SSL Server Test** (ssllabs.com)
**crt.sh** (Certificate Transparency log search)

### Optional — Include If You Used Them

Any additional tools from your lab history: `nmap`, `wget`, specific platform CLIs (aws, az), cert-manager kubectl commands if you explored the stretch exercises.

---

## Format for Each Entry

Use the following format for every tool and subcommand:

```markdown
## [Tool Name] — [Brief Description]

**What it does:**
[One to two sentences describing what this tool does.]

**When to use it:**
[One to two sentences describing the circumstances where you would reach for this tool.]

**Example command from my labs:**
\`\`\`bash
[A real command from your Phase 1 lab submissions — not made up, copied from your actual work]
\`\`\`

**What the output tells you:**
[One sentence describing what to look for in the output of this command.]

**Phase 1 source:** Week [X], [Lab name]
```

---

## Example Entry

Here is an example entry to illustrate the format. Your entries should reflect your actual lab work — not this example.

---

### `openssl x509 -text -noout` — Parse a Certificate

**What it does:**
Reads an X.509 certificate file and prints all fields in human-readable form — validity dates, subject, issuer, subject alternative names, extensions, and signature algorithm.

**When to use it:**
Any time you need to read a certificate you have retrieved or been given. This is Step 2 of the PKI Diagnostic Framework — parse the certificate after retrieving it from the live system.

**Example command from my labs:**
```bash
openssl x509 -in expired_cert.pem -text -noout
```

**What the output tells you:**
Whether the certificate is within its validity period, whether the SAN matches the hostname being accessed, and who issued it — which determines whether the chain path goes through a public CA or an internal CA.

**Phase 1 source:** Week 6, Lab 01 — Expired Certificate Diagnosis

---

## Document Structure

Your PKI Toolkit document should follow this structure:

```markdown
# My PKI Toolkit
**CVI PKI Career Pathway — Phase 1 Foundations**
Completed: [Month Year]

---

## About This Document

[2–3 sentences describing what this document is and why you built it.]

---

## Core Command-Line Tools

### openssl x509
[entry]

### openssl s_client
[entry]

### openssl req
[entry]

### openssl verify
[entry]

### openssl ocsp
[entry]

### openssl pkcs12
[entry]

[any additional openssl subcommands you used]

---

## Network and Inspection Tools

### curl
[entry]

[any others]

---

## Reference and Analysis Services

### SSL Labs SSL Server Test
[entry]

### crt.sh — Certificate Transparency Search
[entry]

---

## Phase 1 Skills Summary

[A brief paragraph — 3–5 sentences — summarizing what Phase 1 built as a whole and how these tools connect to real PKI operational work. Written in your own words.]

```

---

## Submission

Save your completed toolkit as `lab-02-pki-toolkit.md` in:

```
labs/week-07/submissions/pki-toolkit/
```

Commit with a meaningful message. This is a portfolio artifact — your commit message should reflect that.

**Suggested commit message:**
```
Week 7 Lab 02 — PKI Toolkit: Phase 1 tools reference and skills summary
```

Log your submission at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app) under Week 7 — Lab 02 (Stretch).

---

## What Makes a Strong PKI Toolkit

**Real examples.** The example commands should come from your actual lab submissions — not reconstructed from memory or copied from documentation. Pull them from your lab write-ups. The specificity of real examples from real work is what makes this document valuable.

**Honest descriptions.** The "when to use it" section should reflect when you actually reached for this tool, not when the documentation says to use it. If you used `openssl s_client` every time you needed to diagnose a TLS failure, say that.

**Your own language.** Do not copy definitions from documentation. Write descriptions in the way you would explain this tool to someone who is starting Phase 1 where you started. Clear, direct, honest.

**Complete coverage.** A PKI Toolkit that only covers `openssl x509` is incomplete. Go through every lab from every week. Every tool you used should appear here.

---

## Connecting to Week 8

The Week 8 Phase 1 Reflection Project asks you to document what you built and how your thinking changed. The PKI Toolkit is a concrete artifact that supports the reflection — it shows the breadth of what you learned in a form that does not require self-assessment.

When you write your reflection in Week 8, you can reference your toolkit as evidence. "I built a documented reference of every tool I used across Phase 1" is a different kind of claim than "I learned OpenSSL." The document makes it true.

---

*CVI PKI Career Pathway — Foundations Phase*
