# Week 14 — Lab Submission Checklist

**Student Name:**
**Week:** 14
**Submission Deadline:** (see Kajabi)

---

## Lab 01 — AD CS Event Log Navigation

Submit to: `labs/week-14/lab-01-event-log-navigation.md`

- [ ] Logged into PKI-SRV01 as CORP\pki.admin
- [ ] Custom Event Viewer filter created for Source = CertificationAuthority
- [ ] At least three distinct event categories documented with Event ID, timestamp, and description
- [ ] certutil -view output for all issued certificates included
- [ ] certutil -view -restrict filter output for at least one disposition code included
- [ ] At least two specific certificate records identified and documented (from prior lab work)
- [ ] Part C analysis written — event log entry and certificate record interpreted with explanation
- [ ] All lab report questions answered in complete sentences
- [ ] File committed to `labs/week-14/lab-01-event-log-navigation.md`

---

## Lab 02 — CA Health Check Routine

Submit to: `labs/week-14/lab-02-ca-health-check.md`

- [ ] Logged into PKI-SRV01 as CORP\pki.admin
- [ ] certutil -CRL output documented (fresh CRL published without errors)
- [ ] certutil -URL CRL test output documented with pass/fail interpretation
- [ ] CRL NextUpdate timestamp recorded and hours-until-expiry calculated
- [ ] certutil -URL OCSP test output documented for a valid certificate
- [ ] certutil -URL OCSP test output documented for a revoked certificate (Week 12)
- [ ] certutil -view -restrict certificate expiration pipeline results for 30/60/90 days documented
- [ ] Health check summary table completed with pass/fail status for each signal
- [ ] Health check procedure written as a repeatable checklist
- [ ] All lab report questions answered in complete sentences
- [ ] File committed to `labs/week-14/lab-02-ca-health-check.md`

---

## Lab 03 — Audit Logging Configuration and Verification *(Stretch)*

Submit to: `labs/week-14/lab-03-audit-logging-configuration.md`

- [ ] Pre-configuration certutil -getreg CA\AuditFilter output documented
- [ ] Pre-configuration Local Security Policy Audit Object Access setting documented
- [ ] Audit Object Access enabled in Local Security Policy — confirmed with screenshot or policy output
- [ ] CA audit filter configured with certutil -setreg — command and output documented
- [ ] Post-configuration certutil -getreg CA\AuditFilter output confirms new value
- [ ] Test certificate issued — certutil or MMC confirmation included
- [ ] Test certificate revoked — certutil confirmation included
- [ ] CRL published after revocation — certutil -CRL output included
- [ ] Security event log Event ID 4887 (issuance) located and documented
- [ ] Security event log Event ID 4870 (revocation) located and documented
- [ ] Security event log Event ID 4872 (CRL publication) located and documented
- [ ] Before/after comparison documented
- [ ] All lab report questions answered in complete sentences
- [ ] File committed to `labs/week-14/lab-03-audit-logging-configuration.md`
