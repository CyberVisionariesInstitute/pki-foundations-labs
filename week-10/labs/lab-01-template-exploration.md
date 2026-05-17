# Lab 01: Explore and Duplicate a Certificate Template

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-01-template-exploration.md`

---

## Pre-Lab Verification

Run the following on PKI-SRV01 before starting. Do not proceed until all checks pass.

```powershell
# Check 1 — CA service running
Get-Service -Name CertSvc

# Check 2 — CA responding
certutil -ping

# Check 3 — Issuing CA cert in enterprise store
certutil -store -enterprise CA
```

**All checks passed:**
- [ ] Yes
- [ ] No — describe the issue and how you resolved it:

```
(describe here)
```

---

## Part A — Explore Three Built-in Templates

Open the Certificate Templates console: **Run → certtmpl.msc**

### Template 1: User

| Field | Value |
|-------|-------|
| Template Display Name | |
| Template Name (internal) | |
| Minimum Supported CA | |
| Validity Period | |
| Renewal Period | |

**General tab — notes:**

```
(your observations)
```

**Request Handling tab — Purpose:**

```
(Signature, Encryption, or Signature and Encryption?)
```

**Subject Name tab — Subject name format:**

```
(Which one should you select: Built from AD? Supplied in request? What fields?)
```

**Extensions tab — Key Usage:**

```
(list Key Usages present)
```

**Extensions tab — Application Policies (EKU):**

```
(list EKUs present)
```

---

### Template 2: Computer

| Field | Value |
|-------|-------|
| Template Display Name | |
| Template Name (internal) | |
| Validity Period | |
| Renewal Period | |

**Request Handling tab — Purpose:**

```
(Signature, Encryption, or Signature and Encryption?)
```

**Subject Name tab:**

```
(Built from AD? Supplied in request?)
```

**Extensions tab — Key Usage:**

```
(list Key Usages present)
```

**Extensions tab — Application Policies (EKU):**

```
(list EKUs present)
```

---

### Template 3: Web Server

| Field | Value |
|-------|-------|
| Template Display Name | |
| Template Name (internal) | |
| Validity Period | |
| Renewal Period | |

**Request Handling tab — Purpose:**

```
(Signature, Encryption, or Signature and Encryption?)
```

**Subject Name tab:**

```
(Built from AD? Supplied in request?)
```

**Extensions tab — Key Usage:**

```
(list Key Usages present)
```

**Extensions tab — Application Policies (EKU):**

```
(list EKUs present)
```

---

### Template Comparison

In your own words — what is the most significant difference between the User, Computer, and Web Server templates?

```
(your answer here — 3–5 sentences)
```

Why does the Web Server template use "Supplied in the request" for the subject name rather than building it from Active Directory?

```
(your answer here)
```

---

## Part B — Duplicate the Web Server Template

**Steps performed:**

1. Right-clicked the **Web Server** template → **Duplicate Template**
2. Selected compatibility settings:

   - Certification Authority: ________________
   - Certificate Recipient: ________________

3. Opened the properties of the new duplicate template.

**General tab — changes made:**

| Setting | Original Value | New Value |
|---------|---------------|-----------|
| Template display name | Web Server | CVI-WebServer |
| Template name | WebServer | CVI-WebServer |
| Validity period | | |
| Renewal period | | |

**Rationale for validity period chosen:**

```
(why did you choose this period?)
```

**Subject Name tab — changes made:**

| Setting | Change Made | Rationale |
|---------|-------------|-----------|
| | | |

**Security tab — permissions confirmed:**

| Group / Account | Enroll | Autoenroll |
|-----------------|--------|------------|
| Domain Admins | | |
| Authenticated Users | | |

**Ensure Write and Enroll permissions are checked under Authenticated Users**

**Template saved:**
- [ ] Yes — template visible in certtmpl.msc

---

## Part C — Inspect the Duplicate Template with certutil

Run the following command on PKI-SRV01:

```powershell
certutil -template CVI-WebServer
```

**Full output:**

```
(paste output here)
```

**From the certutil output — record the following:**

| Field | Value from certutil Output |
|-------|---------------------------|
| Template Name | |
| Template OID | |
| Schema Version | |
| Key Usage | |
| Enhanced Key Usage (EKU) | |
| Validity Period | |
| Subject Name flags | |

---

## Reflection

**Why does AD CS require you to duplicate a built-in template rather than modifying it directly?**

```
(your answer here)
```

**One setting in the template you found unexpected or would want to explore further:**

```
(your observation here)
```

---

## Submission Checklist

- [ ] Pre-lab verification completed and outputs recorded
- [ ] Part A: All three templates explored with tab-level observations
- [ ] Part A: Comparison question answered in own words
- [ ] Part B: Duplicate template created as CVI-WebServer
- [ ] Part B: All changes documented with rationale
- [ ] Part C: certutil -template output pasted and key fields extracted
- [ ] Reflection section completed
- [ ] File saved as `lab-01-template-exploration.md`
- [ ] File committed to portfolio repo under `labs/week-10/`
