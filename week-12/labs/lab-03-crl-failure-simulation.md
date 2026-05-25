# Lab 03: Simulate a CRL Publication Failure *(Stretch)*

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 12  
**Submission Path:** `labs/week-12/lab-03-crl-failure-simulation.md`

---

> **Stretch Lab:** Lab 03 is optional but strongly recommended. It is the highest-value evidence artifact of Week 12. The incident report format you produce here — failure, detection, resolution — is the same format used for real PKI infrastructure incidents throughout your career.

---

## Prerequisites

Complete Labs 01 and 02 before starting Lab 03.

- Lab 01 complete: a revoked certificate exists in the CA
- Lab 02 complete: the Online Responder is configured and operational

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Proceed to Part A.

## Record the current CRL publication settings BEFORE making any changes
certutil -getreg CA\CRLPeriod
certutil -getreg CA\CRLPeriodUnits
certutil -getreg CA\CRLDeltaPeriod
certutil -getreg CA\CRLDeltaPeriodUnits
```

**Current CRL publication settings (RECORD THESE — you will need them to restore):**

| Setting | Current Value |
|---|---|
| CRLPeriod | |
| CRLPeriodUnits | |
| CRLDeltaPeriod | |
| CRLDeltaPeriodUnits | |

**Current CRL NextUpdate (record before you create the failure):**

```powershell
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\<CA-Name>.crl" | findstr "NextUpdate"
```

```
NextUpdate before failure:
```

---

## Part A — Create the CRL Publication Failure

You will stop the CRL publication schedule by setting the publication interval to an extremely long value. This simulates what happens during a maintenance window when the CA service is restarted without the publication schedule resuming, or when the CRL period is misconfigured.

> **Note:** Do not forcibly delete the CRL file. This simulation works by making the publication interval so long that the existing CRL will expire before a new one is automatically published.

**Step 1:** Extend the CRL publication period to simulate a missed publication.

```powershell
# Extend CRL period to a very long interval
certutil -setreg CA\CRLPeriodUnits 99
certutil -setreg CA\CRLPeriod Years

# Restart the CA service to apply the registry change
net stop CertSvc
net start CertSvc
```

```
(paste output of the net stop / net start commands)
```

**Step 2:** Verify the new CRL period is set.

```powershell
certutil -getreg CA\CRLPeriod
certutil -getreg CA\CRLPeriodUnits
```

```
(paste output confirming the extended period)
```

**Step 3:** Record the current CRL NextUpdate again — this is when the existing CRL will expire.

```powershell
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\<CA-Name>.crl" | findstr "NextUpdate\|ThisUpdate"
```

```
NextUpdate (stale CRL expires at):
ThisUpdate (CRL was last published at):
```

> **Lab Note:** You may need to wait until the NextUpdate time has passed to observe the full failure mode, or advance the system clock temporarily for simulation. Check with your instructor. Alternatively, document the expected failure behavior based on what certutil reports for an expired CRL, even if you observe the transition.

---

## Part B — Observe the Failure

**Step 1:** Attempt to verify a certificate while the CRL is stale or expired.

```powershell
# Attempt certificate verification — observe revocation check behavior
certutil -verify <valid-certificate.cer>
```

```
(paste output — does it succeed, fail, or warn about CRL status?)
```

**Step 2:** Test the CDP URL and OCSP URL.

```powershell
certutil -URL <valid-certificate.cer>
```

```
(paste output — what does the URL Retrieval Tool show for CRL and OCSP status?)
```

**Step 3:** Check the OCSP responder behavior.

After the CRL becomes stale, the Online Responder should return Unknown status for OCSP queries (because it reads the CRL for its data).

```powershell
# Test OCSP query — observe whether it returns Good, Revoked, or Unknown
certutil -URL <valid-certificate.cer>
```

**OCSP response after CRL becomes stale:**
- [ ] Good (OCSP responder has not yet detected the stale CRL)
- [ ] Unknown (OCSP responder has detected the stale CRL)
- [ ] Connection error

```
(describe what you observe — if the OCSP still returns Good, note the delay between CRL expiry and OCSP awareness)
```

**Step 4:** Check Event Viewer for relevant events.

```powershell
# Check for CRL publication events
Get-EventLog -LogName Application -Source "CertSvc" -Newest 30 | 
  Where-Object {$_.EventID -in 46,47,4870} | 
  Format-List TimeGenerated, EventID, Message
```

```
(paste relevant Event Viewer output — document Event IDs and timestamps)
```

**Event IDs observed:**

| Event ID | Timestamp | Description |
|---|---|---|
| | | |
| | | |

---

## Part C — Diagnose the Failure

Document your diagnostic process as if you were responding to an incident — not as if you already know the cause.

**What symptom would alert you to this failure in a production environment?**

```
(e.g., monitoring alert, user complaint, proactive check — describe what would trigger your investigation)
```

**Diagnostic steps you took (in order):**

1. 
2. 
3. 
4. 

**Root cause identified:**

```
(state the specific root cause: what failed and why)
```

**Is this environment hard-fail or soft-fail for CRL checking?**
- [ ] Hard-fail (certificate verification failed with an error)
- [ ] Soft-fail (certificate verification succeeded despite stale CRL)
- [ ] Unclear — describe what you observed:

```
(explain how you determined the fail behavior)
```

---

## Part D — Restore Normal Operation

**Step 1:** Restore the original CRL publication settings.

```powershell
# Restore to the values you recorded in the Pre-Lab section
certutil -setreg CA\CRLPeriodUnits <original-value>
certutil -setreg CA\CRLPeriod <original-value>

# Restart the CA service
net stop CertSvc
net start CertSvc
```

```
(paste output)
```

**Step 2:** Force immediate CRL publication.

```powershell
certutil -CRL
```

```
(paste output)
```

**Step 3:** Verify the new CRL is accessible and not expired.

```powershell
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\<CA-Name>.crl" | findstr "ThisUpdate\|NextUpdate"
```

```
NewThisUpdate:
New NextUpdate:
```

**Is the new NextUpdate in the future?**
- [ ] Yes
- [ ] No — describe:

**Step 4:** Confirm certificate verification succeeds after restoration.

```powershell
certutil -verify <valid-certificate.cer>
```

```
(paste output — confirm verification succeeds)
```

**Step 5:** Confirm OCSP responder returns correct status after CRL is restored.

```powershell
certutil -URL <valid-certificate.cer>
```

```
(paste output — confirm OCSP returns Good for valid cert, Revoked for Lab 01 cert)
```

---

## Part E — Structured Incident Report

Write your incident report using the standard format: **Failure → Detection → Resolution**.

This is the primary deliverable for Lab 03. Write it as you would for a real PKI operations incident — clearly, in past tense, with timestamps where available.

---

### Incident Report: CRL Publication Failure Simulation

**Incident Date:**  
**Environment:** corp.cvilab.local  
**Incident Type:** CRL publication schedule failure — simulated  

---

#### Section 1: Failure

*Describe what happened, what caused it, and when the failure began.*

```
(write your description here — be specific about the technical root cause and the timeline)
```

**Supporting evidence — Event Log entries:**

| Event ID | Source | Timestamp | Description |
|---|---|---|---|
| | | | |

**Supporting evidence — CRL inspection (NextUpdate before restoration):**

```
(paste the certutil output showing the stale CRL)
```

---

#### Section 2: Detection

*Describe how the failure was identified — what symptom or check revealed it.*

```
(write your description here — was it proactive or reactive? what command or observation confirmed it?)
```

**Diagnostic commands used:**

```powershell
(list the commands you ran to confirm the failure — in order)
```

**Failure behavior observed (hard-fail or soft-fail, and what that looked like):**

```
(describe what you saw — whether verification failed with an error or succeeded silently)
```

---

#### Section 3: Resolution

*Describe the steps taken to restore normal operation, in order.*

**Resolution steps (in order, with commands):**

```
1. (first step taken)

2. (second step taken)

3. (third step taken)

(add more as needed)
```

**Verification that resolution was successful:**

```
(paste certutil -verify output after restoration, confirming verification succeeds)
```

**Time from failure identification to restored CRL:**

```
(estimate based on your lab session)
```

---

## Part F — Reflection Questions

**1. In a production environment, how would you know the CRL had become stale before a relying party noticed? What monitoring or alerting would you put in place?**

```
(your answer here)
```

**2. You observed whether your environment was hard-fail or soft-fail. What is the security implication of soft-fail behavior specifically for a key compromise scenario — where the attacker is using a revoked certificate that is not being rejected?**

```
(your answer here)
```

**3. The OCSP responder's status changed when the CRL became stale. Why? What does this tell you about the dependency between CRL publication and OCSP responder accuracy?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] Pre-lab settings recorded (original CRL period values)
- [ ] CRL publication failure created (registry change + service restart)
- [ ] Failure observed and documented (certutil -verify and -URL output)
- [ ] Event log evidence collected (Event IDs and timestamps)
- [ ] Root cause identified (specific technical description)
- [ ] Hard-fail vs. soft-fail behavior documented
- [ ] Normal operation restored (registry values, CRL published, verification succeeds)
- [ ] Post-restoration OCSP status confirmed
- [ ] Structured incident report completed (Failure / Detection / Resolution)
- [ ] All three reflection questions answered
- [ ] Lab report committed to `labs/week-12/lab-03-crl-failure-simulation.md`
