# Week 10 — Certificate Templates & First Issuance

## Focus

This week you stop reading the environment and start operating it.

Week 9 gave you the orientation: the three-VM lab, the two-tier CA hierarchy, the console layout, and the verification habit. You know where the CA lives, who signed its certificate, and how to confirm it is running before you touch anything. Week 10 puts that foundation to work. You will learn how the CA decides what to put inside a certificate — the certificate template — and then you will issue one.

Three labs follow the four lessons. Lab 01 takes you inside the Certificate Templates console: you examine built-in templates, compare their settings, duplicate one, and modify it for a specific purpose. Lab 02 publishes your new template to the CA and walks you through the full request-to-issuance workflow — from the MMC enrollment wizard to finding your certificate in the Issued Certificates node. Lab 03 is an observation report from the live session: the instructor runs a SoftHSM2 key ceremony and you document what you see, connecting software-based key storage to the hardware protection model introduced in Lesson 4.

Week 11 introduces certificate revocation. Come to it having issued a certificate you understand.

---

## Outcomes

By the end of this week, you can:

- Describe the structure and purpose of a certificate template, and explain how Key Usage and Extended Key Usage constrain what a certificate can do
- Navigate CA Properties and identify the settings that govern CRL publication schedule, certificate validity periods, and issuance policy
- Duplicate a built-in certificate template, modify its settings for a defined purpose, and publish it to an enterprise CA using certsrv.msc
- Submit a certificate request through the MMC Certificates snap-in, locate the issued certificate in certsrv.msc, and verify it with certutil
- Explain what PKCS#11 is, why hardware-based key storage matters, and how a key ceremony differs from software key generation
- Describe the operational risk difference between software-protected CA private keys and HSM-protected keys

---

## The Phase 2 Environment

The environment does not change from Week 9. If your environment is not running cleanly, resolve that before starting Lab 01.

| Component | Detail | Notes |
|---|---|---|
| Domain | corp.cvilab.local | Windows Active Directory domain |
| DC01 | Domain Controller — runs AD DS and DNS | Start first. Wait for Server Manager to fully load. |
| PKI-SRV01 | Enterprise Issuing CA — runs CVI Issuing CA 1 | Start second. Log in as CORP\pki.admin. |
| Root-CA | Offline Root CA — not domain-joined | Leave powered off. Not required this week. |
| CORP\pki.admin | CA Administrator | Manages templates, CRLs, CA configuration — all lab work this week |
| CORP\cert.manager | Certificate Manager | Requests certificates — used in Lab 02 enrollment |
| CA Hierarchy | CVI Root CA (offline) → CVI Issuing CA 1 (PKI-SRV01) | Root signed the Issuing CA cert and stays offline |

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab 01 — Explore and Duplicate a Certificate Template | `labs/week-10/lab-01-template-exploration.md` |
| Lab 02 — Issue Your First Certificate from a Custom Template | `labs/week-10/lab-02-first-issuance.md` |
| Lab 03 — HSM Key Ceremony Observation Report | `labs/week-10/lab-03-hsm-observation.md` |

All three are markdown files committed to your GitHub portfolio repo under `labs/week-10/`. Each lab has its own submission template in the `/Labs/` folder — use it.

> **Note on lab order:** Lab 01 and Lab 02 are sequential. You must complete Lab 01 — specifically, creating the CVI-WebServer template — before Lab 02 will work. Lab 03 is an observation report based on the live session demonstration. If you missed the session, the recording and the posted ceremony command sequence are sufficient to complete Part B.

---

## Checklist

**Pre-lab (both Lab 01 and Lab 02):**
- [ ] Start DC01 first and wait for Server Manager to fully load before starting PKI-SRV01
- [ ] Confirm CertSvc is running: `Get-Service CertSvc`
- [ ] Confirm CA is accepting connections: `certutil -ping`

**Lab 01 — Template Exploration:**
- [ ] Open certtmpl.msc and locate the User, Computer, and Web Server templates
- [ ] Document key settings for all three templates: General, Request Handling, Subject Name, Security tabs
- [ ] Write the comparison response — explain why the differences between templates exist, not just what they are
- [ ] Duplicate the Web Server template — name it CVI-WebServer
- [ ] Modify validity period, subject name handling, and key size settings as directed; document each change with a reason
- [ ] Publish CVI-WebServer to CVI Issuing CA 1 in certsrv.msc
- [ ] Run `certutil -template CVI-WebServer` and paste full output in a code block
- [ ] Extract key fields from certutil output into the table
- [ ] Commit `lab-01-template-exploration.md` to your portfolio repo

**Lab 02 — First Issuance:**
- [ ] Confirm CVI-WebServer appears in the Certificate Templates node of certsrv.msc
- [ ] Open MMC with the Certificates snap-in and request a certificate using the CVI-WebServer template
- [ ] Document the enrollment wizard steps: policy endpoint, templates visible, subject name entered
- [ ] Locate the issued certificate in certsrv.msc → Issued Certificates node and record the Request ID
- [ ] Verify the certificate from the command line: `certutil -store My` (or `-store -user My`)
- [ ] Record the General tab and Details tab fields from the certificate viewer
- [ ] Write Part D — the issuance workflow in your own words
- [ ] Commit `lab-02-first-issuance.md` to your portfolio repo

**Lab 03 — HSM Observation:**
- [ ] Attend the live session or watch the recording
- [ ] Document each ceremony step in Part B: command run, what it did, output observed
- [ ] Complete the SoftHSM2 vs. Thales Luna comparison table in Part C
- [ ] Write Part D — explain what a key ceremony is and why it exists
- [ ] Write Part E — connect software vs. HSM key risk to Phase 2 operations
- [ ] Commit `lab-03-hsm-observation.md` to your portfolio repo
- [ ] Commit all three labs with a meaningful message

---

## What "Good" Looks Like

- Lab 01 Part A does not just list template settings — the comparison response explains *why* the differences exist. For the Web Server template, "Supplied in the request" means the CA does not have the information Active Directory would normally provide; the student's write-up explains why that is the case for server certificates.
- The CVI-WebServer template settings reflect deliberate choices — each change documented with a reason tied to its purpose, not just "I changed it to match the lab instructions"
- The `certutil -template CVI-WebServer` output is pasted in a code block and interpreted — key fields are extracted and each one is identified by name, not just copied from the raw output
- Lab 02 Part D narrates the full pipeline — what publishing the template did, what the enrollment wizard sent to the CA, what the CA evaluated, and where the certificate ended up — in the student's own words, not paraphrased from the lesson
- Lab 03 Part B documents what was actually observed during the ceremony, not just the command sequence posted after the session
- Lab 03 Part D and Part E are analytical — they explain the *why*, not just the what: why ceremonies exist, what risk hardware protects against that software cannot, and how that connects to the CA private key the student worked with this week

---

## Connecting to Prior Weeks

Week 10 is where Phase 1 vocabulary becomes Phase 2 operational choices:

- **Week 9** — Environment orientation: the CA hierarchy, the certsrv.msc console, and the pki.admin account are the same environment you documented last week. Lab 01 and Lab 02 happen inside that environment — if your Week 9 documentation is accurate, it will answer most environment questions before you have to ask them
- **Week 3** — Certificate anatomy: every field in the template maps directly to a field in the issued certificate. The Key Usage and Extended Key Usage extensions you dissected in Week 3 are the same extensions you configure in the template's Extensions tab this week
- **Week 5** — Lifecycle: the validity period in CA Properties and in the template are lifecycle decisions — the same thinking from Week 5's renewal, replacement, and expiry discussion applied at the CA configuration level
- **Week 6** — Trust decisions: the issuance policy settings in CA Properties reflect the CA's answer to the question of how much it validates before issuing — the same trust evaluation framework from Week 6, now expressed as configuration

---

*CVI PKI Career Pathway — Phase 2 Operations*
