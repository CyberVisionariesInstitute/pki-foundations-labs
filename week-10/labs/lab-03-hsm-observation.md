# Lab 03: HSM Key Ceremony — Observation Report

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-03-hsm-observation.md`

---

## Overview

This is an instructor-led demonstration. You observe and document — no hands-on configuration is required on your part. The instructor runs a **PKCS#11 key ceremony** using **SoftHSM2**, a software-based HSM simulator that exposes the same interface as a physical HSM like the Thales Luna Network HSM.

Your deliverable is a structured observation report. You document every command the instructor ran, what it produced, and what the equivalent step would look like with a real HSM in an enterprise environment.

> **If you missed the live session:** The recording and full command sequence will be posted to Kajabi. Complete this lab from those resources — all parts are required regardless of attendance.

---

## Pre-Lab — Setup Context

Before documenting the ceremony steps, answer the following to confirm your understanding of what you are watching.

### Part A — What Is SoftHSM2?

**Step 1 — In your own words, describe what SoftHSM2 is and why it is being used for this demonstration instead of a physical HSM:**

```
(your answer here — what is SoftHSM2, what does it simulate, and why is it a valid teaching tool?)
```

**Step 2 — In your own words, describe what the PKCS#11 interface is:**

```
(your answer here — what is PKCS#11, and why is it significant that SoftHSM2 uses the same interface as a physical HSM?)
```

**Step 3 — Record the setup details used in this demo:**

| Item | Value |
|------|-------|
| Software used | SoftHSM2 |
| PKCS#11 library path (Windows) | C:\Program Files\SoftHSM2\lib\softhsm2-x64.dll |
| Companion tool used for key ceremony commands | pkcs11-tool (from OpenSC) |

---

## Part B — Step-by-Step Ceremony Observation

Document each step of the ceremony as the instructor runs it. For each step, record the exact command, what it does, and what you observed in the output.

---

### Step 1 — Show Initial State

**Command run by instructor:**

```
(paste the exact command here)
```

**What this command does:**

```
(describe in your own words — what is the instructor showing before any ceremony steps have run?)
```

**Output observed:**

```
(paste or describe the output — what slot information appeared? was a token initialized?)
```

**Why this step is part of the ceremony:**

```
(your explanation — why does a real key ceremony begin by documenting the starting state?)
```

---

### Step 2 — Token Initialization

**Command run:**

```
(paste the exact command here)
```

**Break down each flag in the command:**

| Flag | Value Used | What It Does |
|------|-----------|--------------|
| --init-token | | |
| --slot | 0 | |
| --label | | |
| --pin | | |
| --so-pin | | |

**Output observed:**

```
(paste or describe the output)
```

**What is the Security Officer (SO) PIN and why is it separate from the User PIN?**

```
(your explanation — what does the SO PIN control, and why does the dual-PIN model matter for security?)
```

---

### Step 3 — Confirm Initialization

**Command run:**

```
(paste the exact command here — same command as Step 1)
```

**Output observed:**

```
(paste or describe the output — how has it changed since Step 1? what new information appears?)
```

**What the output confirms:**

```
(your observation — what does this output prove about the token's current state?)
```

---

### Step 4 — Key Pair Generation

**Command run:**

```
(paste the exact command here)
```

**Key parameters observed:**

| Parameter | Value Used |
|-----------|-----------|
| Key type | |
| Key size | |
| Key label | |
| Key ID | |
| Token (--login) | |

**Output observed:**

```
(paste or describe the output)
```

**What this step accomplished — explain in your own words:**

```
(where was the key pair generated? did the private key ever exist in system memory or on disk? 
what does "token-resident" mean in this context?)
```

---

### Step 5 — Confirm Key Residency

**Command run:**

```
(paste the exact command here)
```

**Output observed:**

```
(paste or describe the output — how many objects appeared? what are they labeled?)
```

**What the output proves:**

```
(describe what the list of objects demonstrates about where the key lives — 
and what would happen on a physical HSM if you tried to export the private key object?)
```

---

### Step 6 — Any Additional Steps Demonstrated

*(Use this section if the instructor ran additional commands — e.g. key attribute inspection, PKCS#11 URI display, or CA configuration reference. Leave blank if not applicable.)*

**Command:**

```
(paste command)
```

**What it showed:**

```
(your observation)
```

---

## Part C — SoftHSM2 vs. a Physical HSM

Using what you observed in the demonstration and the Week 10 lesson materials, complete the comparison table. No cells should be left blank.

**Physical HSM reference: Thales Luna Network HSM**

| Dimension | SoftHSM2 (Demo) | Thales Luna (Enterprise) |
|-----------|-----------------|--------------------------|
| Key storage location | Software (filesystem, encrypted) | |
| Tamper resistance | None — root-level OS access can reach key files | |
| PKCS#11 interface | Yes — same interface as physical HSMs | |
| FIPS 140 validation | Not applicable (software simulation) | |
| Used in production CAs | No — simulation and training only | |
| Cost | Free / open source | |
| Private key exportable? | Yes (with filesystem access) | |
| Required for a CA private key in enterprise? | No — but recommended | |

**In your own words: what does a physical HSM protect against that SoftHSM2 cannot?**

```
(your answer here — 2–3 sentences. Be specific: name the attack or access scenario 
that a physical HSM defeats and SoftHSM2 cannot.)
```

---

## Part D — The Key Ceremony Concept

**What is a key ceremony?**

In your own words, describe what a key ceremony is and why enterprise PKI deployments conduct them with multiple administrators present. Your answer should explain the M-of-N quorum concept and what it prevents.

```
(your answer — 3–5 sentences)
```

**What would be different about this ceremony if CVI Issuing CA 1 were using a physical HSM?**

Address at least three of the following differences: physical presence requirements, multi-admin quorum for the SO PIN, ceremony documentation, air-gapped environment, HSM-to-HSM backup procedure.

```
(your answer)
```

---

## Part E — Connection to Phase 2

**Software-protected CA key vs. HSM-protected CA key**

CVI Issuing CA 1 in your lab environment stores its private key in software (the Windows CNG Key Storage Provider). Based on what you observed in this demonstration:

**What is the operational risk of a software-stored CA private key compared to an HSM-protected key?**

```
(your answer — be specific about what an attacker with privileged access to PKI-SRV01 could do 
that they could not do against an HSM-protected key)
```

**When you back up the CA in Week 13, what specific step becomes more sensitive because the private key is software-protected?**

```
(your answer — think about what certutil -backup and the -backupkey flag export, 
and why the output file is especially sensitive when the key is software-stored)
```

---

## Reflection

**The most important thing you took away from this demonstration:**

```
(your answer — be specific, not generic. Name a command, a concept, or a distinction 
that changed how you think about CA key management.)
```

**One question the demonstration raised that you want to understand better:**

```
(your question here)
```

---

## Submission Checklist

- [ ] Part A: SoftHSM2 described in own words — not lifted from lesson text
- [ ] Part A: PKCS#11 described in own words — significance of the shared interface explained
- [ ] Part B: Step 1 — initial state command, output, and significance documented
- [ ] Part B: Step 2 — token initialization command, all flags broken down, SO PIN role explained
- [ ] Part B: Step 3 — confirmation command and output documented
- [ ] Part B: Step 4 — key generation command, all parameters recorded, "token-resident" explained
- [ ] Part B: Step 5 — key residency command, output documented, private key export behavior explained
- [ ] Part C: SoftHSM2 vs. Thales Luna comparison table fully completed — no blank cells
- [ ] Part C: Physical HSM protection question answered with a specific attack scenario
- [ ] Part D: Key ceremony concept explained with M-of-N quorum detail
- [ ] Part D: Enterprise ceremony differences cover at least 3 of the listed items
- [ ] Part E: Software key risk identified — specific to privileged OS access
- [ ] Part E: Week 13 backup connection made — certutil -backupkey and PFX sensitivity addressed
- [ ] Reflection completed with specific observation
- [ ] File saved as `lab-03-hsm-observation.md`
- [ ] File committed to portfolio repo under `labs/week-10/`
