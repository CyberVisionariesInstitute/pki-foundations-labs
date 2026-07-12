# Lab 03: Policy Zones, Violations & Automated Remediation

**Student Name:**
**Date Completed:**
**Phase:** 2 | **Week:** 15
**Submission Path:** `labs/week-15/lab-03-clm-policy-automation.md`

---

## Overview

This lab has two connected parts, using the same fleet you discovered in Lab 02. In **Part A**, you explore the Policy tab — reading your fleet's zone-by-zone pass and fail rates and explaining specific violation types in your own words. In **Part B**, you run Automated Remediation and document exactly which certificates got auto-fixed, which need manual review, and why — grounded in the trust-decision boundary from Lesson 3.

This is one continuous flow, not two separate exercises: the violations you document in Part A are exactly what Part B acts on.

**Prerequisite:** Completion of Lab 02 (Discovery). Your fleet does not change between labs — you're working with the same certificates throughout the week.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Access Path | Lab portal → `/select-track` → CLM Simulator |
| Tabs Used This Lab | Policy, Automation |
| Zones | Web Servers, Service Accounts, Domain Infrastructure, Network/Remote Access, Unmanaged/Legacy, Other |

---

## Part A — Policy Zones & Violations

### Step 1 — Return to the CLM Simulator

Sign in to the lab portal and navigate to `/select-track` → **CLM Simulator**, the same way you did in Lab 02 (or go directly to `/select-track/clm-simulator` if you've bookmarked it). You'll land back on the same tool with the same fleet you discovered in Lab 02 — nothing resets between labs.

### Step 2 — Open the Policy Tab

In the tab bar at the top of the page (Discovery / Policy / Automation / Monitoring), click **Policy**. The content area below the tabs will switch to the Policy view.

### Step 3 — Locate the Zone Groupings

On the Policy tab, your fleet should appear grouped into sections or cards, one per zone — look for section headers or labels reading **Web Servers**, **Service Accounts**, **Domain Infrastructure**, **Network/Remote Access**, **Unmanaged/Legacy**, and **Other**. Each zone section should show a pass/fail count or ratio near its header.

```
(confirm all six zone groupings are visible, even if some are empty for your fleet)
```

### Step 4 — Document Zone Pass/Fail Rates

| Zone | Certificates in Zone | Pass | Fail | Pass Rate |
|---|---|---|---|---|
| Web Servers | | | | |
| Service Accounts | | | | |
| Domain Infrastructure | | | | |
| Network/Remote Access | | | | |
| Unmanaged/Legacy | | | | |
| Other | | | | |

### Step 5 — Identify Your Lowest-Passing Zone

Lowest-passing zone: `___________________`

**Failures in this zone cluster around one violation type, or spread across several?**
- [ ] Cluster around one type — which one:
- [ ] Spread across several types — list them:

### Step 6 — Explain Two Violation Types in Your Own Words

Using two violation types present in your fleet, write your own explanation (not copied from the resource pack hint text) of why each matters operationally — using a specific certificate from your fleet as the example for each.

**Violation type 1:** `___________________`
```
(your explanation, referencing a specific certificate)
```

**Violation type 2:** `___________________`
```
(your explanation, referencing a specific certificate)
```

**Part A Summary:**

| Check | Result |
|---|---|
| All six zones reviewed | Yes / No |
| Zone pass/fail rates documented | Yes / No |
| Lowest-passing zone identified with failure pattern | Yes / No |
| Two violation types explained in own words | Yes / No |

---

## Part B — Automated Remediation

### Step 1 — Open the Automation Tab

In the same tab bar (Discovery / Policy / Automation / Monitoring), click **Automation**. This switches you away from the Policy view into the Automation view, still working against your same fleet.

### Step 2 — Predict Before You Run

Before clicking Run Automated Remediation, review your Part A violations and predict which certificates you expect to be auto-fixed versus which will need manual review, based on the trust-decision boundary (trusted-CA reissuance and re-tagging are automatable; self-signed/untrusted issuers and weak keys are not).

```
(list your predictions here, certificate by certificate)
```

### Step 3 — Locate and Click "Run Automated Remediation"

On the Automation tab, find the button labeled **Run Automated Remediation** (the main action button on this tab, similar in placement to Discovery's scan button). Click it, and wait for the results to populate — you should see certificates split into two groups or lists.

Certificates auto-fixed: `___________________` (count)
Certificates needing manual review: `___________________` (count)

### Step 4 — Compare Predictions to Actual Results

**Your predictions matched the actual split:**
- [ ] Fully
- [ ] Partially — describe any mismatches and why they occurred:
- [ ] Not at all — describe what you missed:

### Step 5 — Document the Manual-Review Reasoning

For each certificate needing manual review, name the *specific trust decision* involved — not just "needs review."

| Certificate | Violation(s) | Specific Trust Decision Required |
|---|---|---|
| | | |
| | | |
| | | |

### Step 6 — Check Ownership on Manual-Review Items

For each manual-review certificate, note whether it has an owner tag. If it does, that's who a real workflow would route the ticket to. If it doesn't, note that as an additional gap.

```
(list manual-review certificates and their ownership status)
```

**Part B Summary:**

| Check | Result |
|---|---|
| Predictions recorded before running remediation | Yes / No |
| Automated Remediation run — counts recorded | Yes / No |
| Manual-review trust decisions documented per certificate | Yes / No |
| Ownership status checked for manual-review certificates | Yes / No |

---

## Lab Report Questions

**1. Compare your lowest-passing zone (Part A) to your highest-risk-score certificates from Lab 02. Do they point to the same certificates, or different ones? What does this tell you about the difference between "risk score" and "zone blast radius" as prioritization tools?**

```
(your answer here)
```

**2. For one of your manual-review certificates, explain specifically why automating its fix would require a "trust decision" a machine can't safely make — reference the specific violation and what a human would actually have to weigh.**

```
(your answer here)
```

**3. If your fleet had a certificate that was both self-signed AND missing an owner tag, walk through what happens to it in this lab: how would it score in Discovery (Lab 02), which zone would it land in (Part A), and how would Automation (Part B) handle it? Trace it through all three.**

```
(your answer here)
```

**4. Your Automated Remediation results showed a specific split between auto-fixed and manual-review certificates. If your organization's policy required 100% automation with no manual-review category at all, what specifically would have to change about how trust decisions get made — and why is that a bad idea, based on Lesson 3's reasoning?**

```
(your answer here)
```

**5. Now that you've seen both policy zones and automated remediation acting on the same fleet, explain in two to three sentences how these two tabs work together — what would be missing if a CLM platform only had one of them, not both?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] All six policy zones reviewed and pass/fail rates documented
- [ ] Lowest-passing zone identified with its failure pattern
- [ ] Two violation types explained in your own words, referencing specific certificates
- [ ] Predictions recorded before running Automated Remediation
- [ ] Automated Remediation run — auto-fixed and manual-review counts recorded
- [ ] Predictions compared to actual results, mismatches explained
- [ ] Specific trust decision documented for every manual-review certificate
- [ ] Ownership status checked for all manual-review certificates
- [ ] All five lab report questions answered in complete sentences
- [ ] File committed to `labs/week-15/lab-03-clm-policy-automation.md`
