# Week 13 — CA Backup & Recovery

## Focus

Week 12 built out the revocation infrastructure that tells the world when a certificate can no longer be trusted. Week 13 addresses a different question: what happens when the CA itself fails?

A CA is not just another Windows server. Its private key is the root of every certificate trust decision in the environment. Its database is the authoritative record of every certificate issued and every revocation performed. Its configuration — stored in Windows registry, Active Directory, and the certificate store — is the product of the deployment work in Weeks 9 through 12. Losing any of these components without a backup means starting over. The private key cannot be regenerated — it can only be lost.

Lesson 1 covers what a complete CA backup consists of and why each component matters. Three components are required: the CA database, which holds every issued certificate and revocation record; the CA private key and certificate, exported as a .p12 file using certutil -backup; and the Windows system state, which captures registry configuration, the local certificate store, and Active Directory objects that the CA depends on. Lesson 1 also introduces VSS — the Volume Shadow Copy Service — which is the Windows infrastructure that makes online CA backup possible. Understanding VSS is the prerequisite for Lab 01.

Lesson 2 covers backup strategy: how to decide backup frequency for different CA types, why the CRL publication period is directly tied to your Recovery Point Objective, how certutil -backup uses VSS to take a consistent snapshot without stopping the CA service, when stopping the CA service is appropriate, how to store backup media and private key passwords securely, and how to verify that a backup is actually recoverable before you need it.

Lesson 3 covers recovery procedures for three failure scenarios at increasing severity: Scenario 1 is a service failure — the OS is running but CertSvc will not start. Scenario 2 is an OS-level failure — the server is unbootable but the hardware is intact, with two recovery paths depending on whether a recent snapshot exists. Scenario 3 is hardware loss — the server is gone and the CA must be rebuilt on new hardware from backup files alone. Each scenario has a specific entry condition, a specific procedure, and an expected recovery time. Lesson 3 also covers the post-recovery verification checklist, which exists because a CA that starts its service but cannot publish CRLs or issue certificates is not recovered.

Lesson 4 covers PKI disaster recovery planning: how RTO and RPO are set based on operational dependencies rather than convenience, how CA unavailability cascades through the systems that depend on it, what the offline Root CA recovery procedure requires, and what belongs in a written PKI DR plan.

Three labs follow. Lab 01 performs a complete CA backup — database, private key, and system state — and produces a backup documentation record. Lab 02 simulates a CA database failure and recovers from a snapshot, tracing the full snapshot-restore-verify workflow. Lab 03, the stretch lab, performs a file-based restore from the Lab 01 backup files, simulating the recovery path that applies when no recent snapshot is available.

Week 14 is Monitoring and Alerting. The failure scenarios in Lesson 3 are precisely the events that a well-designed monitoring system catches before they escalate to recovery procedures. Come to Week 14 with all three lab submission records from this week.

---

## Outcomes

By the end of this week, you can:

- Identify the three components of a complete CA backup and explain what is lost — and what recovery requires — if each component is missing
- Perform a complete online CA backup using certutil -backup and wbadmin, with the CA service running throughout, and explain why no downtime is required
- Explain how the VSS ESE writer coordinates the freeze-snapshot-resume cycle inside certutil -backup — and why copying the .edb file directly while the CA is running is not a substitute
- Determine appropriate backup frequency for different CA types by connecting backup interval to RPO and CRL publication period
- Classify a CA failure into Scenario 1, 2, or 3 and select the correct recovery procedure for each — explaining why using the wrong procedure costs time
- Restore a CA from a VirtualBox or UTM snapshot and verify full operational state using certutil -ping, certutil -CRL, and the Application event log
- Restore a CA from backup files using certutil -restorekey and certutil -restoredb — and explain why the private key must be restored before the database
- Define RTO and RPO as applied to CA services and explain how each is set based on the systems that depend on the CA, not on technical convenience
- Write backup documentation and a recovery record that would be usable by another PKI administrator who was not present during the operation

---

## The Phase 2 Environment

The environment is unchanged from Weeks 9–12. Resolve any environment issues before starting Lab 01.

| Component | Detail | Notes |
|---|---|---|
| Domain | corp.cvilab.local | Windows Active Directory domain |
| DC01 | Domain Controller — runs AD DS and DNS | Start first. Wait for Server Manager to fully load before starting PKI-SRV01. |
| PKI-SRV01 (192.168.10.20) | Enterprise Issuing CA — CVI Issuing CA 1 | All backup and recovery work happens here. Log in as CORP\pki.admin. |
| Root-CA | Offline Root CA — not domain-joined | Leave powered off. Not required this week. |
| CORP\pki.admin | CA Administrator | All certutil commands, backup operations, CA console, wbadmin |
| CORP\cert.manager | Certificate Manager | Not required this week |
| C:\CABackup | CA backup destination (database + private key) | Created in Lab 01. Required for Lab 03. Do not delete between labs. |
| D:\ or alternate volume | System state backup destination | wbadmin requires a target volume different from C:. See Lab 01 for alternatives if only C: is available. |

> **Critical for Lab 03:** Record your Lab 01 backup password in a location separate from C:\CABackup before closing Lab 01. Lab 03 requires the password to run certutil -restorekey. If the password is lost, Lab 03 cannot be completed without re-running Lab 01.

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Full CA Backup | `labs/week-13/lab-01-ca-backup.md` |
| Lab 02 — CA Recovery Simulation | `labs/week-13/lab-02-ca-recovery-simulation.md` |
| Lab 03 — Restore from Backup Files *(Stretch)* | `labs/week-13/lab-03-restore-from-backup.md` |

All are markdown files committed to your GitHub portfolio repo under `labs/week-13/`. Each lab has its own submission template in the `/Labs/` folder — use it.

> **Lab sequencing is a hard dependency:** Lab 02 requires the Lab 01 backup files to be present in C:\CABackup before the snapshot is taken. Lab 03 requires the Lab 01 backup password. Complete labs in order: 01 → 02 → 03.

---

## Checklist

**Pre-lab (all labs):**
- [ ] Start DC01 first and wait for Server Manager to fully load before starting PKI-SRV01
- [ ] Confirm CertSvc is running: `Get-Service CertSvc`
- [ ] Confirm CA is accepting connections: `certutil -ping`
- [ ] Log in as CORP\pki.admin — confirm with `whoami` (expected: `corp\pki.admin`)

---

**Lab 01 — Full CA Backup:**
- [ ] Create C:\CABackup via File Explorer — confirm the folder is empty before running certutil
- [ ] Choose a backup password: at least 12 characters, uppercase + lowercase + numbers + symbols
- [ ] Record password storage location separately from C:\CABackup — write the location, not the password
- [ ] Run `certutil -backup -p <password> C:\CABackup` — paste full output
- [ ] Confirm both backup files are present: `<CAName>.p12` and `DataBase\<CAName>.edb` — record file sizes and timestamps
- [ ] Run `certutil -dump -p <password>` on the .p12 file — confirm it is readable and paste output
- [ ] Install Windows Server Backup feature if not already installed
- [ ] Identify backup target volume (different from C:) — document the drive or path used
- [ ] Run `wbadmin start systemstatebackup -backuptarget:D: -quiet` — paste output or document limitation with reason
- [ ] Confirm system state backup appears in `wbadmin get versions` with today's timestamp
- [ ] Confirm CA is still operational after backup: `Get-Service CertSvc`, `certutil -ping`, `certutil -CRL` — paste all three
- [ ] Answer all five lab report questions
- [ ] Commit `lab-01-ca-backup.md` to portfolio repo

---

**Lab 02 — CA Recovery Simulation:**
- [ ] Confirm Lab 01 backup files are present in C:\CABackup before taking any snapshot
- [ ] Record pre-failure CA state: certutil -CRL output, CA certificate thumbprint (first 8 characters), most recent issued certificate RequestID
- [ ] Shut down PKI-SRV01 cleanly before taking the snapshot
- [ ] Take snapshot on host machine — record exact snapshot name (VirtualBox or UTM procedure)
- [ ] Start PKI-SRV01 and confirm certutil -ping succeeds after restart before proceeding to destructive operation
- [ ] Choose destructive option: delete CA database files (recommended) or rename CertLog folder
- [ ] Confirm CA service is in a failed state — paste Get-Service CertSvc output and event log errors (Event IDs and messages)
- [ ] Document failure state in your own words: what the error means and what is causing it
- [ ] Shut down PKI-SRV01 and restore snapshot on host machine
- [ ] Start VM — confirm CA service is running without any manual intervention
- [ ] Post-recovery verification: certutil -ping, certutil -CRL, CRL accessible at HTTP CDP, database certificate count matches pre-failure baseline, event log clean (no CA errors)
- [ ] Record recovery completion timestamp and elapsed time from snapshot restore to CA fully operational
- [ ] Answer all five lab report questions
- [ ] Commit `lab-02-ca-recovery-simulation.md` to portfolio repo

---

**Lab 03 — Restore from Backup Files *(Stretch)*:**
- [ ] Confirm Lab 01 backup files are present in C:\CABackup and backup password is in hand before starting
- [ ] Take Lab03 pre-restore snapshot on host machine — record exact snapshot name
- [ ] Stop CertSvc and delete all files in C:\Windows\System32\CertLog — paste dir output confirming deletion
- [ ] Remove CA certificate from local machine certificate store (certlm.msc or PowerShell) — paste output
- [ ] Attempt Start-Service CertSvc — confirm failure and paste event log errors
- [ ] Run `certutil -restorekey "C:\CABackup\<CAName>.p12" <password>` — paste output confirming success
- [ ] Confirm CA certificate is back in local machine store with HasPrivateKey = True — paste Get-ChildItem output
- [ ] Run `certutil -restoredb "C:\CABackup"` — paste output confirming success
- [ ] Confirm database files are present in C:\Windows\System32\CertLog — paste dir output
- [ ] Run Start-Service CertSvc — confirm Running
- [ ] Post-recovery verification: certutil -ping, certutil -CRL, issued certificate history present, CRL accessible at HTTP CDP
- [ ] Record recovery timestamp and estimated time from Part A to CA fully operational
- [ ] Complete snapshot vs. file-based restore comparison table (time, password required, data currency, hardware dependency, steps)
- [ ] Answer all five lab report questions
- [ ] Commit `lab-03-restore-from-backup.md` to portfolio repo

---

## What "Good" Looks Like

- Lab 01's backup documentation reads as a professional record — every command output is pasted, the .p12 file size and timestamp are recorded, and the password storage location is explained as a security decision: "I stored the password in [location] and not in C:\CABackup because an attacker who accesses the backup folder should not also have the key to open the .p12 file."
- Lab 01's system state discussion explains what the system state backup captures that certutil -backup does not — and what a recovery operator would need to do manually without it.
- Lab 02's failure state documentation includes specific Event IDs with explanations of what they indicate. The recovery time is not just a number — it is contextualized: "This recovery took 12 minutes from snapshot restore to CA fully operational. Against a 4-hour RTO, that is comfortable. The risk is the data loss window from the snapshot date, not the recovery time."
- Lab 02's post-recovery verification explains what each check tests and why it is insufficient to simply confirm the CA service is running.
- Lab 03's restorekey-before-restoredb ordering is explained as a dependency, not just a sequence: "certutil -restoredb places the database files in CertLog — but if the CA service cannot find the private key when it starts, it will fail with Event ID 100 regardless of whether the database is intact."
- Lab 03's snapshot vs. file-based comparison table reflects direct experience from both labs, not guesses. Times, steps, and observations are specific.

---

## Connecting to Prior Weeks

- **Week 9** — The two-tier CA hierarchy you built in Week 9 determines why backup sequencing differs between the Root CA and the Issuing CA. The Root CA is backed up after each operation; the Issuing CA is backed up on a schedule. The architecture creates the operational distinction.
- **Week 10** — The HSM discussion in Week 10 applies directly to Lesson 1. If the Issuing CA private key is stored in an HSM, certutil -backup captures the certificate but not the key material — the HSM vendor's backup procedure handles the key. The backup is incomplete without understanding which component holds the private key.
- **Weeks 10–11** — Every certificate issued in Weeks 10 and 11 is a record in the CA database you back up in Lab 01. The backup is the authoritative history of everything the PKI has issued. If the CA fails and the only backup is three days old, every certificate issued in the last three days is an unrecorded issuance from the CA's perspective — even if the certificates themselves still exist in the field.
- **Week 12** — Revocation records are part of the CA database. The RPO you set for the CA determines how much revocation history is at risk. If the CA fails the day after you revoked a certificate and the backup was three days old, that revocation is absent from the restored database — but the CRL published before the failure may still be cached by relying parties, who see that certificate as revoked. This discrepancy is the RPO gap for CAs: what the database reflects versus what the CRL reflects.
- **Week 14** — Monitoring and Alerting. Every failure scenario in Lesson 3 has a precursor observable condition: a CA service that is slow to respond, a CRL approaching its NextUpdate, a disk filling up in the CertLog directory. The monitoring infrastructure in Week 14 is what turns those precursors into alerts — and alerts into prevention rather than recovery. Come to Week 14 with this week's labs complete.

---

*CVI PKI Career Pathway — Phase 2 Operations*
