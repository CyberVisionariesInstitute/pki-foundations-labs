# Lab 02: Issue a Code Signing Certificate

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 11  
**Submission Path:** `labs/week-11/lab-02-code-signing-certificate.md`

---

## Pre-Lab Verification

Run on PKI-SRV01 before starting.

```powershell
Get-Service -Name CertSvc
certutil -ping
```

**CertSvc status:** ________________  
**CA responding:** Yes / No

---

## Part A — Design the CVI-CodeSigning Template

Open the Certificate Templates console: **Run → certtmpl.msc**

### Template Duplication

**Source template duplicated:** ________________ (use the built-in Code Signing template)

**Compatibility settings:**
- Certification Authority: ________________
- Certificate Recipient: ________________

### Design Decisions

**1 — Key Usage**

| Key Usage | Included? | Reason |
|-----------|-----------|--------|
| Digital Signature | | |
| Key Encipherment | | |
| Non-Repudiation | | |

**Explanation of Key Usage decision:**

```
(why is Digital Signature the only required Key Usage for a code signing certificate?)
```

**2 — EKU**

| EKU | Included? | Reason |
|-----|-----------|--------|
| Code Signing (1.3.6.1.5.5.7.3.3) | | |
| Client Authentication | | |
| Other | | |

**Explanation of EKU decision:**

```
(what does the Code Signing EKU control — and why should no other EKU be added?)
```

**3 — Subject Name**

| Setting | Value | Reason |
|---------|-------|--------|
| Subject name source | | |
| Subject built from | | |

**4 — Validity and Enrollment Permissions**

| Setting | Value | Reason |
|---------|-------|--------|
| Validity period | | |
| Enroll — account(s) granted | | |
| Autoenroll | | |

**Template names:**

| Field | Value |
|-------|-------|
| Template display name | CVI Code Signing |
| Template name (internal) | CVI-CodeSigning |

**Template saved:**
- [ ] Yes — visible in certtmpl.msc

---

## Part B — Publish and Issue the Certificate

### Publish the Template

**Steps performed:**

1. certsrv.msc → CVI Issuing CA 1 → Certificate Templates → New → Certificate Template to Issue
2. Selected **CVI-CodeSigning**

**CVI-CodeSigning visible in Certificate Templates node:**
- [ ] Yes

### Request the Certificate (as pki.admin)

**Steps performed:**

1. mmc.exe → Certificates snap-in → **My user account**
2. Personal → All Tasks → Request New Certificate
3. Enrollment policy: ________________
4. Template selected: **CVI-CodeSigning**
5. Enrolled

**Certificate issued:**
- [ ] Yes — immediately
- [ ] Pending — describe:

```
(describe outcome)
```

**Request ID from certsrv.msc Issued Certificates node:** ________________

> **Save this Request ID.** It is used in Week 12 revocation and in Lab 03.

### Verify the Certificate

```powershell
certutil -store My
```

**Full certutil output for the code signing certificate:**

```
(paste output here)
```

**Key fields confirmed:**

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

### Create the Test Script

Create a simple PowerShell script on PKI-SRV01:

```powershell
# Create the test script
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

---

### Sign the Script

```powershell
# Get the code signing certificate
$cert = Get-ChildItem -Path Cert:\CurrentUser\My -CodeSigningCert | Select-Object -First 1

# Confirm this is the right certificate
$cert | Select-Object Subject, Thumbprint, NotAfter
```

**Output of certificate selection:**

```
(paste output here)
```

```powershell
# Sign the script
$result = Set-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1" -Certificate $cert
$result
```

**Set-AuthenticodeSignature output:**

```
(paste output here)
```

---

### Verify the Signature

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

**Is a timestamp present?**

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

---

### Hash Mismatch Test (Destructive)

Modify the script after signing to verify the signature breaks:

```powershell
# Add content to the signed script
Add-Content -Path "C:\Scripts\Test-CVI.ps1" -Value "# Modified after signing"

# Re-verify
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

**What does the Code Signing EKU enforce, and at what layer?**

Cover: what application or OS component checks for the Code Signing EKU, what it does when the EKU is present vs. absent, and how this is different from the cryptographic validity check.

```
(your explanation here — prose paragraphs)
```

**What did the hash mismatch test demonstrate about what the signature is protecting?**

Cover: what the signature covers (the code hash), what the mismatch status means, and why this matters for software integrity in a production environment.

```
(your explanation here — prose paragraphs)
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
- [ ] Part A: CVI-CodeSigning template designed with rationale for all settings
- [ ] Part A: Template created and visible in certtmpl.msc
- [ ] Part B: Template published to CVI Issuing CA 1
- [ ] Part B: Certificate issued to pki.admin — Request ID recorded
- [ ] Part B: certutil output pasted with EKU confirmed as Code Signing
- [ ] Part C: Test script created
- [ ] Part C: Script signed and Set-AuthenticodeSignature output pasted
- [ ] Part C: Get-AuthenticodeSignature output = Valid, pasted
- [ ] Part C: Timestamp check output pasted
- [ ] Part C: Hash mismatch test performed and output pasted (Status = HashMismatch)
- [ ] Part D: Written explanation in prose — EKU enforcement and hash mismatch
- [ ] Reflection completed
- [ ] File saved as `lab-02-code-signing-certificate.md`
- [ ] File committed to portfolio repo under `labs/week-11/`
