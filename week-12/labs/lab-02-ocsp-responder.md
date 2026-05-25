# Lab 02: Configure and Test the OCSP Responder

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 12  
**Submission Path:** `labs/week-12/lab-02-ocsp-responder.md`

---

## Prerequisites

Complete Lab 01 before starting Lab 02. You need:
- A revoked certificate from Lab 01 (for OCSP Revoked status testing)
- A valid, non-revoked certificate (your TLS cert from Week 10 or another cert from Week 11)

---

## Pre-Lab Verification

Run the following on PKI-SRV01 before starting.

```powershell
# Check 1 — CA service running
Get-Service -Name CertSvc

# Check 2 — Lab 01 complete: at least one revoked certificate in the CA
certutil -view -restrict "Disposition=21" -out "RequestID,SerialNumber,CommonName,RevokedReason" | head -10

# Check 3 — CRL is current (not expired)
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\<CA-Name>.crl" | findstr "NextUpdate"
```

**All checks passed:**
- [ ] Yes
- [ ] No — describe the issue and how you resolved it:

```
(describe here)
```

---

## Part A — Install the Online Responder Role

**Step 1:** Open Server Manager on PKI-SRV01.

Navigate to: **Manage → Add Roles and Features → Role-based or feature-based installation → PKI-SRV01**

Under **Active Directory Certificate Services**, select: **Online Responder**

Complete the installation. Do not restart unless prompted.

**Installation completed successfully:**
- [ ] Yes
- [ ] No — describe the error:

**Step 2:** Verify the Online Responder service is installed.

```powershell
Get-Service -Name OCSPSvc
```

```
(paste output here — confirm Status is Running)
```

---

## Part B — Configure the Revocation Configuration

**Step 1:** Open the Online Responder snap-in.

```
Start → Administrative Tools → Online Responder Management
```

Or:

```
mmc → Add Snap-in → Online Responder
```

**Step 2:** In the Online Responder snap-in, right-click **Revocation Configuration** → **Add Revocation Configuration**.

Work through the wizard:

**Revocation configuration name entered:**

```
(e.g., CVI Issuing CA 1 OCSP)
```

**CA certificate selection:** Select **Select a certificate for an Existing enterprise CA** and browse to CVI Issuing CA 1.

**CA certificate found and selected:**
- [ ] Yes
- [ ] No — describe:

**Step 3:** Complete the wizard. The Online Responder will attempt to issue an OCSP signing certificate from the CA.

**Status indicator after configuration (in Online Responder snap-in):**
- [ ] Green (operational)
- [ ] Yellow (warning — describe below)
- [ ] Red (error — describe below)

```
(describe what the snap-in shows — any warnings or errors, or confirmation of green status)
```

---

## Part C — Verify the OCSP Signing Certificate

Locate the OCSP signing certificate issued to the Online Responder.

**Step 1:** In the Online Responder snap-in, click the revocation configuration you created. Look for the signing certificate details in the lower pane.

**OCSP signing certificate subject:**

```
(paste the CN or subject from the signing certificate shown in the snap-in)
```

**Step 2:** Inspect the signing certificate using certutil.

First, export or locate the signing certificate. It should appear in the Personal certificate store on PKI-SRV01.

```powershell
# List certificates in the Personal store on PKI-SRV01
certutil -store My
```

```
(paste the output — identify the OCSP signing certificate entry)
```

**Step 3:** Dump and inspect the OCSP signing certificate.

```powershell
certutil -store My "<OCSP signing cert subject>"
```

**Verify the following properties in the output:**

| Property | Expected Value | Observed Value |
|---|---|---|
| Extended Key Usage | OCSP Signing (1.3.6.1.5.5.7.3.9) | |
| id-pkix-ocsp-nocheck extension | Present | |
| Key Usage | Digital Signature | |
| Issuer | CVI Issuing CA 1 | |
| Validity (NotAfter) | In the future | |

**All expected properties present:**
- [ ] Yes
- [ ] No — describe what is missing:

---

## Part D — Test the OCSP Responder

**Step 1:** Find the OCSP URL in the AIA extension of one of your certificates.

```powershell
# Inspect the AIA extension of a certificate issued by CVI Issuing CA 1
certutil -dump <certificate.cer> | findstr /i "OCSP\|Authority Information"
```

**OCSP URL found in the AIA extension:**

```
(paste the OCSP URL — e.g., http://pki-srv01.corp.cvilab.local/ocsp)
```

**Step 2:** Test a VALID certificate against the OCSP responder.

Use a certificate that was NOT revoked — your TLS certificate from Week 10 or another valid certificate from Week 11.

```powershell
certutil -URL <valid-certificate.cer>
```

In the URL Retrieval Tool:
- Select **OCSP** from the dropdown
- Click **Retrieve**

**OCSP response for the VALID certificate:**
- [ ] Good
- [ ] Revoked (unexpected — investigate)
- [ ] Unknown
- [ ] Connection timeout / error

```
(paste or describe the certutil -URL output for the valid certificate)
```

**Step 3:** Test the REVOKED certificate from Lab 01 against the OCSP responder.

```powershell
certutil -URL <revoked-certificate.cer>
```

**OCSP response for the REVOKED certificate:**
- [ ] Revoked
- [ ] Good (unexpected — investigate)
- [ ] Unknown
- [ ] Connection timeout / error

```
(paste or describe the certutil -URL output for the revoked certificate)
```

**Step 4:** Run a full certificate verification on both certificates.

```powershell
# Valid certificate — should succeed
certutil -verify <valid-certificate.cer>

# Revoked certificate — should fail with revocation error
certutil -verify <revoked-certificate.cer>
```

**certutil -verify output for the VALID certificate:**

```
(paste output — confirm "Verified" or similar success message)
```

**certutil -verify output for the REVOKED certificate:**

```
(paste output — confirm "Certificate is revoked" or similar error message)
```

---

## Part E — Inspect the AIA Extension in Context

```powershell
# Full AIA inspection of one of your certificates
certutil -dump <certificate.cer>
```

**From the AIA extension, identify both entries:**

**CA Issuers URL:**

```
(the URL that points to the issuing CA certificate for chain building)
```

**OCSP URL:**

```
(the URL that points to the Online Responder)
```

**Explain in one sentence what each URL is used for:**

```
CA Issuers URL:

OCSP URL:
```

---

## Part F — Lab Report

Answer all questions in your own words.

**1. What is the difference between querying a CRL and sending an OCSP query? From a relying party's perspective, what is the operational advantage of OCSP?**

```
(your answer here)
```

**2. The OCSP signing certificate has two properties not found on standard end-entity certificates. Name them and explain why each is required.**

```
(your answer here)
```

**3. Your Online Responder reads the CRL for its revocation data. What happens to the OCSP responses if the CRL becomes stale (expired) while the OCSP responder is still running?**

```
(your answer here)
```

**4. If the OCSP responder on PKI-SRV01 became unreachable, and a relying party was configured for hard-fail revocation checking, what would happen to certificate verifications in your environment?**

```
(your answer here)
```

**5. Compare the two test results from Part D: Good for the valid certificate and Revoked for the Lab 01 certificate. What did the OCSP response tell the relying party that a stale CRL could not?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] Online Responder role service installed and running (OCSPSvc)
- [ ] Revocation configuration created for CVI Issuing CA 1
- [ ] OCSP signing certificate issued and verified (EKU + NoCheck extension confirmed)
- [ ] Online Responder snap-in shows green status
- [ ] OCSP URL identified from AIA extension
- [ ] OCSP test for valid certificate: Good response documented
- [ ] OCSP test for revoked certificate (Lab 01): Revoked response documented
- [ ] certutil -verify output for both certificates included
- [ ] AIA extension entries (CA Issuers URL + OCSP URL) identified and explained
- [ ] All five lab report questions answered
- [ ] Lab report committed to `labs/week-12/lab-02-ocsp-responder.md`
