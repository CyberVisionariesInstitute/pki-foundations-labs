# Lab 01: Revoke a Certificate and Observe CRL Propagation

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 12  
**Submission Path:** `labs/week-12/lab-01-revoke-and-crl-propagation.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Proceed to Part A.

---

## Part A — Choose Your Certificate

You will revoke one of the certificates issued in Weeks 10 or 11. Select one and document your choice.

**Available candidates:**
- The service account certificate issued to `svc.autoenroll` (Lab 01, Week 11)
- The code signing certificate issued to `pki.admin` (Lab 02, Week 11)
- A TLS certificate issued in Week 10 (if available)

**Certificate selected for revocation:**

```
(Common Name / Subject)
```

**Serial number of the selected certificate:**

```powershell
# Run on PKI-SRV01 to find the serial number
certutil -view -restrict "CommonName=<cn-of-your-cert>" -out "RequestID,SerialNumber,CommonName,NotAfter"
```

```
(paste output here)
```

**Why you selected this certificate:**

```
(brief explanation — e.g., "I chose the service account cert because...")
```

---

## Part B — Revoke the Certificate

**Step 1:** Open the Certification Authority console on PKI-SRV01:
```
Run → certsrv.msc
```

Navigate to **Issued Certificates**. Locate your selected certificate by Common Name or Serial Number.

**Step 2:** Right-click the certificate → **All Tasks → Revoke Certificate**.

**Revocation reason code selected:**

| Reason Code | Selected? |
|---|---|
| Key Compromise | |
| CA Compromise | |
| Affiliation Changed | |
| Superseded | |
| Cessation of Operation | |

**Why you chose this reason code:**

```
(explain your reasoning — this is the operational design decision for this lab)
```

**Step 3:** Confirm the revocation. The certificate should now appear in the **Revoked Certificates** view.

**Confirm the certificate appears in Revoked Certificates:**
- [ ] Yes
- [ ] No — describe:

**Timestamp of revocation (from Revoked Certificates view):**

```
(record the revocation date/time shown in the CA console)
```

---

## Part C — Publish the CRL

After marking the certificate revoked, publish a new CRL immediately.

**Step 1:** In the CA console, right-click the CA name → **All Tasks → Publish**.

**Select:** New CRL (base CRL)

```
Publish result (any output or confirmation shown):
```

**Step 2:** Publish using certutil from the command line to confirm:

```powershell
certutil -CRL
```

```
(paste output here)
```

**Did the publication complete without errors?**
- [ ] Yes
- [ ] No — describe the error:

---

## Part D — Locate and Inspect the CRL

**Step 1:** Find the CRL Distribution Point URL in your certificate.

```powershell
# Inspect the CDP extension of the certificate you revoked
# Export the cert first if needed:
certutil -view -restrict "SerialNumber=<serial>" -out "RawCertificate" | certutil -dump
```

**CRL Distribution Point URL found in the certificate:**

```
(paste the HTTP URL from the CDP extension)
```

**Step 2:** Download the CRL from the distribution point and inspect it.

```powershell
# Download the CRL to the local machine
certutil -URL <certificate.cer>
# In the URL Retrieval Tool: test the CDP URL — confirm HTTP 200

# Or download directly and inspect:
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\<CA-Name>.crl"
```

**CRL inspection output (paste the relevant sections):**

```
ThisUpdate:
```

```
NextUpdate:
```

```
(paste the full certutil -dump output of the CRL here, or the most relevant sections)
```

**Step 3:** Confirm your revoked certificate appears in the CRL.

```powershell
# Search the CRL for your certificate's serial number
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\<CA-Name>.crl" | findstr -i "<your-serial-number>"
```

```
(paste output here)
```

**Does your revoked certificate serial number appear in the CRL?**
- [ ] Yes
- [ ] No — describe what you see and what you suspect:

---

## Part E — Verify Revocation Status

```powershell
# Full certificate verification — should now show revocation failure
certutil -verify <certificate.cer>
```

```
(paste output here)
```

**Does certutil -verify report the certificate as revoked?**
- [ ] Yes — paste the relevant error text:
- [ ] No — describe what you see:

---

## Part F — Lab Report

Answer all questions in your own words.

**1. Walk through the two-step revocation workflow you performed. What happens at each step — and why is step 2 not optional?**

```
(your answer here)
```

**2. You chose a specific revocation reason code. What would have changed if you had chosen Key Compromise instead? What would change operationally?**

```
(your answer here)
```

**3. What does the NextUpdate field in the CRL tell a relying party? If a relying party tries to verify your revoked certificate after the NextUpdate has passed, what happens?**

```
(your answer here)
```

**4. The certificate you revoked was issued to a specific identity. If a relying party in a soft-fail environment had already cached the CRL before you published the new one, would they know the certificate is revoked? Why or why not?**

```
(your answer here)
```

**5. What is one thing you would do differently or check additionally in a production environment that you did not need to do in this lab?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] Certificate selected and serial number documented
- [ ] Revocation reason code chosen with rationale
- [ ] Certificate confirmed in Revoked Certificates view
- [ ] CRL published manually (certutil -CRL output included)
- [ ] CRL Distribution Point URL identified from certificate
- [ ] CRL inspected: ThisUpdate and NextUpdate recorded
- [ ] Revoked serial number confirmed in CRL
- [ ] certutil -verify shows revocation
- [ ] All five lab report questions answered
- [ ] Lab report committed to `labs/week-12/lab-01-revoke-and-crl-propagation.md`
