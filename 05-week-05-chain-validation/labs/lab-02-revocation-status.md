# Lab 02 — Check Certificate Revocation Status with OCSP

## Goal

This lab builds operational understanding of how certificate revocation works in practice — specifically how OCSP (Online Certificate Status Protocol) allows systems to check whether a certificate is still valid before trusting it.

You will:

- Retrieve a live certificate from a production website
- Locate the OCSP responder URL inside the certificate's extensions
- Retrieve the issuer certificate needed to perform an OCSP query
- Query the OCSP responder using OpenSSL and interpret the response
- Locate the CRL Distribution Point and understand how CRL checking differs from OCSP

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Your Week 5 folder already exists in your portfolio repo at `labs/week-05/`
- Week 3 Lab 02 completed — you inspected certificate extensions including OCSP and CRL fields

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Retrieve the Certificate Chain

### Step 1 — Create your artifact directory

```bash
mkdir -p labs/week-05/submissions/revocation-status
```

### Step 2 — Connect to a live site and retrieve the leaf certificate

```bash
echo | openssl s_client -connect github.com:443 2>/dev/null | \
  openssl x509 \
  -out labs/week-05/submissions/revocation-status/leaf_cert.pem
```

Confirm it saved correctly:

```bash
openssl x509 \
  -in labs/week-05/submissions/revocation-status/leaf_cert.pem \
  -subject -issuer -dates -noout
```

Record: What is the Subject (issued to) and what is the Issuer (signed by)?

---

### Step 3 — Retrieve the full certificate chain

To perform an OCSP query, you need both the leaf certificate and its issuer certificate. Retrieve the full chain from the server:

```bash
echo | openssl s_client -connect github.com:443 -showcerts 2>/dev/null \
  > labs/week-05/submissions/revocation-status/fullchain.txt
```

Open `fullchain.txt` in a text editor. You will see two or more certificate blocks. The **first block** is the leaf certificate (already saved). The **second block** is the intermediate CA — this is your issuer certificate.

Copy the second `-----BEGIN CERTIFICATE-----` through `-----END CERTIFICATE-----` block and save it as:

```
labs/week-05/submissions/revocation-status/issuer_cert.pem
```

Verify it is a CA certificate:

```bash
openssl x509 \
  -in labs/week-05/submissions/revocation-status/issuer_cert.pem \
  -subject -issuer -noout
```

Record: What does it mean that the Subject and Issuer of `issuer_cert.pem` are different from the leaf cert?

> **Note:** You can clean up `fullchain.txt` after extracting the issuer — it does not need to be committed.

---

## Part 2 — Locate Revocation Information in the Certificate

### Step 4 — Find the OCSP responder URL

Inspect the leaf certificate's extensions for the OCSP URL:

```bash
openssl x509 \
  -in labs/week-05/submissions/revocation-status/leaf_cert.pem \
  -text -noout | grep -A4 "Authority Information Access"
```

You should see output similar to:

```
Authority Information Access:
  OCSP - URI:http://ocsp.example.com
  CA Issuers - URI:http://certs.example.com/issuer.crt
```

Record: What is the OCSP responder URL in this certificate?

---

### Step 5 — Find the CRL Distribution Point

```bash
openssl x509 -in labs/week-05/submissions/revocation-status/leaf_cert.pem -text -noout
```

Record: What is the CRL Distribution Point URL?

> **Connect to Week 3:** You first saw this extension in Week 3 Lab 02 when you inspected certificate extensions. Now you are using it operationally.

---

## Part 3 — Query the OCSP Responder

### Step 6 — Set the OCSP URL as a variable

Extract the OCSP URL from the certificate automatically:

```bash
OCSP_URL=$(openssl x509 \
  -in labs/week-05/submissions/revocation-status/leaf_cert.pem \
  -text -noout | grep "OCSP" | grep "http" | sed 's/.*URI://' | tr -d ' ')

echo "OCSP URL: $OCSP_URL"
```

---

### Step 7 — Query the OCSP responder

```bash
openssl ocsp \
  -issuer labs/week-05/submissions/revocation-status/issuer_cert.pem \
  -cert labs/week-05/submissions/revocation-status/leaf_cert.pem \
  -url "$OCSP_URL" \
  -resp_text \
  -noverify \
  2>/dev/null | tee labs/week-05/submissions/revocation-status/ocsp_response.txt
```

> **If this command hangs or times out**, the OCSP responder may be temporarily unavailable or blocked on your network. Try substituting `google.com` for `github.com` in Steps 2–3 and re-run. Alternatively, you can document the expected output format from Step 8 below and note that network access was unavailable.

---

### Step 8 — Interpret the OCSP response

Look for these key fields in `ocsp_response.txt`:

```
Response verify OK
leaf_cert.pem: good
        This Update: ...
        Next Update: ...
```

| Response Status | Meaning |
|---|---|
| `good` | Certificate is valid and has not been revoked |
| `revoked` | Certificate has been revoked — do not trust it |
| `unknown` | The OCSP responder does not have status information for this certificate |

Record:
- What is the status of the certificate you queried?
- What do `This Update` and `Next Update` tell you about OCSP response caching?
- Why does the query require the issuer certificate in addition to the leaf certificate?

---

## Part 4 — Compare OCSP and CRL

### Step 9 — Document the difference

Based on what you observed in this lab, record your answers to these questions in your write-up:

- OCSP gives you a **real-time status** for a specific certificate. CRL is a **list of all revoked certificates** published by the CA on a schedule. Which approach is better for high-traffic systems, and why?
- If an OCSP responder is unavailable, what do most systems do? What are the security implications of "soft fail" vs "hard fail"?
- Why might an organization choose to run its own OCSP responder instead of relying on the public CA's?

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `leaf_cert.pem` | Leaf certificate retrieved from the live site |
| `issuer_cert.pem` | Intermediate CA certificate (issuer of the leaf cert) |
| `ocsp_response.txt` | Captured OCSP query output |
| `lab-02-revocation-status.md` | Your completed observations |

---

### Step 1 — Confirm your artifact directory exists

```bash
ls labs/week-05/submissions/revocation-status/
```

Your folder should contain the four files listed above.

---

### Step 2 — Complete your observations file

Open `labs/week-05/submissions/revocation-status/lab-02-revocation-status.md` and fill in the template below:

```markdown
# Lab 02 — Check Certificate Revocation Status with OCSP

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept or system behavior were you investigating?

---

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (`openssl version`):
- Target site used:

---

## Steps Performed
Summarize the key steps you performed to complete the lab.
Do not copy the lab instructions — describe what you actually did.

1.
2.
3.
4.
5.

---

## Results
Include the important outputs or findings from the lab.

- What was the Subject and Issuer of the leaf certificate?
- What OCSP URL did you find in the Authority Information Access extension?
- What CRL Distribution Point URL did you find?
- What was the OCSP response status for the certificate?
- What do "This Update" and "Next Update" tell you?

If you include screenshots, store them in the assets folder and reference them here:
![Description](../../assets/screenshots/week-05/your-screenshot.png)

---

## Key Findings
Document the most important observations from the lab.

•
•
•

---

## Explanation
Explain why the results matter.

- Why does an OCSP query require both the leaf certificate and the issuer certificate?
- What is the difference between OCSP and CRL in practice?
- What would happen if a system trusted a revoked certificate because OCSP was unavailable?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

---

## Artifacts
List the files generated during this lab.

- leaf_cert.pem
- issuer_cert.pem
- ocsp_response.txt
```

---

### Step 3 — Commit and push to GitHub

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-05/submissions/revocation-status/
git commit -m "Week 5 Lab 02 — Check Certificate Revocation Status with OCSP"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-05/submissions/revocation-status/`
3. Click **Add file → Upload files**
4. Drag and drop your files into the upload area
5. Scroll down to **Commit changes**, enter `Week 5 Lab 02 — Check Certificate Revocation Status with OCSP`, and click **Commit changes**

> **Don't see the folder yet?** Click **Add file → Create new file** and type the full path in the filename field: `labs/week-05/submissions/revocation-status/lab-02-revocation-status.md`. This creates the folder and file at the same time.

---

### Step 4 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 5 → Lab 02: Check Certificate Revocation Status with OCSP**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

*CVI PKI Career Pathway — Foundations Phase*
