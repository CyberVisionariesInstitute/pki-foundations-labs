# Week 15 — Certificate Lifecycle Management at Scale

## Focus

Every week so far has centered on one CA, one certificate, one operation at a time — issue it, revoke it, back it up, check its health. That model works when you're operating a lab environment with a handful of certificates. It breaks down the moment you're responsible for a real enterprise fleet: hundreds or thousands of certificates, issued by multiple CAs and cloud services, held by servers, service accounts, and devices that nobody has manually tracked in months. Week 15 is about that scale problem, and about the class of tooling — certificate lifecycle management, or CLM — that real PKI operations teams build their day-to-day work around.

Lesson 1 opens with the mechanism you already know: autoenrollment, configured through Group Policy, silently issuing and renewing certificates to domain-joined machines without a help desk ticket ever being filed. That's genuinely powerful, and it's also exactly why fleets get out of control — autoenrollment issues certificates reliably, but it doesn't tell you what exists, what's about to expire, what's misconfigured, or what's dangerously weak. CLM doesn't replace autoenrollment; it sits on top of it, giving you the visibility and governance layer that pure issuance never provided. Lesson 1 makes the case for why CLM has become close to universal in real enterprise environments, even where AD CS autoenrollment is still doing the actual issuance underneath.

Lesson 2 introduces the first CLM discipline: discovery and risk scoring. A certificate fleet you can't see is a fleet you can't secure, and Lesson 2 walks through how a CLM platform builds a live inventory and scores each certificate's risk — expiring within 7 days, expiring within 30, using a weak key under 2048 bits, chaining to an untrusted or self-signed issuer, missing an assigned owner. Each factor adds points toward a capped 0–100 score, and that score sorts into bands — Critical, High, Medium, Low — that tell an operations team where to look first instead of reviewing thousands of certificates in arbitrary order.

Lesson 3 covers policy and automation — the two disciplines that turn visibility into action. Policy zones (Web Servers, Service Accounts, Domain Infrastructure, Network/Remote Access, Unmanaged/Legacy, and a catch-all Other) group certificates by the context they operate in and enforce zone-specific rules, so a rule that makes sense for a public web server doesn't get blindly applied to a domain controller. Automation is where the lesson draws its sharpest line: reissuing a certificate from a CA you already trust, or re-tagging a certificate's owner, can be automated safely because no new trust decision is being made. Reissuing something self-signed, or something using a weak key, cannot — a human has to decide whether that issuer or that key strength is acceptable, and no risk score, however high, changes that.

Lesson 4 closes the week with monitoring at fleet scale: the same CRL freshness thresholds and OCSP soft-fail risk from Weeks 12 and 14, now read across an entire fleet rather than one CA, plus a 30/60/90-day expiration pipeline and a severity-ordered alert feed that surfaces what needs attention first. It also raises the operational reality of soft-fail OCSP at scale — when Windows' default behavior means an unreachable OCSP responder gets treated as a valid certificate, silently, that's a very different risk when it's happening across a thousand certificates instead of one.

Four labs put this into practice. Lab 01 is the one hands-on autoenrollment lab this week — configuring GPO-based enrollment end to end, because CLM assumes that mechanism exists underneath it, and no operations engineer should be untouched by how it actually works. Labs 02 through 04 are built entirely around a real CLM Simulator inside your lab portal — a live, browser-based tool with a personalized 15-certificate fleet seeded just for you, covering discovery and risk scoring, policy zones and automation, and fleet monitoring with a required reflection and an exportable compliance report as your final deliverable.

Week 16 is the mini capstone — an end-to-end PKI architecture and operations scenario that will expect you to reason with both the autoenrollment mechanism from Lab 01 and the CLM discipline from Labs 02–04, not just one or the other.

---

## Outcomes

By the end of this week, you can:

- Configure certificate autoenrollment through Group Policy, from template permissions through GPO linking and client-side verification
- Explain why CLM platforms have become standard in real enterprise environments even where AD CS autoenrollment still performs the underlying issuance
- Navigate a CLM platform's Discovery view and interpret a live certificate inventory
- Calculate certificate risk scores from individual risk factors (expiration window, key strength, issuer trust, ownership) and classify them into Critical/High/Medium/Low bands
- Explain how policy zones group certificates by operational context and why zone-specific rules matter
- Distinguish which CLM remediation actions are safe to automate (trusted-CA reissuance, re-tagging) from which require human trust judgment (untrusted/self-signed issuers, weak keys) regardless of risk score
- Read a fleet-wide health dashboard for CRL freshness and OCSP reachability at scale, using the same PASS/WARNING/CRITICAL/EXPIRED thresholds established in Weeks 12 and 14
- Explain the operational risk of soft-fail OCSP behavior across a large certificate fleet
- Triage a severity-ordered alert feed and prioritize remediation using both risk score and business context
- Produce and interpret an exportable fleet compliance report

---

## The Phase 2 Environment

Lab 01 runs on PKI-SRV01 and CLIENT01 in the corp.cvilab.local domain. Labs 02–04 run entirely in the browser-based CLM Simulator inside the student lab portal — no VM or Bastion access required.

| Component | Detail | Notes |
|---|---|---|
| Domain | corp.cvilab.local | Windows Active Directory domain |
| DC01 | Domain Controller — runs AD DS and DNS | Start first for Lab 01 |
| PKI-SRV01 (192.168.10.20) | Enterprise Issuing CA — CVI Issuing CA 1 | Lab 01 only. Log in as CORP\pki.admin. |
| CLIENT01 | Domain-joined workstation, CVI Workstations OU | Lab 01 — used to verify autoenrollment via `gpupdate`, `gpresult`, and the Certificates MMC |
| CVI Lab Portal | Browser-based — no VM required | Labs 02–04. Sign in with your student account. |
| CLM Simulator | Lab portal → `/select-track` → CLM Simulator card (requires `clm_access`) | Your personalized 15-certificate fleet is seeded automatically the first time you open it — it will look the same every time you return. |

> **First time in the CLM Simulator:** this is a brand-new tool for you this week. Lab 02 walks through exactly how to sign in and find it — you do not need to have used it before Lab 02.

> **Prerequisite:** Lab 01 requires a published certificate template from Week 10. Labs 02–04 require CLM Simulator access — confirm with your instructor before Lesson 1 if you're unsure whether your account has it enabled.

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Autoenrollment via Group Policy | `labs/week-15/lab-01-autoenrollment-gpo.md` |
| Lab 02 — CLM Discovery and Risk Scoring | `labs/week-15/lab-02-clm-discovery-risk.md` |
| Lab 03 — CLM Policy Zones and Automation | `labs/week-15/lab-03-clm-policy-automation.md` |
| Lab 04 — CLM Monitoring and Reflection | `labs/week-15/lab-04-clm-monitoring-reflection.md` |

> **Lab sequencing is a hard dependency for Labs 02–04:** each builds directly on the state and findings of the one before it — the highest-risk certificates you identify in Lab 02 are the ones you'll zone and automate in Lab 03, and the fleet health picture in Lab 04 assumes Automation has already run. Complete them in order. Lab 01 is independent and can be completed any time. Labs 02–04 all operate on the same per-student, seeded fleet — reloading or re-signing in does not regenerate a new one.

---

## Checklist

**Pre-lab (all labs):**
- [ ] Lab 01 only: confirm DC01 and PKI-SRV01 are both running and CertSvc responds to `certutil -ping`
- [ ] Labs 02–04: confirm you can sign in to the CVI Lab Portal and locate the CLM Simulator card under Select Track

---

**Lab 01 — Autoenrollment via Group Policy:**
- [ ] Confirm the target certificate template exists (from Week 10) and note its name
- [ ] Grant Autoenroll, Enroll, and Read permissions to the CVI Workstations OU (or the appropriate security group)
- [ ] Create a new GPO for certificate autoenrollment
- [ ] Configure the Certificate Services Client — Auto-Enrollment policy inside the GPO
- [ ] Link the GPO to the CVI Workstations OU
- [ ] On CLIENT01, run `gpupdate /force` and confirm no errors
- [ ] Run `gpresult /r` and confirm the new GPO is listed as applied
- [ ] Open the Certificates MMC and confirm the autoenrolled certificate is present
- [ ] Run `certutil -viewstore` (or equivalent) to confirm the certificate from the client side
- [ ] On PKI-SRV01, run `certutil -view` to confirm the corresponding issuance record
- [ ] Document the reusable GPO autoenrollment procedure
- [ ] Answer all five lab report questions, including why CLM still needs autoenrollment underneath it
- [ ] Commit `lab-01-autoenrollment-gpo.md` to portfolio repo

---

**Lab 02 — CLM Discovery and Risk Scoring:**
- [ ] Sign in to the CVI Lab Portal and navigate to Select Track → CLM Simulator
- [ ] Identify all four tabs (Discovery, Policy, Automation, Monitoring) before starting any task steps
- [ ] Run the Discovery Scan and let your personalized 15-certificate fleet populate
- [ ] Read the resulting inventory table and record what fields are shown for each certificate
- [ ] Identify your top 5 highest-risk certificates by score
- [ ] Manually recalculate the risk score for at least two certificates and verify it against the tool's displayed score
- [ ] Record how many certificates fall into each risk band (Critical/High/Medium/Low)
- [ ] Identify at least one certificate with two or more stacked risk factors and explain how they combine
- [ ] Predict which policy zone each of your top 5 certificates likely belongs to, before opening the Policy tab
- [ ] Answer all required lab report questions
- [ ] Commit `lab-02-clm-discovery-risk.md` to portfolio repo

---

**Lab 03 — CLM Policy Zones and Automation:**
- [ ] Return to the CLM Simulator and open the Policy tab
- [ ] Locate the zone groupings and identify which zone each of your Lab 02 top-5 certificates actually fell into
- [ ] Compare your Lab 02 predictions against the actual zone assignments and note any misses
- [ ] Identify the zone with the lowest overall compliance/passing rate in your fleet
- [ ] Explain the specific rule violations driving that zone's low pass rate
- [ ] Open the Automation tab and, before running anything, predict which of your flagged certificates can be safely automated
- [ ] Run the automated remediation step
- [ ] Compare the tool's actual automation decisions against your predictions
- [ ] Document which certificates were automated (trusted-CA reissuance, re-tagging) and which were held back for human review (self-signed/untrusted issuer, weak key)
- [ ] Explain in your own words why a high risk score alone never triggers automated reissuance for an untrusted or weak-key certificate
- [ ] Confirm every remediated certificate retained a valid assigned owner after automation
- [ ] Answer all required lab report questions
- [ ] Commit `lab-03-clm-policy-automation.md` to portfolio repo

---

**Lab 04 — CLM Monitoring and Reflection:**
- [ ] Open the Monitoring tab (Automation and the health check from earlier steps must have already run)
- [ ] Read the Fleet Health Dashboard and record the overall CRL freshness status
- [ ] Identify any certificates in the WARNING or CRITICAL freshness bands and note their NextUpdate timing
- [ ] Review OCSP reachability versus accuracy for your fleet and note any discrepancies
- [ ] Review the 30/60/90-day expiration pipeline counts
- [ ] Triage the severity-ordered alert feed and identify the top 3 alerts you would act on first, with justification
- [ ] Explain the soft-fail OCSP risk in the context of your fleet's scale
- [ ] Locate the reflection section (it only appears once both Automation and the Health Check have run) and answer all three required reflection questions
- [ ] Export the compliance report (`clm-report-<date>.txt`)
- [ ] Submit the exported report alongside your lab markdown
- [ ] Commit `lab-04-clm-monitoring-reflection.md` and the exported report to portfolio repo

---

## What "Good" Looks Like

- Lab 01's answer on why CLM still needs autoenrollment doesn't just repeat the lesson — it connects back to the student's own GPO work: "the CLM Simulator can score and flag certificates, but every one of those certificates still had to be issued by something — in a real environment, that something is usually autoenrollment, which is exactly the mechanism I configured in this lab."
- Lab 02's risk-score verification shows actual arithmetic, not just agreement with the tool: "This cert had 12 days to expiry (+20), a 1024-bit key (+35), and no assigned owner (+10) = 65, which the tool correctly placed in the High band."
- Lab 02's zone predictions are genuine predictions made before opening the Policy tab, and Lab 03 explicitly says whether each one was right or wrong rather than silently correcting the record.
- Lab 03's explanation of the automation boundary is precise about *why*, not just *what*: "Reissuing from our own trusted CA doesn't introduce a new party we have to trust — the CA was already trusted. Reissuing a self-signed cert would mean trusting whatever entity signed it in the first place, and that's a judgment call the tool correctly refuses to make on its own."
- Lab 04's alert triage justifies priority order using both the risk score and real-world consequence, not just the number: "I'd act on the domain controller certificate first even though its score was slightly lower than another alert, because a failure there affects authentication fleet-wide."
- Lab 04's reflection answers are specific to the student's own fleet and its actual numbers, not generic statements about CLM in general.

---

## Connecting to Prior Weeks

- **Week 10** — The certificate templates you designed are exactly what autoenrollment issues against in Lab 01 — template permissions are the first gate every autoenrolled certificate has to pass through.
- **Week 12** — The CRL freshness thresholds and OCSP soft-fail behavior you learned for a single CA are the same thresholds the CLM Monitoring tab applies across your entire fleet in Lab 04 — the concepts don't change, only the scale.
- **Week 13** — The trust-and-verification discipline from CA backup and recovery — never assuming something is fine without checking — is the same discipline behind Lab 03's automation boundary: some things are safe to do automatically, and some genuinely require a human to verify trust first.
- **Week 14** — The pkiview-then-certutil health check habit from Lesson 2 is the direct single-CA predecessor to Lab 04's fleet-wide health dashboard — you're reading the same signals, just across many CAs and certificates instead of one.
- **Week 16** — The capstone will expect you to reason with both the autoenrollment mechanism from Lab 01 and the CLM discipline from Labs 02–04 together, as parts of one architecture rather than separate topics.

---

*CVI PKI Career Pathway — Phase 2 Operations*
