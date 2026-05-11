# Week 10 — Student Submission Guide

**PKI Career Pathway · Phase 2 Operations · Cohort 1**

---

## Overview

Week 10 has three lab submissions. All three are markdown files committed to your GitHub portfolio repository.

**Your portfolio repo structure for Week 10:**

```
labs/
└── week-10/
    ├── lab-01-template-exploration.md
    ├── lab-02-first-issuance.md
    └── lab-03-hsm-observation.md
```

> **Important:** Lab 01 and Lab 02 are sequential — you must complete Lab 01 before Lab 02. Lab 03 is an observation report based on the live session instructor demonstration.

---

## Lab 01 — Explore and Duplicate a Certificate Template

**Submission file:** `labs/week-10/lab-01-template-exploration.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab verification | Get-Service CertSvc and certutil -ping outputs in code blocks |
| Part A — Template exploration | User, Computer, and Web Server templates documented with tab-level observations |
| Part A — Comparison | Written response explaining the key differences between the three templates — not just what is different, but why |
| Part B — Duplicate template | All changes documented with a reason for each change |
| Part B — Security tab | Permissions table completed |
| Part C — certutil output | Full certutil -template CVI-WebServer output pasted in a code block |
| Part C — Field extraction | Key fields extracted from certutil output into the table |
| Reflection | Both reflection questions answered |

**The comparison question in Part A matters.** Do not list settings — explain why the differences exist. The Web Server template requires this specifically: why does it use "Supplied in the request" for the subject name instead of building it from Active Directory?

**Format requirements:**
- All command output in fenced code blocks (` ``` `)
- Part A template exploration in structured tables
- Reflection in plain prose — not bullet points
- Use the provided lab-01-template-exploration.md template as your starting structure

**What not to include:**
- Screenshots of the Certificate Templates console (all required observations can be documented in the provided tables)
- The raw certutil output without interpretation — paste the output, then extract the key fields into the table below it

---

## Lab 02 — Issue Your First Certificate from a Custom Template

**Submission file:** `labs/week-10/lab-02-first-issuance.md`

**What to include:**

| Section | Requirement |
|---------|-------------|
| Pre-lab verification | CertSvc status and certutil -ping confirmed in code blocks |
| Part A — Template published | Confirmation that CVI-WebServer appears in certsrv.msc Certificate Templates node |
| Part B — MMC request | Enrollment wizard steps documented — enrollment policy, templates visible, subject name entered |
| Part B — Issuance outcome | Certificate issued / pending / error — documented with output or description |
| Part C — Certificate details | General tab and Details tab fields recorded |
| Part C — certutil output | Full certutil -store My output with thumbprint identified |
| Part C — certsrv.msc confirmation | Certificate confirmed in Issued Certificates node with Request ID |
| Part D — Write-up | Issuance workflow described in your own words — what happened at each stage |

**Part D is the most important section in Lab 02.** Describe what you actually did and what happened at each step — in your own words, not copied from the lesson. Cover: what publishing the template to the CA did, what the MMC enrollment wizard sent to the CA, what the CA evaluated, and where the certificate ended up.

**Format requirements:**
- All command output in fenced code blocks
- Part D written in prose paragraphs — not a list of steps
- Use the provided lab-02-first-issuance.md template as your starting structure

> **Forward note:** The CVI-WebServer certificate you issue in Lab 02 is used as a revocation target in Week 11. Do not delete it from the certificate store or revoke it before Week 11.

---

## Lab 03 — HSM Key Ceremony Observation Report

**Submission file:** `labs/week-10/lab-03-hsm-observation.md`

**This lab is an observation report.** The instructor runs the SoftHSM2 demonstration during the live session. Your job is to document what you observed with enough detail to explain it to someone who was not in the session.

**What to include:**

| Section | Requirement |
|---------|-------------|
| Part A — Setup context | SoftHSM2 and PKCS#11 described in your own words |
| Part B — Step-by-step | Each ceremony step documented: command run, what it did, output observed |
| Part C — Comparison table | SoftHSM2 vs. Thales Luna table completed based on lesson and demonstration |
| Part C — Physical HSM question | Written response — what does hardware protect against that software cannot? |
| Part D — Key ceremony concept | Explained in own words — what a ceremony is and why it exists |
| Part D — Enterprise differences | What would be different with a real HSM in production |
| Part E — Connection to Phase 2 | Software vs. HSM key risk addressed; Week 13 backup connection made |
| Reflection | Both questions answered |

**Part D and Part E are analytical, not descriptive.** Show that you understand the *why*, not just the what. What risk does a ceremony control for? What changes about backup and recovery when a CA key is HSM-resident rather than software-protected?

**Format requirements:**
- All commands and outputs in fenced code blocks
- Written responses in plain prose — not bullet lists
- Use the provided lab-03-hsm-observation.md template as your starting structure

**If you missed the live session:** The instructor will make the recording and the ceremony command sequence available in Kajabi after the session. Use both to complete Part B — do not leave it blank.

---

## Commit Instructions

Use whichever method you are most comfortable with. All three produce the same result.

**Commit message format:**
```
Week 10: <brief description>
```

Examples:
- `Week 10: Template exploration, first issuance, and HSM observation`
- `Week 10: Lab 01–03 complete — CVI-WebServer template issued and HSM ceremony documented`

---

### Option 1 — GitHub Web UI

1. Go to your portfolio repository on GitHub.com
2. Navigate to `labs/week-10/` (create the folder if it does not exist — type `labs/week-10/` as part of the file path when creating a new file)
3. Click **Add file → Upload files** (for existing files) or **Add file → Create new file** (to paste content directly)
4. Upload or paste your first lab file
5. Scroll to **Commit changes** at the bottom of the page
6. Enter your commit message: `Week 10: Template exploration, first issuance, and HSM observation`
7. Select **Commit directly to the main branch**
8. Click **Commit changes**
9. Repeat for remaining lab files, or drag and drop all three at once using Upload files for a single commit

> You can upload all three files in one go using **Upload files** and drag-and-drop — then write a single commit message covering all three.

---

### Option 2 — VS Code

1. Open VS Code and open your portfolio repo folder (`File → Open Folder`)
2. Place your completed lab files into the `labs/week-10/` folder using the Explorer panel (create the folder if needed)
3. Open the **Source Control** panel (Ctrl+Shift+G on Windows/Linux, Cmd+Shift+G on Mac)
4. You will see your lab files listed under **Changes**
5. Click the **+** icon next to each file to stage them (or click **+** next to **Changes** to stage all at once)
6. Type your commit message in the box at the top: `Week 10: Template exploration, first issuance, and HSM observation`
7. Click the **✓ Commit** button (or press Ctrl+Enter / Cmd+Enter)
8. Click **Sync Changes** (or the **↑** push icon) to push to GitHub

> If VS Code prompts you to publish the branch, click **Publish Branch**.

---

### Option 3 — Git Commands

```bash
# Navigate to your portfolio repo
cd your-pki-portfolio

# Create the week-10 folder if it does not exist
mkdir -p labs/week-10

# Stage, commit, and push all three lab files
git add labs/week-10/
git commit -m "Week 10: Template exploration, first issuance, and HSM observation"
git push
```

---

## Common Issues

**Lab 01:**

| Issue | Fix |
|-------|-----|
| CVI-WebServer template not visible after duplication | Refresh certtmpl.msc (F5). If still not visible, confirm you saved the template properties before closing. |
| certutil -template CVI-WebServer returns "Template not found" | The query uses the internal template name, not the display name. Confirm the internal name is `CVI-WebServer` (no spaces) in the General tab. |
| Template General tab shows grayed-out fields | Some fields on built-in templates are locked. Duplicate to a V2 or V3 template — confirm your compatibility settings in the duplicate dialog. |
| Cannot publish template to CA | Confirm you are logged in as CORP\pki.admin and that CVI Issuing CA 1 is selected in certsrv.msc before right-clicking Certificate Templates → New → Certificate Template to Issue. |

**Lab 02:**

| Issue | Fix |
|-------|-----|
| CVI-WebServer not visible in MMC enrollment wizard | Template may not be published to the CA yet. Return to certsrv.msc → Certificate Templates node and confirm CVI-WebServer appears there. |
| Certificate request shows "Pending" instead of issuing | CA may require manager approval for this template. Check the CVI-WebServer template → Issuance Requirements tab — CA certificate manager approval should be unchecked. |
| certutil -store My returns no results | Try `certutil -store -user My` if you requested as a user account, or check the Local Computer store if you requested as a computer account. |
| Certificate not visible in certsrv.msc Issued Certificates | Refresh the node (F5). Confirm you are looking at CVI Issuing CA 1, not a different CA. |

**Lab 03:**

| Issue | Fix |
|-------|-----|
| Missed the live session demo | Watch the recording. Complete Part B from the recording rather than leaving it blank. |
| Could not capture all commands during the demo | The instructor will post the ceremony command sequence in Kajabi after the session. Fill in Part B from the posted sequence plus your observation notes. |

---

*CyberVisionaries Institute · PKI Career Pathway Phase 2 · Cohort 1 · 2026*
