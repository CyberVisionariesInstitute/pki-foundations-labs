# Lab 01: Environment Verification & VM Connectivity Check

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 9  
**Submission Path:** `labs/week-09/lab-01-environment-verification.md`

---

## Step 1 – VM Startup & Login

**VMs started in correct order (DC01 → PKI-SRV01 → Root-CA):**
- [ ] Yes
- [ ] No — describe what happened:

**Login credentials used:**

| VM | Account Used | Login Successful? |
|----|-------------|-------------------|
| DC01 | CORP\pki.admin | Yes / No |
| PKI-SRV01 | CORP\pki.admin | Yes / No |
| Root-CA | .\Administrator | Yes / No |

**Notes / issues encountered:**

```
(enter notes here)
```

---

## Step 2 – VM Connectivity Test

**Command run on PKI-SRV01:**

```powershell
Test-Connection -ComputerName DC01 -Count 2
```

**Output received:**

```
(paste your output here)
```

**DC01 responded successfully:**
- [ ] Yes
- [ ] No — troubleshooting steps taken:

---

## Step 3 – CertSvc Service Status

**Command run on PKI-SRV01:**

```powershell
Get-Service -Name CertSvc
```

**Output received:**

```
(paste your output here)
```

**CertSvc status shown:** ________________

**Service was Running:**
- [ ] Yes
- [ ] No — action taken:

---

## Step 4 – Certification Authority Console

**Steps completed on PKI-SRV01:**
- [ ] Opened certsrv.msc via Run dialog
- [ ] Confirmed CA name visible: ________________
- [ ] Confirmed CA status: ________________
- [ ] Expanded left pane — folders visible (Revoked Certificates, Issued Certificates, etc.)

**Screenshot or description of what you observed in certsrv.msc:**

```
(describe or reference your screenshot here)
```

---

## Step 5 – Certificate Log File Path

**Command run on PKI-SRV01:**

```powershell
Get-ChildItem "C:\Windows\System32\CertLog"
```

**Output received:**

```
(paste your output here)
```

**Files found in CertLog:**

| File Name | Approximate Size |
|-----------|-----------------|
| | |
| | |

---

## Reflection

**One thing that went well during this lab:**

```
(your response here)
```

**One thing that was confusing or unexpected:**

```
(your response here)
```

---

## Submission Checklist

- [ ] All five steps completed
- [ ] All command outputs pasted
- [ ] Reflection section filled in
- [ ] File saved as `lab-01-environment-verification.md`
- [ ] File committed to my portfolio repo under `labs/week-09/`
