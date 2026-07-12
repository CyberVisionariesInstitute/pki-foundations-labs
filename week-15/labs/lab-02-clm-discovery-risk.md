# Lab 02: Certificate Discovery & Risk Scoring

**Student Name:**
**Date Completed:**
**Phase:** 2 | **Week:** 15
**Submission Path:** `labs/week-15/lab-02-clm-discovery-risk.md`

---

## Overview

In this lab, you run your first Discovery scan in the CLM Simulator — a browser-based tool in your lab portal, no VM required. Your simulator generates a personalized certificate fleet just for you, seeded from a larger pool, so your findings will be specific to your own account, not a copy of a classmate's.

You will run a Discovery Scan, read the computed risk scores, and document your fleet's highest-risk certificates along with the specific violations driving each score. This lab is the foundation for Lab 03 (Policy Zones and Automation) and Lab 04 (Monitoring) — all three use this same fleet.

**Prerequisite:** Your account must have CLM Simulator access (`clm_access`). If you can't reach the CLM Simulator card from `/select-track`, contact your instructor before starting this lab.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Access Path | Lab portal → `/select-track` → CLM Simulator card |
| Tool | CLM Simulator (browser-based, no VM) |
| Fleet | 15 certificates, personalized per student, seeded from a 50-certificate pool |
| Tabs Used This Lab | Discovery |

---

## Pre-Lab Check — Orientation: Finding and Opening the CLM Simulator

This is your first lab using the CLM Simulator, so slow down and follow each navigation step exactly before you start clicking around.

### Step 1 — Sign In to the Lab Portal

Go to the lab portal and sign in with your student account, the same way you do for VM access. If you land on your `/dashboard` (VM Buildout view) instead of a track-selection screen, that's expected — see Step 2.

### Step 2 — Navigate to Track Selection

In your browser's address bar, go to `/select-track` (or use the "Select Track" / "Switch Track" link if your dashboard shows one). You should see two large option cards: **VM Buildout** and **CLM Simulator**.

**Track selection screen loaded with both cards visible:**
- [ ] Yes — proceed to Step 3
- [ ] No — describe what you see instead, and check with your instructor before continuing:

### Step 3 — Confirm CLM Simulator Access and Open It

Look at the **CLM Simulator** card. It should be clickable, not showing a locked or "access pending" state. Click into it (the card itself, or its "Continue"-style button if present).

**CLM Simulator accessible and opened:**
- [ ] Yes — proceed to Step 4
- [ ] No — it's locked or shows a pending-access message — contact your instructor before continuing

### Step 4 — Orient Yourself to the Interface

You should now be on the CLM Simulator page (URL will be something like `/select-track/clm-simulator`). Before touching anything, locate these elements on the page:

- A page header identifying this as the CLM Simulator, with a short intro line
- A row of **four tabs**: **Discovery**, **Policy**, **Automation**, **Monitoring** — Discovery should already be selected/highlighted, since it's first
- Below the tabs, the main content area for whichever tab is currently selected

```
(confirm all four tabs are visible and Discovery is the active/highlighted one)
```

---

## Part A — Run Your First Discovery Scan

### Step 1 — Confirm You're on the Discovery Tab

If Discovery isn't already the highlighted tab, click it now. This is where you'll spend all of Part A and Part B.

### Step 2 — Locate and Click "Run Discovery Scan"

Near the top of the Discovery tab's content area, find the button labeled **Run Discovery Scan** (it may be styled as a prominent colored button — this is the main action on this tab). Click it.

```
(describe what happens immediately after clicking — does a table appear, is there a brief loading state, etc.)
```

### Step 3 — Read the Resulting Table

Once the scan completes, a table of certificates should populate the page. Each row is one certificate. Look across the columns before recording anything — you're getting oriented, not filling in data yet.

```
(describe what happens — how many certificates appear, and what columns are shown)
```

### Step 4 — Record Your Fleet Size and Column Structure

Total certificates discovered: `___________________`

Columns shown (e.g., subject/hostname, issuer, expiration, key algorithm/size, owner, risk score): 

```
(list the columns exactly as they appear)
```

### Step 5 — Locate the Risk Score Column and Confirm the Sort Order

Find the column showing each certificate's risk score — it should display as a colored badge (red/amber/yellow/green) rather than a plain number alone.

**The Discovery table is sorted by risk score, descending, by default (highest-risk certificate in row 1):**
- [ ] Yes — confirmed
- [ ] No — describe what you observed instead:

---

## Part B — Document Your Highest-Risk Certificates

### Step 1 — Identify Your Top 5 Highest-Risk Certificates

For each, record the exact risk score and every violation contributing to it (a certificate can have more than one).

| Rank | Subject / Hostname | Issuer | Risk Score | Badge | Violations Contributing |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

### Step 2 — Verify the Score Math on Your #1 Certificate

Using the risk score formula from Lesson 1 (expiring ≤7 days = 40, expiring 8–30 days = 20, weak key = 35, untrusted/self-signed = 35, missing owner = 10, capped at 100), show your work confirming your #1 certificate's score:

```
(show the calculation — e.g., "expiring in 5 days (+40) + missing owner (+10) = 50, High")
```

**Your calculation matches the score shown in the tool:**
- [ ] Yes
- [ ] No — describe the discrepancy:

### Step 3 — Count Certificates by Risk Band

| Band | Score Range | Count in Your Fleet |
|---|---|---|
| Critical | 70–100 | |
| High | 40–69 | |
| Medium | 15–39 | |
| Low | 0–14 | |

---

## Part C — Analysis

### Step 1 — Identify Stacking Violations

Find at least one certificate in your fleet with more than one violation contributing to its score. Explain how the individual violations combine.

```
(your answer here)
```

### Step 2 — Preview: What Zone Does Each Top-5 Certificate Belong To?

Based on the naming patterns you learned about in Lesson 1 (www/portal/api = Web Servers, svc- = Service Accounts, dc01/pki-srv01-style = Domain Infrastructure, vpn = Network/Remote Access, self-signed/no owner = Unmanaged/Legacy), guess which zone each of your top 5 certificates likely belongs to. You'll confirm this in Lab 03.

| Rank | Certificate | Predicted Zone |
|---|---|---|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

---

## Lab Report Questions

**1. Your #1 highest-risk certificate — walk through its exact risk score calculation and explain, in plain language, why it needs attention before your other four top-5 certificates.**

```
(your answer here)
```

**2. Compare your fleet's risk band counts (Part B, Step 3) to a classmate's, if you have access to that information, or reason about it hypothetically. Why do you expect these counts to differ between students even though everyone is using the same simulator?**

```
(your answer here)
```

**3. Autoenrollment, which you configured by hand in Lab 01, has no concept of a risk score at all. Explain specifically what autoenrollment would have to do differently to produce something like this Discovery view — and why that's outside its actual job.**

```
(your answer here)
```

**4. Pick one certificate in your fleet with a missing owner tag. Explain, using Lesson 1's reasoning, why this violation only adds 10 points to the risk score even though Lesson 2 describes missing ownership as one of the most operationally dangerous, quietest problems in a certificate fleet. Is the point weighting inconsistent with that framing, or does it make sense — explain your reasoning.**

```
(your answer here)
```

**5. If you were designing this Discovery tool for a real organization, what is one additional column or data point you would want to see that isn't currently shown, and why would it help with prioritization?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] CLM Simulator access confirmed
- [ ] Discovery Scan run — fleet size and column structure recorded
- [ ] Default sort order (risk score, descending) confirmed
- [ ] Top 5 highest-risk certificates documented with exact scores and violations
- [ ] Risk score calculation shown and verified for your #1 certificate
- [ ] Risk band counts completed for the full fleet
- [ ] Stacking-violation example identified and explained
- [ ] Zone predictions recorded for all top 5 certificates
- [ ] All five lab report questions answered in complete sentences
- [ ] File committed to `labs/week-15/lab-02-clm-discovery-risk.md`
