# Week 11 — Certificate Profiles & Complex Issuance

## Focus

Week 10 gave you one template and one certificate type. Week 11 gives you the framework to build any template — and three use cases to apply it.

This week moves beyond TLS. Enterprise PKI issues certificates to service accounts, code-signing identities, and users authenticating with smart cards. The CA infrastructure does not change — the template design does. Lesson 1 maps the full enterprise certificate landscape. Lesson 2 teaches template design as a five-decision framework. Lesson 3 applies that framework to service account certificates and non-human identity. Lesson 4 is a practitioner overview of smart cards, S/MIME, and code signing.

Three labs follow. Lab 01 builds the CVI-ServiceAccount template from scratch and issues a certificate to the svc.autoenroll account. Lab 02 designs and issues a code signing certificate, signs a PowerShell script, and verifies the signature. Lab 03 places three certificates — the TLS cert from Week 10, the service account cert from Lab 01, and the code signing cert from Lab 02 — side by side and asks you to explain why they are different.

Week 12 introduces certificate revocation. Come to it with Request IDs recorded for every certificate you have issued.

---

## Outcomes

By the end of this week, you can:

- Explain the six primary enterprise certificate use cases and identify the EKU each one requires
- Apply the five-decision template design framework to a new certificate use case: Key Usage, EKU, Subject Name source, Validity period, Enrollment permissions
- Design a certificate template for a service account — set the correct Key Usage, EKU, and enrollment restrictions with documented rationale
- Issue a certificate to a non-human account using the MMC Certificates snap-in Service account option
- Issue a code signing certificate, sign a PowerShell script with it, and verify the signature using Get-AuthenticodeSignature
- Place three certificate types side by side using certutil and explain the differences analytically — not just list them

---

## The Phase 2 Environment

The environment is unchanged from Weeks 9 and 10. If your environment is not running cleanly, resolve that before starting Lab 01.

| Component | Detail | Notes |
|---|---|---|
| Domain | corp.cvilab.local | Windows Active Directory domain |
| DC01 | Domain Controller — runs AD DS and DNS | Start first. Wait for Server Manager to fully load. |
| PKI-SRV01 | Enterprise Issuing CA — runs CVI Issuing CA 1 | Start second. Log in as CORP\pki.admin. |
| Root-CA | Offline Root CA — not domain-joined | Leave powered off. Not required this week. |
| CORP\pki.admin | CA Administrator | Manages templates, CRLs, CA configuration — all template and issuance work |
| CORP\cert.manager | Certificate Manager | Pending request approval — not required this week |
| CORP\svc.autoenroll | Service Account | The account that receives the Lab 01 certificate — do not disable or delete |
| CA Hierarchy | CVI Root CA (offline) → CVI Issuing CA 1 (PKI-SRV01) | Root stays offline — all work happens on PKI-SRV01 |

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Build a Certificate Profile for a Service Account | `labs/week-11/lab-01-service-account-profile.md` |
| Lab 02 — Issue a Code Signing Certificate | `labs/week-11/lab-02-code-signing-certificate.md` |
| Lab 03 — Compare Certificate Profiles Side by Side | `labs/week-11/lab-03-profile-comparison.md` |

All three are markdown files committed to your GitHub portfolio repo under `labs/week-11/`. Each lab has its own submission template in the `/Labs/` folder — use it.

> **Note on Lab 03:** Lab 03 requires the TLS certificate issued in Week 10 Lab 02. Before starting Lab 03, confirm that certificate is still in the Personal store on PKI-SRV01. Do not delete it.

---

## Checklist

**Pre-lab (Labs 01 and 02):**
- [ ] Start DC01 first and wait for Server Manager to fully load before starting PKI-SRV01
- [ ] Confirm CertSvc is running: `Get-Service CertSvc`
- [ ] Confirm CA is accepting connections: `certutil -ping`

**Lab 01 — Service Account Certificate Profile:**
- [ ] Open certtmpl.msc and duplicate a baseline template to create CVI-ServiceAccount
- [ ] Set Key Usage: Digital Signature, Key Encipherment
- [ ] Set EKU: Client Authentication only
- [ ] Set Subject Name: Build from Active Directory
- [ ] Set Validity: 1 year
- [ ] Set Security tab: Enroll permission for svc.autoenroll only; Read for Authenticated Users
- [ ] Document every setting with rationale in Part A
- [ ] Publish CVI-ServiceAccount to CVI Issuing CA 1 in certsrv.msc
- [ ] Open MMC Certificates snap-in → Service account → svc.autoenroll
- [ ] Request a certificate using CVI-ServiceAccount — confirm it issues (not pending)
- [ ] Run `certutil -store` for the service account store and confirm the certificate
- [ ] Record the Request ID from certsrv.msc Issued Certificates node
- [ ] Complete Part D written explanation
- [ ] Commit `lab-01-service-account-profile.md` to portfolio repo

**Lab 02 — Code Signing Certificate:**
- [ ] Open certtmpl.msc and duplicate the Code Signing template to create CVI-CodeSigning
- [ ] Set Key Usage: Digital Signature only
- [ ] Set EKU: Code Signing only
- [ ] Set Subject Name: Build from Active Directory
- [ ] Set Validity: 1 year
- [ ] Set Security tab: Enroll permission for pki.admin only
- [ ] Document every setting with rationale
- [ ] Publish CVI-CodeSigning to CVI Issuing CA 1
- [ ] Request the certificate as pki.admin via MMC Certificates (My user account)
- [ ] Verify the certificate with `certutil -store My` — confirm EKU is Code Signing
- [ ] Create a test PowerShell script `Test-CVI.ps1`
- [ ] Sign the script: `Set-AuthenticodeSignature -FilePath ... -Certificate $cert`
- [ ] Verify the signature: `Get-AuthenticodeSignature -FilePath ...` — Status = Valid
- [ ] Modify the script after signing and re-run Get-AuthenticodeSignature — Status = HashMismatch
- [ ] Record the Request ID from certsrv.msc
- [ ] Complete Part D written explanation
- [ ] Commit `lab-02-code-signing-certificate.md` to portfolio repo

**Lab 03 — Profile Comparison:**
- [ ] Locate the Week 10 TLS certificate in the Personal store — record the thumbprint
- [ ] Locate the Lab 01 service account certificate — record the thumbprint
- [ ] Locate the Lab 02 code signing certificate — record the thumbprint
- [ ] Run `certutil -store My "<thumbprint>"` for each certificate
- [ ] Build the comparison table in Part A: Key Usage, EKU, Subject, Validity, Subject Source
- [ ] Write the Part B analysis — explain *why* each difference exists, not just what it is
- [ ] Commit `lab-03-profile-comparison.md` to portfolio repo
- [ ] Commit all three labs with a meaningful message
- [ ] Record Request IDs for all three certificate types — needed for Week 12

---

## What "Good" Looks Like

- Lab 01 Part A is a design record, not a settings list. Every setting has a reason tied to the use case — "I set EKU to Client Authentication because this certificate authenticates a service account to other services. Adding Server Authentication or Secure Email EKU would grant capabilities this account should not have."
- The certutil output in Lab 01 and Lab 02 is pasted in full and then interpreted — key fields are extracted and identified, not just copied
- The Lab 02 hash mismatch test is documented with the actual output of Get-AuthenticodeSignature before and after the modification, not just described in words
- Lab 03 Part B is analytical prose — it explains the cause of each difference, not just names the differences. "The TLS certificate uses 'Supply in request' for the subject because the CA has no way to look up the DNS name of a web server in Active Directory — the DNS name is specific to the service being certified, not an AD object."
- Request IDs are recorded for all three certificate types — not just noted as "done"

---

## Connecting to Prior Weeks

- **Week 10** — The TLS certificate you issued in Lab 02 is the third certificate in Lab 03's comparison. Do not delete it. The template design framework applied this week extends what you built in Week 10 to three new use cases.
- **Week 3** — Certificate anatomy from Phase 1. The Key Usage and EKU extensions you dissected in Week 3 are the same extensions you are now designing in templates. This week closes the loop between reading a certificate and designing one.
- **Week 5** — Certificate lifecycle. Validity period decisions this week apply the same lifecycle thinking from Week 5 — but now you are making the decision as the template designer, not just observing it as a certificate consumer.
- **Week 12** — Every certificate you issue this week is a potential revocation target. The Request IDs you record in Labs 01 and 02 are the identifiers you will use in Week 12's revocation exercises. Complete the labs and record the IDs before Week 12 opens.
- **Week 15** — The svc.autoenroll service account certificate from Lab 01 will be referenced in the Week 15 autoenrollment lab. Do not revoke it.

---

*CVI PKI Career Pathway — Phase 2 Operations*
