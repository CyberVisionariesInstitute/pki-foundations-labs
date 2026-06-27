# Week 14 Labs — Root README

**Week:** 14 | **Phase:** 2 | **Topic:** CA Monitoring, Audit Logging & Cloud PKI Orientation

---

## Lab Overview

| Lab | Title | Type | Submission Path |
|-----|-------|------|-----------------|
| Lab 01 | AD CS Event Log Navigation | Core | `labs/week-14/lab-01-event-log-navigation.md` |
| Lab 02 | CA Health Check Routine | Core | `labs/week-14/lab-02-ca-health-check.md` |
| Lab 03 | Audit Logging Configuration and Verification | Stretch | `labs/week-14/lab-03-audit-logging-configuration.md` |

---

## Lab Environment

All labs run on PKI-SRV01 (192.168.10.20) in the corp.cvilab.local domain.

| Component | Details |
|-----------|---------|
| CA Server | PKI-SRV01 |
| CA Name | CVI Issuing CA 1 |
| Login Account | CORP\pki.admin |
| Domain | corp.cvilab.local |

**Prerequisite:** Labs 01 and 02 draw on the event history and certificate database built during Weeks 10–13. The CA must be in an operational state with at least one issued certificate and one revocation in the database. If the environment was restored from the "Phase 2 - Clean Start" snapshot, complete Lab 01 and Lab 02 of Week 10 before attempting these labs.

---

## Lab Sequencing

- **Lab 01** should be completed after Lesson 1. It uses the event log data already present on PKI-SRV01.
- **Lab 02** should be completed after Lesson 2. It requires a functioning OCSP responder — confirm Week 12 Lab 02 was completed.
- **Lab 03** is independent and can be completed any time after Lesson 3. It modifies the CA audit configuration — changes are non-destructive and can be reverted.

---

## Submission Format

All labs are submitted as completed markdown files committed to the student's GitHub repository. Commands must include actual output — not expected output. Screenshots are accepted for event log steps where copy-paste is not practical.

---

## Instructor Notes

See `Week14-Instructors-Lab-Guide.docx` for answer keys, expected outputs, and facilitation guidance for each lab.

Lab 03 requires two configuration changes: (1) enabling Audit Object Access in Local Security Policy and (2) setting the CA audit filter. Neither change affects CA availability. Both can be reversed with `certutil -setreg CA\AuditFilter 0` and the corresponding GPO setting.
