# PKI Troubleshooting Runbook

## What is a Runbook?

In cybersecurity and IT operations, a **runbook** is a concise reference document that captures how to diagnose and resolve a specific issue. It is written so that anyone — including a future version of yourself — could follow it step by step without needing to rediscover the problem from scratch.

PKI professionals rely on runbooks because certificate and trust chain issues often surface under pressure: during a deployment, an outage, or an audit. A good runbook turns a hard-won troubleshooting experience into a repeatable, shareable process.

This project asks you to write a one-page runbook based on a real challenge you encountered during a Phase 1 lab.

---

## Where Your Source Material Comes From

Every lab submission template has a **Challenges / Troubleshooting** section. Over the course of Phase 1, you have been documenting errors, failed commands, verification issues, and how you resolved them.

Before writing your runbook, review those entries across all your weekly lab submissions (`labs/week-01/` through `labs/week-07/`). Choose **one scenario** that was meaningful — something you had to think through, diagnose, or look up. That scenario becomes your runbook.

---

## How to Write It

Your runbook should fit on one page. Use the following structure:

**1. Title**
Name the scenario clearly. Example: *Certificate Verification Failure Due to Expired Intermediate CA* or *OpenSSL Chain Validation Error on Self-Signed Root*.

**2. Problem Statement**
One to two sentences describing what went wrong. What did you expect to happen? What happened instead?

**3. Environment**
Note the relevant context — tool(s) used, certificate type, any configuration details that matter. This helps someone reading it know whether it applies to their situation.

**4. Symptoms**
List the specific signals that indicated something was wrong — error messages, unexpected command output, failed validation, etc. Be exact. Paste the actual error if you have it.

**5. Diagnostic Steps**
Walk through what you did to identify the root cause. Number each step. Include the commands you ran and what their output told you.

**6. Resolution**
Describe exactly what fixed it. Include the specific command, config change, or action taken.

**7. Prevention Note**
One or two sentences on how to avoid this issue in future — or what to check first if it happens again.

---

## What Makes a Good Runbook

- **Be specific.** "I got an error" is not useful. The exact error message, the exact command, and the exact fix are what make a runbook worth keeping.
- **Write it for someone else.** If a teammate had the same issue, could they follow your runbook and resolve it without asking you?
- **Keep it to one page.** A runbook is not a lab write-up. Every line should earn its place.

---

## Where to Submit

Commit your completed runbook to your GitHub portfolio repository:

```
runbooks/pki-troubleshooting-runbook.md
```

---

*CVI PKI Career Pathway — Phase 1 Foundations*
