# Lab 02: Issue a Code Signing Certificate

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 11  
**Submission Path:** `labs/week-11/lab-02-code-signing-certificate.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Proceed to Part A.

---

## Part A — Design the CVI-CodeSigning Template

### Step 1 — Open the Certificate Templates Console

1. Press **Win + R**, type `certtmpl.msc`, and press **Enter**
2. The Certificate Templates console opens showing all templates installed on this CA

### Step 2 — Duplicate the Code Signing Template

1. Scroll through the list to find the built-in **Code Signing** template
2. Right-click **Code Signing** → select **Duplicate Template**
3. A new template Properties window opens — this is your working copy

> **Why Code Signing and not User?** The built-in Code Signing template already has the correct Key Usage (Digital Signature only) and EKU (Code Signing only) pre-configured. Starting from User would require removing multiple EKUs and introduces the risk of leaving incorrect settings in place.

**Source template duplicated:** ________________

### Step 3 — Set Compatibility Settings

1. Click the **Compatibility** tab
2. Set **Certification Authority** to: `Windows Server 2012 R2`
3. Set **Certificate Recipient** to: `Windows 8.1 / Windows Server 2012 R2`
4. Click **OK** on any informational dialog that appears

**Compatibility settings:**
- Certification Authority: ________________
- Certificate Recipient: ________________

### Step 4 — Set the Template Name (General Tab)

1. Click the **General** tab
2. Change **Template display name** to: `CVI Code Signing`
3. Confirm the **Template name** (internal) auto-fills as `CVI-CodeSigning`

**Template names:**

| Field | Value |
|-------|-------|
| Template display name | CVI Code Signing |
| Template name (internal) | CVI-CodeSigning |

### Step 5 — Configure Key Usage

1. Click the **Extensions** tab
2. Select **Key Usage** in the list → click **Edit**
3. Confirm only **Digital Signature** is checked. Uncheck anything else if present
4. Check **Make this extension critical**
5. Click **OK**

| Key Usage | Included? | Reason |
|-----------|-----------|--------|
| Digital Signature | | |
| Key Encipherment | | |
| Non-Repudiation | | |

**Explanation of Key Usage decision:**

```
(why is Digital Signature the only required Key Usage for a code signing certificate?)
```

### Step 6 — Configure Extended Key Usage (Application Policies)

1. Still on the **Extensions** tab, select **Application Policies** → click **Edit**
2. Confirm only **Code Signing (1.3.6.1.5.5.7.3.3)** is listed
3. If any other EKUs are present, select them and click **Remove**
4. Click **OK**

| EKU | Included? | Reason |
|-----|-----------|--------|
| Code Signing (1.3.6.1.5.5.7.3.3) | | |
| Client Authentication | | |
| Other | | |

**Explanation of EKU decision:**

```
(what does the Code Signing EKU control — and why should no other EKU be added?)
```

### Step 7 — Configure Subject Name

1. Click the **Subject Name** tab
2. Select **Build from this Active Directory information**
3. Under Subject name format, select **User principal name (UPN)**

| Setting | Value | Reason |
|---------|-------|--------|
| Subject name source | | |
| Subject built from | | |

### Step 8 — Set Validity Period and Enrollment Permissions

**Validity:**
1. Click the **General** tab
2. Set **Validity period** to `1` year (or 2 — document your reasoning)
3. Leave **Renewal period** at the default

**Security / Enrollment Permissions:**
1. Click the **Security** tab
2. Select **Authenticated Users** — confirm Read is checked, Enroll is NOT checked
3. Select **Domain Admins** — confirm Read and Enroll are checked
4. pki.admin should inherit Enroll through Domain Admins. If it does not, click **Add** → type `pki.admin` → Check Names → OK → check **Read** and **Enroll**
5. Do NOT enable Autoenroll — code signing certificates should require a deliberate enrollment action
6. Click **Apply**

| Setting | Value | Reason |
|---------|-------|--------|
| Validity period | | |
| Enroll — account(s) granted | | |
| Autoenroll | | |

### Step 9 — Save the Template

1. Click **OK** to close the Properties window
2. Verify **CVI-CodeSigning** now appears in the certtmpl.msc list

**Template saved:**
- [ ] Yes — visible in certtmpl.msc

---

## Part B — Publish and Issue the Certificate

### Step 1 — Publish the Template to the CA

1. Press **Win + R**, type `certsrv.msc`, and press **Enter**
2. Expand **CVI Issuing CA 1** in the left pane
3. Right-click **Certificate Templates** → **New** → **Certificate Template to Issue**
4. Scroll to find **CVI-CodeSigning** → select it → click **OK**
5. The template should appear in the Certificate Templates node within 30 seconds. If it doesn't, right-click the node → **Refresh**

**CVI-CodeSigning visible in Certificate Templates node:**
- [ ] Yes

### Step 2 — Request the Certificate (as pki.admin)

1. Press **Win + R**, type `mmc.exe`, and press **Enter**
2. Go to **File → Add/Remove Snap-in**
3. Select **Certificates** → click **Add**
4. Choose **My user account** → click **Finish** → click **OK**
5. In the left pane, expand **Certificates (Current User)** → expand **Personal**

> **Note:** If no certificates have been issued yet, you will not see a **Certificates** folder under Personal — this is expected. Right-click **Personal** directly → **All Tasks** → **Request New Certificate**. Once the certificate is issued, the Certificates folder will appear automatically.

6. Right-click **Personal** → **All Tasks** → **Request New Certificate**
7. Click **Next** through the enrollment wizard
8. Select **Active Directory Enrollment Policy** → click **Next**
9. The **CVI-CodeSigning** template should appear in the list. Check the box next to it
10. Click **Enroll**
11. Enrollment should complete immediately. Click **Finish**

**Certificate issued:**
- [ ] Yes — immediately
- [ ] Pending — describe:

```
(describe outcome)
```

### Step 3 — Record the Request ID

1. Open **certsrv.msc**
2. Expand **CVI Issuing CA 1** → click **Issued Certificates**
3. Find the pki.admin code signing certificate
4. Record the Request ID below

**Request ID from certsrv.msc Issued Certificates node:** ________________

> **Save this Request ID.** It is used in Week 12 revocation and in Lab 03.

### Step 4 — Verify the Certificate

Open **PowerShell** on PKI-SRV01 and run:

```powershell
certutil -store My
```

**Full certutil output for the code signing certificate:**

```
(paste output here)
```

Locate the CVI-CodeSigning certificate in the output and confirm the EKU field:

| Field | Value |
|-------|-------|
| Subject | |
| EKU | |
| Validity | |
| Thumbprint | |

**EKU = 1.3.6.1.5.5.7.3.3 (Code Signing) confirmed:**
- [ ] Yes
- [ ] No — describe discrepancy:

---

## Part C — Sign a PowerShell Script

### Step 1 — Create the Test Script

Open **PowerShell** on PKI-SRV01 as CORP\pki.admin and run the following block. Copy and paste it exactly:

```powershell
$scriptContent = @'
# CVI Phase 2 — Week 11 Code Signing Test
Write-Host "This script is signed with a CVI code signing certificate."
Write-Host "Issued to: pki.admin"
Write-Host "Date: $(Get-Date)"
'@

New-Item -Path "C:\Scripts" -ItemType Directory -Force
Set-Content -Path "C:\Scripts\Test-CVI.ps1" -Value $scriptContent
```

**Script created at C:\Scripts\Test-CVI.ps1:**
- [ ] Yes

### Step 2 — Retrieve the Code Signing Certificate

Run the following to confirm the certificate is accessible and select it into a variable:

```powershell
$cert = Get-ChildItem -Path Cert:\CurrentUser\My -CodeSigningCert | Select-Object -First 1
$cert | Select-Object Subject, Thumbprint, NotAfter
```

**Output of certificate selection:**

```
(paste output here)
```

> If this returns nothing, the certificate was not issued with the Code Signing EKU. Go back to certtmpl.msc, check the Application Policies on CVI-CodeSigning, and re-enroll.

### Step 3 — Sign the Script

```powershell
$result = Set-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1" -Certificate $cert
$result
```

**Set-AuthenticodeSignature output:**

```
(paste output here)
```

Expected result: **Status = Valid**

### Step 4 — Verify the Signature

```powershell
Get-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1"
```

**Full Get-AuthenticodeSignature output:**

```
(paste output here)
```

**Status:**
- [ ] Valid
- [ ] Other — describe:

### Step 5 — Check for a Timestamp

```powershell
(Get-AuthenticodeSignature "C:\Scripts\Test-CVI.ps1").TimeStamperCertificate
```

**TimeStamperCertificate output:**

```
(paste output — $null if no timestamp)
```

**Timestamp present:**
- [ ] Yes — note the timestamp authority:
- [ ] No — note this in Part D

### Step 6 — Hash Mismatch Test

Modify the script after signing to verify the signature breaks:

```powershell
Add-Content -Path "C:\Scripts\Test-CVI.ps1" -Value "# Modified after signing"

Get-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1"
```

**Get-AuthenticodeSignature output after modification:**

```
(paste output here)
```

**Status after modification:**
- [ ] HashMismatch
- [ ] Other — describe:

---

## Part D — Written Explanation

Answer the following in plain prose paragraphs — not bullet points.

**What does the Code Signing EKU enforce, and at what layer?**

Cover: what application or OS component checks for the Code Signing EKU, what it does when the EKU is present vs. absent, and how this is different from the cryptographic validity check.

```
(your explanation here)
```

**What did the hash mismatch test demonstrate about what the signature is protecting?**

Cover: what the signature covers (the code hash), what the mismatch status means, and why this matters for software integrity in a production environment.

```
(your explanation here)
```

**Should the CVI-CodeSigning template require CA certificate manager approval in a production environment? Why or why not?**

```
(your answer here)
```

---

## Reflection

**Why is a timestamp operationally significant for a code signing certificate — particularly for software that will be distributed and executed over a long period?**

```
(your answer here)
```

**One thing about the code signing workflow you would want to understand better or configure differently:**

```
(your observation here)
```

---

## Submission Checklist

- [ ] Pre-lab verification completed
- [ ] Part A: Template duplicated from the built-in Code Signing template
- [ ] Part A: All settings configured — Key Usage, EKU, Subject Name, Validity, Enrollment Permissions
- [ ] Part A: Template saved as CVI-CodeSigning and visible in certtmpl.msc
- [ ] Part B: Template published to CVI Issuing CA 1
- [ ] Part B: Certificate issued to pki.admin — Request ID recorded
- [ ] Part B: certutil output pasted with EKU confirmed as Code Signing (1.3.6.1.5.5.7.3.3)
- [ ] Part C: Test script created at C:\Scripts\Test-CVI.ps1
- [ ] Part C: Certificate selection output pasted
- [ ] Part C: Set-AuthenticodeSignature output pasted (Status = Valid)
- [ ] Part C: Get-AuthenticodeSignature output pasted (Status = Valid)
- [ ] Part C: Timestamp check output pasted
- [ ] Part C: Hash mismatch test output pasted (Status = HashMismatch)
- [ ] Part D: Written explanation completed in prose
- [ ] Reflection completed
- [ ] File saved as `lab-02-code-signing-certificate.md`
- [ ] File committed to portfolio repo under `labs/week-11/`
