# Lab 01: Explore and Duplicate a Certificate Template

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-01-template-exploration.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as CORP\pki.admin, you are communicating with DC01 and the environment is ready. Proceed to Part A.

---

## Part A — Explore Three Built-in Templates

### Step 1 — Open the Certificate Templates Console

1. On PKI-SRV01, press **Win + R**, type `certtmpl.msc`, and press **Enter**
2. The Certificate Templates console opens showing all templates installed on the forest
3. Scroll through the list to get familiar with what exists before locating your three target templates

### Step 2 — Explore the User Template

1. Scroll to find the **User** template in the list
2. Right-click **User** → select **Properties**
3. Work through each tab below and record what you observe

**General tab:**

| Field | Value |
|-------|-------|
| Template Display Name | |
| Template Name (internal) | |
| Validity Period | |
| Renewal Period | |
| Schema Version | |

**Request Handling tab — Purpose:**

```
(Signature, Encryption, or Signature and Encryption?)
```

**Subject Name tab:**

```
(Build from Active Directory? Supplied in request? What fields are included?)
```

**Extensions tab — Key Usage:**

```
(List all Key Usages present)
```

**Extensions tab — Application Policies (EKU):**

```
(List all EKUs present — include the OID for each, e.g. Client Authentication 1.3.6.1.5.5.7.3.2)
```

4. Close the Properties window when done

### Step 3 — Explore the Computer Template

1. Scroll to find the **Computer** template (note: the internal name is "Machine")
2. Right-click **Computer** → select **Properties**
3. Record the same tabs

**General tab:**

| Field | Value |
|-------|-------|
| Template Display Name | |
| Template Name (internal) | |
| Validity Period | |

**Request Handling tab — Purpose:**

```
(Signature, Encryption, or Signature and Encryption?)
```

**Subject Name tab:**

```
(Build from Active Directory? Supplied in request? What AD attribute is used?)
```

**Extensions tab — Key Usage:**

```
(List all Key Usages present)
```

**Extensions tab — Application Policies (EKU):**

```
(List all EKUs present — include OIDs)
```

4. Close the Properties window when done

### Step 4 — Explore the Web Server Template

1. Scroll to find the **Web Server** template
2. Right-click **Web Server** → select **Properties**

**General tab:**

| Field | Value |
|-------|-------|
| Template Display Name | |
| Template Name (internal) | |
| Validity Period | |

**Request Handling tab — Purpose:**

```
(Signature, Encryption, or Signature and Encryption?)
```

**Subject Name tab:**

```
(Build from Active Directory? Supplied in request? Note how this differs from User and Computer.)
```

**Extensions tab — Key Usage:**

```
(List all Key Usages present)
```

**Extensions tab — Application Policies (EKU):**

```
(List all EKUs present — include OIDs)
```

3. Close the Properties window when done

### Step 5 — Template Comparison Questions

In your own words — what is the most significant difference between the User, Computer, and Web Server templates? Address both the subject name source and the EKU differences in your answer.

```
(your answer here — 3–5 sentences)
```

Why does the Web Server template use "Supplied in the request" for the subject name rather than building it from Active Directory?

```
(your answer here — think about what Active Directory knows vs. what a web server's certificate needs to contain)
```

---

## Part B — Duplicate the Web Server Template

### Step 1 — Open the Duplicate Dialog

1. In `certtmpl.msc`, locate the **Web Server** template
2. Right-click **Web Server** → **Duplicate Template**
3. The Duplicate Template dialog appears

### Step 2 — Set Compatibility Settings

1. In the dialog, set the following compatibility settings:
   - **Certification Authority:** `Windows Server 2012 R2`
   - **Certificate Recipient:** `Windows 7 / Server 2008 R2` (or later)
2. Click **OK** on any informational dialog that appears

**Compatibility settings selected:**
- Certification Authority: ________________
- Certificate Recipient: ________________

### Step 3 — Configure the General Tab

1. A new template Properties window opens — go to the **General** tab
2. Change **Template display name** to: `CVI-WebServer`
3. Confirm the **Template name** (internal name, no spaces) reads exactly: `CVI-WebServer`
4. Set **Validity period** to `1` year
5. Leave **Renewal period** at the default (6 weeks)

> **Important:** The internal name cannot contain spaces. If your display name has a space (e.g., "CVI WebServer"), Windows strips it in the internal name, producing "CVIWebServer" instead of "CVI-WebServer". This will cause `certutil -template CVI-WebServer` to fail in Part C. Confirm the internal name field shows exactly `CVI-WebServer` with a hyphen before saving.

**General tab — changes made:**

| Setting | Original Value | New Value |
|---------|---------------|-----------|
| Template display name | Web Server | CVI-WebServer |
| Template name (internal) | WebServer | CVI-WebServer |
| Validity period | 2 years | 1 year |
| Renewal period | | |

**Rationale for validity period chosen:**

```
(why 1 year? how does this relate to the certificate lifecycle and reducing exposure?)
```

### Step 4 — Verify the Subject Name Tab

1. Click the **Subject Name** tab
2. Confirm **Supply in the request** is selected — this was inherited from the Web Server base template and must stay unchanged

**Supply in the request is selected:**
- [ ] Yes

### Step 5 — Confirm Security Tab Permissions

1. Click the **Security** tab
2. Confirm **Domain Computers** have **Enroll** checked
3. Confirm **Authenticated Users** have **Read** (not Enroll)
4. Click **Apply**

**Security tab — permissions confirmed:**

| Group / Account | Read | Enroll | Autoenroll |
|-----------------|------|--------|------------|
| Authenticated Users | | | |
| Domain Computers | | | |

### Step 6 — Save the Template

1. Click **OK** to close the Properties window and save the template
2. Verify **CVI-WebServer** now appears in the `certtmpl.msc` list. Press **F5** to refresh if it does not appear within 30 seconds.

**CVI-WebServer visible in certtmpl.msc:**
- [ ] Yes

---

## Part C — Inspect the Duplicate Template with certutil

### Step 1 — Run the certutil Command

1. Open **PowerShell** on PKI-SRV01 as CORP\pki.admin
2. Run:

```powershell
certutil -template CVI-WebServer
```

3. Copy the full output and paste it below

**Full certutil output:**

```
(paste output here)
```

### Step 2 — Extract Key Fields

From the certutil output, fill in the table below. No cells should be left blank.

| Field | Value from certutil Output |
|-------|---------------------------|
| Template Name | |
| Template OID | |
| Schema Version | |
| Key Usage | |
| Enhanced Key Usage (EKU) | |
| Validity Period | |
| Subject Name flags | |

> **Finding Key Usage:** Look for `Key Usage=` in the output — it shows a hex value like `0xa0`, which represents Digital Signature (0x80) + Key Encipherment (0x20).
>
> **Finding the Subject Name flag:** Look for `CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT` — this confirms "Supply in request" is set.

---

## Reflection

**Why does AD CS require you to duplicate a built-in template rather than modifying it directly?**

```
(your answer here — what is the consequence of modifying a built-in template, and why does the duplicate workflow protect against this?)
```

**One setting in the template you found unexpected or want to explore further:**

```
(your observation here — be specific about the field name and what question it raised)
```

---

## Submission Checklist

- [ ] Pre-lab verification completed — all three checks documented
- [ ] Part A: User template — General, Request Handling, Subject Name, and Extensions tabs recorded
- [ ] Part A: Computer template — same tabs recorded
- [ ] Part A: Web Server template — same tabs recorded
- [ ] Part A: Comparison question answered in own words (3–5 sentences)
- [ ] Part A: "Supply in the request" question answered with reasoning
- [ ] Part B: Duplicate dialog opened from Web Server template
- [ ] Part B: Compatibility settings recorded
- [ ] Part B: Internal name confirmed as `CVI-WebServer` (hyphen, no spaces)
- [ ] Part B: Validity period rationale provided
- [ ] Part B: Security tab permissions documented
- [ ] Part B: Template visible in certtmpl.msc confirmed
- [ ] Part C: `certutil -template CVI-WebServer` output pasted in full
- [ ] Part C: Key fields extracted (OID, schema version, Key Usage hex, EKU OID, subject name flags)
- [ ] Reflection section completed
- [ ] File saved as `lab-01-template-exploration.md`
- [ ] File committed to portfolio repo under `labs/week-10/`
- [ ] File saved as `lab-01-template-exploration.md`
- [ ] File committed to portfolio repo under `labs/week-10/`
