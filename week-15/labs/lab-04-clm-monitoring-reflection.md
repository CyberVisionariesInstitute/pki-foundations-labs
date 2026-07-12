# Lab 04: Fleet Monitoring, Reflection & Export

**Student Name:**
**Date Completed:**
**Phase:** 2 | **Week:** 15
**Submission Path:** `labs/week-15/lab-04-clm-monitoring-reflection.md`

---

## Overview

This is the closing lab of Week 15's CLM arc. You will run a Health Check across your simulated CA fleet, document CRL freshness and OCSP status per CA, triage your alert feed, and complete the CLM Simulator's three reflection questions. Submitting the reflection marks your CLM Simulator complete and unlocks the Export Report button — **your exported compliance report is your actual submitted deliverable for this lab**, alongside this markdown file.

**Prerequisite:** Completion of Labs 02 and 03. The Monitoring tab reflects the same fleet you've already discovered, assessed, and remediated — it is not a separate dataset.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Access Path | Lab portal → `/select-track` → CLM Simulator |
| Tabs Used This Lab | Monitoring |
| CRL Thresholds | >48h PASS · 24–48h WARNING · <24h CRITICAL · Past NextUpdate EXPIRED |

---

## Part A — Fleet Health Dashboard

### Step 1 — Return to the CLM Simulator and Open the Monitoring Tab

Navigate to `/select-track` → **CLM Simulator** (or directly to `/select-track/clm-simulator`), the same tool and fleet from Labs 02 and 03. In the tab bar (Discovery / Policy / Automation / Monitoring), click **Monitoring**.

### Step 2 — Locate the Fleet Health Dashboard Sections

On the Monitoring tab, you should see two main areas: a **Fleet Health Dashboard** near the top (a summary line plus tables), and an **Alert Feed** below it (a list of individual alerts). Note their positions before proceeding.

### Step 3 — Read the Summary Strip First

Before running anything, look at the top of the Fleet Health Dashboard for a one-line summary (something like "X of Y CAs healthy, Z certificates need attention"). Note whether it's already showing from prior activity, or if this is your first visit to the tab.

```
(record what you see)
```

### Step 4 — Locate and Click "Run Health Check"

Find the button labeled **Run Health Check** (the main action button on this tab, in the same position pattern as "Run Discovery Scan" and "Run Automated Remediation" from the earlier tabs). Click it, and record the resulting summary strip exactly as shown.

Summary strip: `___________________`

### Step 5 — Document CRL Freshness Per CA

| CA Name | Time to NextUpdate | Status |
|---|---|---|
| | | |
| | | |
| | | |
| | | |

### Step 6 — Document OCSP Status Per CA

| CA Name | Reachable? | Accuracy (Valid/Revoked test) |
|---|---|---|
| | | |
| | | |
| | | |
| | | |

### Step 7 — Document the Expiration Pipeline

| Window | Certificate Count |
|---|---|
| Within 30 days | |
| Within 60 days | |
| Within 90 days | |

**Part A Summary:**

| Check | Result |
|---|---|
| Health Check run and summary strip recorded | Yes / No |
| CRL freshness documented for every CA | Yes / No |
| OCSP status documented for every CA | Yes / No |
| Expiration pipeline documented | Yes / No |

---

## Part B — Alert Feed Triage

### Step 1 — Record Your Alert Feed

List every alert exactly as it appears, including severity level.

| Severity | Alert Text |
|---|---|
| | |
| | |
| | |

### Step 2 — Rank Your Alerts by Actual Response Priority

Re-order the alerts above by what you would actually respond to first — not necessarily the order they appear. Explain your top 2 choices.

**#1 priority:** `___________________`
```
(why this one first)
```

**#2 priority:** `___________________`
```
(why this one second)
```

### Step 3 — Connect at Least One Alert to a Soft-Fail Risk

If any alert involves OCSP unreachability, explain what soft-fail behavior means for that specific CA right now (recall Week 12). If no such alert exists in your fleet, explain what the alert would say if it did, and why it would be CRITICAL.

```
(your answer here)
```

---

## Part C — Reflection, Submission & Export

### Step 1 — Locate the Reflection Section

Scroll down past the Alert Feed on the Monitoring tab. Once you've run both Automated Remediation (Lab 03) and Health Check (Part A of this lab) at least once, a **Reflection** section should appear below the alert feed with three text-entry questions. If you don't see it yet, confirm you've actually run Automated Remediation on the Automation tab — it won't appear until both actions have been completed at least once.

### Step 2 — Complete the Reflection Questions

Answer the CLM Simulator's three required reflection questions directly in the tool:

1. Which certificate posed the biggest risk to your fleet, and why?
2. Why couldn't the self-signed or weak-key certificates be auto-remediated?
3. If you were the on-call engineer and got paged about a CRL nearing its NextUpdate, what would you check first?

Paste your submitted answers here for your lab record:

```
1. (your answer)
2. (your answer)
3. (your answer)
```

### Step 3 — Submit and Confirm Completion

Once all three reflection fields are filled in, a **Submit & Mark Complete** button should become active/clickable near the bottom of the Reflection section (it may be disabled or grayed out until all three are non-empty). Click it.

**CLM Simulator shows "Complete":**
- [ ] Yes
- [ ] No — describe what happened instead:

### Step 4 — Locate and Click "Export Report"

After submitting, an **Export Report** button should appear (near the alert feed, or in the same area where you just submitted your reflection). Click it — this should trigger a file download in your browser, not open a new page.

Click **Export Report**. Confirm the downloaded file includes fleet size, violation counts by severity, zone breakdown, remediation actions taken, and your reflection answers.

**Exported report filename:** `___________________`

**Report contents confirmed complete:**
- [ ] Yes
- [ ] No — describe what's missing:

> **Submission requirement:** Attach your exported `clm-report-<date>.txt` file alongside this markdown file. This is your primary lab deliverable for Lab 04.

---

## Lab Report Questions

**1. Your #1 priority alert (Part B, Step 2) — walk through why it outranks the others, using the same severity-plus-time-to-impact reasoning from Lesson 4.**

```
(your answer here)
```

**2. Compare this Monitoring tab to Week 14's pkiview.msc and certutil health check. Name one thing this fleet-wide view can tell you that a single-CA pkiview session cannot, and one thing you'd still want certutil for even with this dashboard available.**

```
(your answer here)
```

**3. Your reflection answer #2 explained why self-signed or weak-key certificates couldn't be auto-remediated. Restate that reasoning here in the context of your fleet's specific Monitoring data — does anything in your CRL/OCSP/expiration results change how urgent that manual-review need is?**

```
(your answer here)
```

**4. You've now completed Discovery (Lab 02), Policy and Automation (Lab 03), and Monitoring (this lab) — the full CLM lifecycle. Pick one certificate from your fleet and trace it through all four stages: how it scored, what zone it landed in, whether it was auto-fixed or flagged for review, and what its current monitoring status is.**

```
(your answer here)
```

**5. Your exported compliance report is a snapshot of your fleet at one point in time. If your organization needed this exact report generated automatically every Monday morning instead of run manually, what would need to be different about the current CLM Simulator workflow to support that — and why is this the natural direction a real production CLM deployment moves in?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] Health Check run — summary strip recorded
- [ ] CRL freshness documented for every CA in the fleet
- [ ] OCSP reachability and accuracy documented for every CA
- [ ] Expiration pipeline (30/60/90-day) documented
- [ ] Full alert feed recorded and re-ranked by actual response priority
- [ ] Top 2 priority alerts explained
- [ ] Soft-fail risk connection explained for at least one alert
- [ ] All three reflection questions answered and submitted in the tool
- [ ] CLM Simulator shows "Complete"
- [ ] Compliance report exported and attached alongside this file
- [ ] All five lab report questions answered in complete sentences
- [ ] File committed to `labs/week-15/lab-04-clm-monitoring-reflection.md`
