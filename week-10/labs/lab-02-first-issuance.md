# Lab 02: Issue Your First Certificate from a Custom Template

**Student Name:**  
**Date Completed:**  
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-02-first-issuance.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Also confirm the following before starting:

```powershell
Get-Service -Name CertSvc
certutil -ping
```

**CertSvc status:** ________________  
**CA responding (certutil -ping):**
- [ ] Yes
- [ ] No — action taken:

**CVI-WebServer template visible in certtmpl.msc (from Lab 01):**
- [ ] Yes
- [ ] No — complete Lab 01 before proceeding

> **Lab 01 is required before Lab 02.** The CVI-WebServer template must exist in certtmpl.msc before you can publish it to the CA in Part A.

---

## Part A — Publish the Template to the CA

Publishing the template makes it available for certificate requests. The template definition lives in Active Directory — this step tells the CA to read it.

### Step 1 — Open the CA Management Console

1. On PKI-SRV01, press **Win + R**, type `certsrv.msc`, and press **Enter**
2. The Certification Authority management console opens

### Step 2 — Publish the Template

1. In the left pane, expand **CVI Issuing CA 1**
2. Right-click **Certificate Templates** → **New** → **Certificate Template to Issue**
3. The Enable Certificate Templates dialog opens
4. Scroll the list to find **CVI-WebServer**
5. Select it → click **OK**
6. **CVI-WebServer** should appear in the Certificate Templates node within 30 seconds. If it does not appear, right-click the node → **Refresh**

**CVI-WebServer visible in Certificate Templates node:**
- [ ] Yes
- [ ] No — describe what happened:

```
(describe here)
```

**What you observe in the Certificate Templates node after publishing:**

```
(describe what you see in certsrv.msc — how many templates are listed? does CVI-WebServer appear?)
```

---

## Part B — Request the Certificate via MMC

### Step 1 — Open MMC and Add the Certificates Snap-in

1. On PKI-SRV01 logged in as CORP\pki.admin, press **Win + R**, type `mmc.exe`, and press **Enter**
2. Go to **File → Add/Remove Snap-in**
3. In the Available snap-ins list, select **Certificates** → click **Add**
4. When prompted, select **My user account** → click **Finish**
5. Click **OK** to close the Add/Remove Snap-in dialog

### Step 2 — Navigate to the Certificate Store

1. In the left pane, expand **Certificates (Current User)**
2. Expand **Personal**

> **Note:** If no certificates have been issued yet to this account, you will not see a **Certificates** folder under Personal — this is expected. Right-click **Personal** directly → **All Tasks** → **Request New Certificate**. Once the certificate is issued, the Certificates folder will appear.

### Step 3 — Launch the Certificate Enrollment Wizard

1. Right-click **Personal** (or **Certificates** if the folder is visible) → **All Tasks** → **Request New Certificate**
2. Click **Next** through the "Before You Begin" screen

### Step 4 — Select Enrollment Policy

1. Select **Active Directory Enrollment Policy**
2. Click **Next**

**Enrollment policy selected:**

```
(Active Directory Enrollment Policy or other?)
```

### Step 5 — Select the CVI-WebServer Template

1. The template list appears. Review all visible templates
2. Locate **CVI-WebServer** in the list

**Templates visible in the wizard:**

```
(list all templates you can see)
```

**CVI-WebServer visible in the wizard:**
- [ ] Yes
- [ ] No — troubleshooting steps taken:

3. If a yellow warning triangle appears next to CVI-WebServer, click the link below the template name that reads **"More information is required to enroll for this certificate. Click here to configure settings."**
4. In the Certificate Properties dialog that opens, go to the **Subject** tab
5. Under Subject name, set **Type** = `Common name`, **Value** = `webserver.corp.cvilab.local`
6. Click **Add** → click **OK**

> **Why the yellow triangle?** The CVI-WebServer template uses "Supply in the request" — the CA requires you to provide the subject name yourself before it can issue the certificate. The common name you supply here (webserver.corp.cvilab.local) becomes the CN in the issued certificate.

**Subject name entered:**

```
(what subject name did you provide? e.g. webserver.corp.cvilab.local)
```

### Step 6 — Enroll

1. Check the box next to **CVI-WebServer** → click **Enroll**
2. Enrollment should complete immediately (Status: Succeeded)
3. Click **Finish**

**Certificate request outcome:**
- [ ] Issued immediately (Status: Succeeded)
- [ ] Pending manager approval — describe resolution:
- [ ] Error encountered:

```
(paste error here if applicable)
```

---

## Part C — Inspect the Issued Certificate

### Step 1 — Inspect in the MMC Certificates Snap-in

1. In the MMC left pane, expand **Certificates (Current User)** → **Personal** → **Certificates**
2. You should see the newly issued certificate in the list
3. Double-click it to open the Certificate dialog

**General tab:**

| Field | Value |
|-------|-------|
| Issued to | |
| Issued by | |
| Valid from | |
| Valid to | |

4. Click the **Details** tab and scroll through the fields

**Details tab — record the following:**

| Field | Value |
|-------|-------|
| Serial Number | |
| Signature Algorithm | |
| Subject | |
| Key Usage | |
| Enhanced Key Usage | |
| Subject Alternative Name (if present) | |
| Thumbprint | |

### Step 2 — Inspect with certutil

1. Copy the **Thumbprint** value from the Details tab (40 hex characters)
2. Open **PowerShell** on PKI-SRV01 as CORP\pki.admin
3. Run the following, replacing `<thumbprint>` with the value you copied (remove any spaces):

```powershell
certutil -store My "<thumbprint>"
```

> If that returns no results, the request was processed under your user account — try:
> ```powershell
> certutil -store -user My "<thumbprint>"
> ```

**Full certutil output:**

```
(paste output here)
```

### Step 3 — Confirm in certsrv.msc Issued Certificates Node

1. Open `certsrv.msc` (or switch back to it if already open)
2. Expand **CVI Issuing CA 1** → click **Issued Certificates**
3. Click the **Request ID** column header to sort by most recent
4. Locate the certificate you just requested

**Certificate visible in Issued Certificates node:**
- [ ] Yes

**Record from the Issued Certificates node:**

| Column | Value |
|--------|-------|
| Request ID | |
| Requester Name | |
| Certificate Template | |
| Issued Common Name | |
| Certificate Expiration Date | |

> **Save your Request ID.** You will use it in Week 12 to revoke this certificate, and it is also needed for the Week 11 Lab 03 profile comparison exercise. If you lose it later, run on PKI-SRV01: `certutil -view -out RequestId,CommonName,CertificateTemplate`

---

## Part D — Write-Up: The Issuance Workflow

Describe the full certificate issuance workflow in your own words, in plain prose paragraphs (not bullet points). Cover all four stages:

1. **What happened in Active Directory when you published the template** — what did publishing actually do?
2. **What the MMC Certificate Enrollment wizard sent to the CA** — what is a CSR and what does it contain?
3. **What the CA evaluated before issuing the certificate** — what checks did it perform?
4. **Where the issued certificate was placed and why** — where does it live now, and what other record was created?

```
(your write-up here — aim for 2 paragraphs minimum, plain language)
```

**One thing about the issuance process you did not expect or want to understand better:**

```
(your observation here)
```

---

## Submission Checklist

- [ ] Pre-lab verification completed — CertSvc running, CA responding, CVI-WebServer template confirmed in certtmpl.msc
- [ ] Part A: CVI-WebServer template published to CVI Issuing CA 1 via certsrv.msc
- [ ] Part A: Template visible in Certificate Templates node — confirmed
- [ ] Part B: MMC opened with Certificates snap-in (My user account)
- [ ] Part B: Active Directory Enrollment Policy selected
- [ ] Part B: CVI-WebServer template located in wizard — yellow triangle behavior documented if encountered
- [ ] Part B: Subject name supplied (webserver.corp.cvilab.local)
- [ ] Part B: Certificate enrollment outcome documented
- [ ] Part C: Certificate details recorded from MMC (General + Details tabs)
- [ ] Part C: certutil -store My output pasted in full
- [ ] Part C: Certificate confirmed in certsrv.msc Issued Certificates node
- [ ] Part C: Request ID recorded
- [ ] Part D: Issuance workflow write-up completed in own words — covers all 4 stages
- [ ] File saved as `lab-02-first-issuance.md`
- [ ] File committed to portfolio repo under `labs/week-10/`
