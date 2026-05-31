# Week 13 — Student Submission Guide
## CA Backup & Recovery

---

## What to Submit This Week

Week 13 has three labs. Submit each completed lab as a markdown file committed to your GitHub portfolio repository under `labs/week-13/`.

| Lab | File to Submit | Path in Your Repo |
|---|---|---|
| Lab 01 | lab-01-ca-backup.md | `labs/week-13/lab-01-ca-backup.md` |
| Lab 02 | lab-02-ca-recovery-simulation.md | `labs/week-13/lab-02-ca-recovery-simulation.md` |
| Lab 03 (stretch) | lab-03-restore-from-backup.md | `labs/week-13/lab-03-restore-from-backup.md` |

---

## Before You Start

**Lab 01** is the foundation for Lab 02 and Lab 03. Complete Lab 01 first.

**Private key backup password:** In Lab 01, you create a backup password with `certutil -backup -p`. Record this password in a location separate from `C:\CABackup`. You will need it in Lab 03. If you lose the password, you cannot complete Lab 03.

**Lab order:** Lab 02 requires the backup files from Lab 01 to be present in `C:\CABackup`. Lab 03 requires the Lab 01 backup password. Do not skip labs.

---

## Lab 01 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Logged in as CORP\pki.admin | Use the domain account, not a local account |
| CA service running | Get-Service CertSvc shows Running |
| Available disk space | At least 5 GB on C:\ for the CA database backup |
| Second volume for system state | wbadmin requires a target different from C:\; see lab notes for alternatives if not available |

**What Lab 01 produces:**
- `C:\CABackup\CVI Issuing CA 1.p12` — CA private key backup (password-protected)
- `C:\CABackup\DataBase\CVI Issuing CA 1.edb` — CA database backup
- A Windows system state backup set on your target volume

---

## Lab 02 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Lab 01 complete | Backup files must be present in C:\CABackup |
| VirtualBox access on your host | Snapshot and restore operations are performed in VirtualBox Manager, not inside the VM |
| CA operational | Verify with certutil -ping before taking the snapshot |

**What Lab 02 requires you to do:**
1. Take a VirtualBox snapshot of PKI-SRV01 (after Lab 01 backup is confirmed present)
2. Perform a destructive operation to simulate CA failure
3. Document the failure state (Event ID 100, service stopped)
4. Restore the snapshot in VirtualBox
5. Run the full post-recovery verification checklist

---

## Lab 03 — What You Need to Complete It

| Requirement | Details |
|---|---|
| Lab 01 backup files | C:\CABackup\*.p12 and C:\CABackup\DataBase\*.edb must be present |
| Lab 01 backup password | Required for certutil -restorekey. Cannot proceed without it. |
| Lab 02 complete | Lab 03 builds on the Lab 02 experience for the comparison section |

**Lab 03 is a stretch lab.** It is not required for Lab 01 and Lab 02 completion, but it is the highest-value evidence artifact of Week 13 and the only lab that covers file-based restore — the procedure that applies when hardware replacement is required.

---

## What Your Lab Reports Must Include

### Lab 01

Your Lab 01 report is the backup documentation. Include:

- [ ] `whoami` output confirming CORP\pki.admin
- [ ] `certutil -ping` output confirming CA is responding before backup
- [ ] Full `certutil -backup -p` output
- [ ] File listing showing .p12 and .edb files present with sizes
- [ ] `certutil -dump` output confirming .p12 is readable
- [ ] Password storage location documented (not the password itself)
- [ ] `wbadmin start systemstatebackup` output (or documented limitation)
- [ ] `wbadmin get versions` output confirming backup completed
- [ ] `certutil -ping` and `certutil -CRL` confirming CA is still operational after backup
- [ ] All 5 lab report questions answered in complete sentences

### Lab 02

- [ ] Pre-failure CA state recorded (CRL status, certificate thumbprint)
- [ ] Snapshot name documented
- [ ] Destructive operation documented: which option chosen, service status and event log after failure
- [ ] Snapshot restore documented (VirtualBox steps)
- [ ] All 6 post-recovery verification steps passed and documented
- [ ] Recovery time recorded
- [ ] All 5 lab report questions answered in complete sentences

### Lab 03 (Stretch)

- [ ] Lab03 pre-restore snapshot name documented
- [ ] Failure state documented (database deleted + CA certificate removed from cert store)
- [ ] `certutil -restorekey` output (success required)
- [ ] `HasPrivateKey = True` confirmed in certificate store
- [ ] `certutil -restoredb` output (success required)
- [ ] All 6 post-recovery verification steps passed and documented
- [ ] Snapshot vs. file-based comparison table completed (all 5 rows)
- [ ] All 5 lab report questions answered in complete sentences

---

## Key Commands Reference

```powershell
# Create backup destination
New-Item -ItemType Directory -Path "C:\CABackup" -Force

# Backup CA database + private key
certutil -backup -p <YourPassword> C:\CABackup

# Verify .p12 is readable
certutil -dump -p <YourPassword> "C:\CABackup\CVI Issuing CA 1.p12"

# Backup system state
wbadmin start systemstatebackup -backuptarget:D: -quiet

# Verify system state backup
wbadmin get versions

# CA operational check
certutil -ping
certutil -CRL

# File-based restore (Lab 03) — ORDER MATTERS
certutil -restorekey "C:\CABackup\CVI Issuing CA 1.p12" <YourPassword>  # FIRST
certutil -restoredb "C:\CABackup"                                         # SECOND
Start-Service CertSvc                                                      # THIRD
```

---

## Submitting Your Work

1. Copy the completed lab file into your GitHub portfolio repository under `labs/week-13/`
2. Commit with a descriptive message: `Week 13 Lab 01 - CA Backup complete`
3. Push to your repository
4. Paste the GitHub permalink to your submission in the Kajabi assignment field

**Submission format:** The file you commit should be your completed version of the student lab guide — with all command outputs pasted in, all checkboxes checked, and all report questions answered. Do not submit a blank template.

---

*CyberVisionaries Institute | PKI Career Pathway | Phase 2 Operations*
*Cohort 1 · 2026 | cybervisionariesinstitute.org*
