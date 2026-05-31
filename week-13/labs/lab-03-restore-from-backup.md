# Lab 03: Restore from Backup Files *(Stretch)*

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 13  
**Submission Path:** `labs/week-13/lab-03-restore-from-backup.md`

---

## Overview

This stretch lab has you restore the CA from the backup files you created in Lab 01 — not from a snapshot. This is a file-based restore: you will import the CA private key from the .p12 file, restore the CA database using `certutil -restoredb`, and verify the CA returns to full operational state. Lab 03 simulates a Scenario 2b or Scenario 3 recovery (Lesson 3) — the procedure that applies when no recent snapshot is available or when hardware must be replaced.

**Requirements before starting:**
- Lab 01 must be complete — you need the C:\CABackup files and your backup password
- Lab 02 must be complete — Lab 03 uses a different starting state than Lab 02

**If you do not have the Lab 01 backup password:** Lab 03 cannot be completed without it. Contact your instructor.

---

## Lab Environment

| Component | Details |
|---|---|
| CA Server | PKI-SRV01 (192.168.10.20) |
| CA Name | CVI Issuing CA 1 |
| Domain Account | CORP\pki.admin |
| Backup Source | C:\CABackup (from Lab 01) |
| Backup Password | The password you used in Lab 01 for certutil -backup -p |

---

## Pre-Lab Setup

### Step 1 — Confirm Lab 01 Backup Files Are Present

The Lab 03 restore requires the backup files from Lab 01 to still be present.

```powershell
Get-ChildItem C:\CABackup -Recurse | Select-Object FullName, Length, LastWriteTime
```

**Confirm these files exist:**

| File | Present? |
|---|---|
| C:\CABackup\<CAName>.p12 | [ ] Yes / [ ] No |
| C:\CABackup\DataBase\<CAName>.edb | [ ] Yes / [ ] No |

> **If the backup files were removed by the Lab 02 snapshot restore:** The snapshot restored PKI-SRV01 to its Lab 01 post-backup state — the backup files should be present. If they are not, you need to re-run Lab 01 before proceeding.

```
(paste Get-ChildItem output here)
```

### Step 2 — Confirm Your Backup Password

You will need the password you specified in Lab 01's `certutil -backup -p <password>` command. Do not proceed without it.

**I have the backup password from Lab 01:**
- [ ] Yes — password is in hand (do not record it here)
- [ ] No — I need to re-run Lab 01 before continuing

### Step 3 — Take a New Snapshot Before Starting

Before performing any destructive operations, take a fresh snapshot of PKI-SRV01. This gives you a rollback point if the restore procedure encounters unexpected issues.

Shut down PKI-SRV01 cleanly:
```powershell
Stop-Computer -Force
```

Then take the snapshot on your **host machine**. Follow the instructions for your hypervisor:

**VirtualBox:** Machine → Take Snapshot → Name it `Week13-Lab03-PreRestore-<today's date>` → Click OK

**UTM:** Right-click **PKI-SRV01** in the UTM sidebar → **Snapshots...** → click **+** → Name it `Week13-Lab03-PreRestore-<today's date>` → click OK

Then start PKI-SRV01 and log back in as CORP\pki.admin.

**Hypervisor:**
- [ ] VirtualBox
- [ ] UTM

**Lab03 pre-restore snapshot taken:**
- [ ] Yes — snapshot name recorded:
- [ ] No — reason:

---

## Part A — Simulate the Failure State

To demonstrate a file-based restore, you need the CA to be in a broken state — one that cannot be fixed with a snapshot restore alone. You will delete the CA database, the CA private key from the Windows certificate store, and clear the CertLog folder.

> ⚠️ **This is the point of no return for snapshot-based recovery.** After Part A, the only path back is the file-based restore in Parts B and C. The Lab03 pre-restore snapshot from Step 3 above is your safety net — it uses the Lab03 snapshot, not the Lab01 snapshot.

### Step 1 — Stop the CA Service

```powershell
Stop-Service CertSvc
Get-Service CertSvc
```

**CA service is stopped:**
- [ ] Yes — status shows Stopped

### Step 2 — Delete the CA Database Files

```powershell
# Remove all CA database files
Remove-Item "C:\Windows\System32\CertLog\*" -Force -Recurse
dir "C:\Windows\System32\CertLog\"
```

**Expected after deletion:** Empty directory or "File Not Found."

```
(paste dir output here)
```

### Step 3 — Remove the CA Private Key from the Windows Certificate Store

This simulates key material being deleted or unavailable — the more severe failure scenario.

```powershell
# List the CA certificate in the local machine store
certlm.msc
# Navigate to: Certificates (Local Computer) → Personal → Certificates
# Locate the "CVI Issuing CA 1" certificate
# Right-click → Delete
```

Alternatively, from PowerShell (find and remove the CA cert):

```powershell
# Find the CA certificate thumbprint
$caCert = Get-ChildItem Cert:\LocalMachine\My | Where-Object { $_.Subject -like "*CVI Issuing CA*" }
$caCert | Select-Object Subject, Thumbprint

# Remove the certificate (including private key if present)
Remove-Item "Cert:\LocalMachine\My\$($caCert.Thumbprint)"
```

```
(paste the commands and output here)
```

**CA certificate removed from local machine certificate store:**
- [ ] Yes
- [ ] Not found — document and continue (the certutil -restorekey step will handle this)

### Step 4 — Attempt to Start the CA and Document the Failure

```powershell
Start-Service CertSvc
Get-Service CertSvc

# Check event log for errors
Get-WinEvent -LogName Application -Source "CertificationAuthority" -MaxEvents 5 |
    Select-Object TimeCreated, Id, Message | Format-List
```

```
(paste both outputs here)
```

**Failure state documented:**

```
CA service status:
Event IDs observed:
Failure description in your own words:
```

---

## Part B — Restore the CA Private Key

The first step of a file-based restore is restoring the CA private key from the .p12 backup. Without the private key, the CA service cannot sign certificates or CRLs.

### Step 1 — Run certutil -restorekey

Replace `<YourBackupPassword>` with the password from Lab 01.

```powershell
certutil -restorekey "C:\CABackup\<CAName>.p12" <YourBackupPassword>
```

**Expected output:**
```
Restoring to container: <ContainerName>
CertUtil: -restorekey command completed successfully.
```

> **If you see "Cannot find object or property":** The path to the .p12 file is incorrect. The CA name contains spaces — the path must be in quotes. Use the exact form: `certutil -restorekey "C:\CABackup\CVI Issuing CA 1.p12" <YourBackupPassword>`. Verify the exact filename first: `dir C:\CABackup\*.p12`

> **If you see "The password does not meet the password policy requirements" or "The credentials supplied to the package were not recognized":** The password is incorrect. Re-check your Lab 01 notes. The password is case-sensitive.

> **If you see "The system cannot find the file specified":** The .p12 file does not exist at the specified path. Run `dir C:\CABackup` to confirm the filename and path.

```
(paste certutil -restorekey output here)
```

**certutil -restorekey completed successfully:**
- [ ] Yes
- [ ] No — error message and resolution:

### Step 2 — Verify the CA Certificate Is Back in the Certificate Store

```powershell
# Confirm the CA certificate and key are now in the local machine store
Get-ChildItem Cert:\LocalMachine\My | Where-Object { $_.Subject -like "*CVI Issuing CA*" } |
    Select-Object Subject, Thumbprint, HasPrivateKey
```

**Expected:** `HasPrivateKey = True` — confirming the key was restored.

```
(paste output here)
```

**CA certificate is in the local machine store with private key:**
- [ ] Yes — HasPrivateKey = True
- [ ] No — describe:

> **If HasPrivateKey shows False despite certutil -restorekey reporting success:** The key was restored to a different CSP container than the CA service expects. Run `certutil -store My` to see all certificates in the personal store and verify the correct CA certificate is listed. If the thumbprint does not match the CA certificate, re-run certutil -restorekey and confirm you are using the correct .p12 file.

---

## Part C — Restore the CA Database

With the private key restored, you now restore the CA database from the certutil backup.

### Step 1 — Run certutil -restoredb

```powershell
certutil -restoredb "C:\CABackup"
```

**Expected output:**
```
Restoring CA database...
Database restored successfully.
CertUtil: -restoredb command completed successfully.
```

> **If you see "The directory is not empty" or "File already exists":** The CertLog directory may have residual files. Clear it first:
> `Remove-Item "C:\Windows\System32\CertLog\*" -Force -Recurse`
> Then re-run certutil -restoredb.

> **If you see "The backup directory does not contain a valid database backup":** The path to the backup is incorrect, or the DataBase subfolder is missing. Verify: `dir C:\CABackup\DataBase\`

```
(paste certutil -restoredb output here)
```

**certutil -restoredb completed successfully:**
- [ ] Yes
- [ ] No — error message and resolution:

### Step 2 — Verify the Database Files Were Restored

```powershell
dir "C:\Windows\System32\CertLog\"
```

**Expected:** .edb file and log files are present.

```
(paste dir output here)
```

---

## Part D — Start the CA Service and Verify

### Step 1 — Start the CA Service

```powershell
Start-Service CertSvc
Get-Service CertSvc
```

**Expected:** Status = Running

```
(paste output here)
```

> **If the service fails to start with Event ID 100 — "CA certificate not found":** certutil -restorekey was not run before certutil -restoredb, or the restorekey step failed silently. Confirm HasPrivateKey = True in the certificate store (Step 2 above). If not, re-run certutil -restorekey first, then Start-Service CertSvc again.

> **If the service fails to start with Event ID 100 — "CA database could not be opened":** The CertLog directory still has residual files. Run `Remove-Item "C:\Windows\System32\CertLog\*" -Force -Recurse` and re-run certutil -restoredb, then Start-Service CertSvc.

> **If the service starts but certutil -CRL fails with "The system cannot find the path specified":** The CertEnroll folder may be missing or the service account lacks write access. Check: `dir "C:\Windows\System32\CertSrv\CertEnroll\"`. If the folder does not exist, create it: `New-Item -ItemType Directory -Path "C:\Windows\System32\CertSrv\CertEnroll" -Force`. Then retry certutil -CRL.

> **If Event ID 100 shows a CA name mismatch:** The CA name in the restored database does not match the installed CA configuration. Restore the system state backup from Lab 01 (wbadmin start systemstaterecovery) and retry.

**CA service started without errors:**
- [ ] Yes — proceed to verification
- [ ] No — error and resolution:

### Step 2 — Run the Full Post-Recovery Verification Checklist

```powershell
# 1. CA responds
certutil -ping

# 2. Publish CRL
certutil -CRL

# 3. Check event log for errors
Get-WinEvent -LogName Application -Source "CertificationAuthority" -MaxEvents 10 |
    Where-Object { $_.LevelDisplayName -eq "Error" } |
    Select-Object TimeCreated, Id, Message | Format-List
```

```
(paste all three outputs here)
```

### Step 3 — Confirm Certificate History Is Intact

```powershell
# View issued certificates from the restored database
certutil -view -restrict "Disposition=20" -out "RequestID,SerialNumber,CommonName,NotAfter" | head -20
```

```
(paste output here)
```

**Issued certificate history is present in the restored database:**
- [ ] Yes — records are present
- [ ] No — database appears empty (document and explain):

### Step 4 — Confirm CRL Is Accessible

```powershell
certutil -URL http://pki-srv01.corp.cvilab.local/CertEnroll/"CVI Issuing CA 1.crl"
```

Or navigate to the URL in a browser on PKI-SRV01.

```
(paste output or describe browser result)
```

**CRL is accessible at the HTTP CDP:**
- [ ] Yes
- [ ] No — describe:

**Recovery timestamp:**

```
(record date and time the CA returned to full operational state)
```

**Estimated time from Part A (simulated failure) to CA fully operational:**

```
(minutes)
```

---

## Part E — Snapshot vs. File-Based Restore Comparison

Reflect on your experience with both Lab 02 (snapshot restore) and Lab 03 (file-based restore).

### Direct Comparison

| Factor | Lab 02 — Snapshot Restore | Lab 03 — File-Based Restore |
|---|---|---|
| Time to complete | | |
| Password required? | | |
| Data currency | | |
| Hardware dependency | | |
| Steps required | | |

Fill in this table based on your direct experience.

---

## Part F — Lab Report

Answer all questions in complete sentences.

**1. Describe the file-based restore procedure you performed in Parts B and C. Walk through each command — certutil -restorekey and certutil -restoredb — and explain what each one does and why the order matters.**

```
(your answer here)
```

**2. In Part B, you restored the CA private key using certutil -restorekey. What would have happened if you had restored the CA database first (certutil -restoredb) without restoring the private key first — specifically, would the CA service have started? Why or why not?**

```
(your answer here)
```

**3. Compare your experience with Lab 02 (snapshot restore) and Lab 03 (file-based restore). Which was faster? Which required more steps? Which required the backup password? Which procedure would apply if the CA server hardware had been destroyed and you were restoring to a new machine?**

```
(your answer here)
```

**4. Explain the "environment mismatch" failure mode described in Lesson 3. What would cause certutil -restoredb to succeed but the CA service to fail to start? What would the event log show, and how would you resolve it?**

```
(your answer here)
```

**5. If this were a production CA with an RTO of 4 hours, would the file-based restore procedure you performed today meet that RTO? What factors could cause it to take longer — or shorter — in a production environment compared to your lab?**

```
(your answer here)
```

---

## Submission Checklist

- [ ] Logged in as CORP\pki.admin (not a local account)
- [ ] Lab 01 backup files confirmed present in C:\CABackup before starting
- [ ] Backup password available from Lab 01 records
- [ ] Lab03 pre-restore snapshot taken (VirtualBox or UTM) — snapshot name documented
- [ ] Failure state simulated — CA database deleted AND CA private key removed from cert store (both operations required)
- [ ] Failure state documented — CA service status and event log errors recorded
- [ ] certutil -restorekey run successfully — output included
- [ ] CA certificate confirmed back in local machine store with HasPrivateKey = True
- [ ] certutil -restoredb run successfully — output included
- [ ] CA database files confirmed in C:\Windows\System32\CertLog\ after restore
- [ ] CA service started without errors — Get-Service CertSvc shows Running
- [ ] certutil -ping successful after recovery
- [ ] certutil -CRL successful after recovery
- [ ] Event log clean (no CA errors after recovery)
- [ ] Issued certificate history confirmed present in restored database
- [ ] CRL accessible at HTTP CDP
- [ ] Recovery timestamp and estimated duration recorded
- [ ] Snapshot vs. file-based comparison table completed
- [ ] All five lab report questions answered in complete sentences
- [ ] Lab file committed to `labs/week-13/lab-03-restore-from-backup.md`
