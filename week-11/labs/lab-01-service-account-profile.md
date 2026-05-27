# Lab 01: Build a Certificate Profile for a Service Account

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 11  
**Submission Path:** `labs/week-11/lab-01-service-account-profile.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Proceed to Part A.

---

## Part A — Design the CVI-ServiceAccount Template

### Step 1 — Open the Certificate Templates Console

1. Press **Win + R**, type `certtmpl.msc`, and press **Enter**
2. The Certificate Templates console opens, showing all templates installed on this CA
3. Scroll through the list to get familiar with what already exists

### Step 2 — Duplicate the User Template

1. Scroll down to find the **User** template in the list
2. Right-click **User** → select **Duplicate Template**
3. A new template Properties window opens — this is your working copy

> **Why User?** The svc.autoenroll account is an AD user-type object, not a machine account. The User template is the correct baseline. The Computer template is for machine accounts and includes Server Authentication EKU, which is not appropriate for a service account.

**Source template duplicated:** ________________ (document your choice)

**Reason for choosing this source template:**

```
(your explanation here)
```

### Step 3 — Set Compatibility Settings

1. Click the **Compatibility** tab
2. Set **Certification Authority** to: `Windows Server 2012 R2`
3. Set **Certificate Recipient** to: `Windows 8.1 / Windows Server 2012 R2`
4. Click **OK** on any informational dialog that appears

**Compatibility settings selected:**
- Certification Authority: ________________
- Certificate Recipient: ________________

### Step 4 — Set the Template Name (General Tab)

1. Click the **General** tab
2. Change **Template display name** to: `CVI Service Account`
3. The **Template name** (internal name, no spaces) will auto-fill as `CVI-ServiceAccount` — confirm this
4. Note the **Schema version** shown at the bottom

**General tab — Template names:**

| Field | Value |
|-------|-------|
| Template display name | CVI Service Account |
| Template name (internal) | CVI-ServiceAccount |
| Schema version | |

### Step 5 — Configure Key Usage

1. Click the **Extensions** tab
2. In the Extensions list, select **Key Usage** → click **Edit**
3. In the Key Usage dialog:
   - Check **Digital Signature**
   - Uncheck **Key Encipherment** (if checked)
   - Uncheck **Data Encipherment** (if checked)
   - Uncheck **Non-Repudiation** (if checked)
   - Check **Make this extension critical**
4. Click **OK**

Document what you set and why:

| Key Usage | Included? | Reason |
|-----------|-----------|--------|
| Digital Signature | | |
| Key Encipherment | | |
| Data Encipherment | | |
| Non-Repudiation | | |

**Explanation of Key Usage decisions:**

```
(in your own words — why did you include what you included, and exclude what you excluded?)
```

### Step 6 — Configure Extended Key Usage (Application Policies)

1. Still on the **Extensions** tab, select **Application Policies** → click **Edit**
2. The User template comes with several EKUs pre-populated (Client Authentication, Encrypting File System, Secure Email). You need to remove all but one:
   - Select **Encrypting File System** → click **Remove**
   - Select **Secure Email** → click **Remove**
   - Leave **Client Authentication** (1.3.6.1.5.5.7.3.2) — this is the only EKU needed
3. Click **OK**

Document your EKU decisions:

| EKU | Included? | OID | Reason |
|-----|-----------|-----|--------|
| Client Authentication | | 1.3.6.1.5.5.7.3.2 | |
| Server Authentication | | 1.3.6.1.5.5.7.3.1 | |
| Code Signing | | 1.3.6.1.5.5.7.3.3 | |
| Secure Email | | 1.3.6.1.5.5.7.3.4 | |

**Explanation of EKU decisions:**

```
(in your own words — what does this certificate need to do, and why does that determine the EKU?)
```

### Step 7 — Configure Subject Name

1. Click the **Subject Name** tab
2. Select **Build from this Active Directory information**
3. Under **Subject name format**, select **User principal name (UPN)** from the dropdown
4. Uncheck any other options under "Include this information in alternate subject name" that are not needed

Document your settings:

| Setting | Value Selected | Reason |
|---------|---------------|--------|
| Subject name format | | |
| Include this information in the subject name | | |

**Explanation of Subject Name decision:**

```
(why does this certificate use "Build from AD" rather than "Supply in request"?)
```

### Step 8 — Set Validity Period

1. Click the **General** tab (you may already be on it)
2. Find the **Validity period** and **Renewal period** fields
3. Set **Validity period** to `1` year (or 2 years — document your reasoning)
4. Leave **Renewal period** at the default (6 weeks)

> **Note:** The CA itself has a maximum validity setting. If you set 5 years here but certs issue for only 2, the CA's max is overriding your template. You can check with: `certutil -getreg ca\ValidityPeriod`

| Setting | Value | Reason |
|---------|-------|--------|
| Validity period | | |
| Renewal period | | |

**Explanation of validity period decision:**

```
(why did you choose this period? how does it relate to the CA maximum and the service account lifecycle?)
```

### Step 9 — Set Enrollment Permissions (Security Tab)

1. Click the **Security** tab
2. You will see **Authenticated Users** and **Domain Admins** already listed
3. Add svc.autoenroll manually:
   - Click **Add**
   - In the object picker, type `svc.autoenroll` and click **Check Names**
   - Confirm the account resolves to `CORP\svc.autoenroll`, then click **OK**
   - With **svc.autoenroll** selected in the list, check **Read**, **Enroll**, and **Autoenroll**
4. Click **Apply**
5. Review the permissions on all three entries and document them below

| Group / Account | Read | Enroll | Autoenroll | Reason |
|-----------------|------|--------|------------|--------|
| Authenticated Users | | | | |
| CORP\svc.autoenroll | | | | |
| Domain Admins | | | | |

**Explanation of enrollment permission decisions:**

```
(why are permissions set this way? what risk does restricting Enroll to svc.autoenroll only prevent?)
```

### Step 10 — Save the Template

1. Click **OK** to close the Properties window and save the template
2. Verify **CVI-ServiceAccount** now appears in the certtmpl.msc list

**Template saved and visible in certtmpl.msc:**
- [ ] Yes

---

## Part B — Publish the Template and Issue the Certificate

### Step 1 — Publish the Template to the CA

1. Press **Win + R**, type `certsrv.msc`, and press **Enter**
2. Expand **CVI Issuing CA 1** in the left pane
3. Right-click **Certificate Templates** → **New** → **Certificate Template to Issue**
4. In the Enable Certificate Templates dialog, scroll to find **CVI-ServiceAccount**
5. Select it → click **OK**
6. The template should appear in the Certificate Templates node within 30 seconds. If it doesn't, right-click the node → **Refresh**

**CVI-ServiceAccount visible in Certificate Templates node:**
- [ ] Yes
- [ ] No — describe what happened:

```
(describe here)
```

---

### Step 2 — Request the Certificate for svc.autoenroll

> **Important:** svc.autoenroll is an AD user account, not a Windows service. The Certificates snap-in "Service account" option lists Windows system services only — svc.autoenroll will not appear there. Instead, use `runas` to open MMC running as the svc.autoenroll account.

1. Open **Command Prompt** or **PowerShell** as Administrator
2. Run the following command:
   ```
   runas /user:CORP\svc.autoenroll mmc.exe
   ```
3. Enter the **svc.autoenroll password** when prompted
4. A new MMC window opens — this window is running as svc.autoenroll
5. In the new MMC window: **File → Add/Remove Snap-in**
6. Select **Certificates** → click **Add**
7. Choose **My user account** → click **Finish** → click **OK**
8. Expand **Certificates (Current User)** → **Personal**

> **Note:** If no certificates have been issued yet for this account, you will not see a **Certificates** folder under Personal — this is expected. Right-click **Personal** directly → **All Tasks** → **Request New Certificate**. Once the certificate is issued, the Certificates folder will appear automatically.

9. Right-click **Personal** → **All Tasks** → **Request New Certificate**
10. Click **Next** through the enrollment wizard
11. Select **Active Directory Enrollment Policy** → click **Next**
12. The CVI-ServiceAccount template should appear in the list. Check the box next to it
13. Click **Enroll**
14. Enrollment should complete immediately (Status: Succeeded). Click **Finish**

**Enrollment wizard — enrollment policy selected:**

```
(Active Directory Enrollment Policy or other?)
```

**Templates visible in the wizard:**

```
(list all templates you see)
```

**CVI-ServiceAccount visible in wizard:**
- [ ] Yes
- [ ] No — troubleshooting steps taken:

**Certificate request submitted:**
- [ ] Yes — issued immediately
- [ ] Yes — pending manager approval (describe resolution):
- [ ] No — error encountered:

```
(paste error or describe outcome)
```

---

## Part C — Verify the Issued Certificate

### Step 1 — Inspect the Certificate with certutil

Open **PowerShell** on PKI-SRV01 as CORP\pki.admin and run:

```powershell
certutil -store My
```

> If the certificate was enrolled via the runas MMC session, it will be in the svc.autoenroll user's personal store, not the pki.admin store. To view it, you can either re-open the runas MMC window, or check the CA's Issued Certificates node (Step 2 below) to confirm issuance.

**Full certutil output:**

```
(paste output here)
```

**From the certutil output — record the following:**

| Field | Value |
|-------|-------|
| Subject | |
| Issuer | |
| Serial Number | |
| Key Usage | |
| Enhanced Key Usage (EKU) | |
| Validity: Not Before | |
| Validity: Not After | |
| Thumbprint | |

### Step 2 — Confirm in certsrv.msc and Record the Request ID

1. Open **certsrv.msc** (Press Win + R → type `certsrv.msc`)
2. Expand **CVI Issuing CA 1** → click **Issued Certificates**
3. Find the svc.autoenroll certificate in the list (sort by Request ID or Requester Name)
4. Double-click it to open and confirm it shows the CVI-ServiceAccount template
5. Record the values below

**Certificate visible in Issued Certificates node:**
- [ ] Yes

**Record from the Issued Certificates node:**

| Column | Value |
|--------|-------|
| Request ID | |
| Requester Name | |
| Certificate Template | |
| Issued Common Name | |
| Certificate Expiration Date | |

> **Save this Request ID.** You will use it in Week 12 to revoke this certificate, and in Lab 03 for the comparison exercise.

---

## Part D — Written Explanation

Answer the following questions in plain prose paragraphs — not bullet points. Aim for 2–3 paragraphs total across the two questions.

**What makes a service account certificate different from a user certificate? Address the following:**

1. Key difference in EKU — what does a user certificate include that the service account certificate should not, and why?
2. Subject Name source — both use "Build from AD," but what is different about the identity being represented?
3. Enrollment — who requests a user certificate vs. who requests a service account certificate, and why does this matter?

```
(your written explanation here)
```

**What are the operational risks of relying on password authentication for service accounts instead of certificate-based authentication?**

```
(your answer here — identify at least two specific risks)
```

---

## Reflection

**One thing about the CVI-ServiceAccount template design that was a non-obvious decision:**

```
(your observation here)
```

**What would you change about this template if this were a production environment rather than a lab?**

```
(your answer here — think about approval workflow, validity period, or monitoring)
```

---

## Submission Checklist

- [ ] Pre-lab verification completed
- [ ] Part A: Template duplicated from the User template
- [ ] Part A: All five design decisions (Key Usage, EKU, Subject Name, Validity, Security) documented with rationale
- [ ] Part A: Template saved as CVI-ServiceAccount and visible in certtmpl.msc
- [ ] Part B: Template published to CVI Issuing CA 1
- [ ] Part B: Certificate requested via runas /user:CORP\svc.autoenroll mmc.exe
- [ ] Part B: Certificate issued — enrollment outcome documented
- [ ] Part C: certutil output pasted and key fields extracted into table
- [ ] Part C: Request ID recorded from certsrv.msc Issued Certificates node
- [ ] Part D: Written explanation completed in prose
- [ ] Reflection section completed
- [ ] File saved as `lab-01-service-account-profile.md`
- [ ] File committed to portfolio repo under `labs/week-11/`
