# Lab 03 (Stretch) — Simulate a Certificate Expiration Scenario

## Goal

This lab builds hands-on understanding of what certificate expiration looks like in practice — how to detect it, how to confirm it, and how to execute the replacement workflow. Certificate expiration is 100% predictable and still takes down major organizations regularly. This lab is about making sure you are not one of those engineers.

You will:

- Create a short-lived certificate and read its validity window
- Create an already-expired certificate to simulate a production expiration event
- Use `openssl x509 -checkend` to programmatically detect expiration
- Observe what an expired certificate looks like when validated
- Practice the full replacement workflow: generate a new key, new CSR, new certificate

---

## Prerequisites

- OpenSSL installed
- Access to a local terminal (Mac Terminal, Git Bash, or WSL)
- Lab 01 completed — you know how to generate a key, CSR, and self-signed certificate
- Your Week 5 folder already exists in your portfolio repo at `labs/week-05/`

> **Note:** All commands must be run locally in your terminal.
> GitHub's web interface cannot execute OpenSSL commands.

---

## Part 1 — Create a Short-Lived Certificate

### Step 1 — Create your artifact directory

```bash
mkdir -p labs/week-05/submissions/expiration-stretch
```

### Step 2 — Generate a key and CSR

```bash
openssl genrsa \
  -out labs/week-05/submissions/expiration-stretch/test_key.pem 2048

openssl req -new \
  -key labs/week-05/submissions/expiration-stretch/test_key.pem \
  -out labs/week-05/submissions/expiration-stretch/test_csr.pem \
  -subj "/CN=shortlived.cvi.internal/O=CyberVisionaries Institute/C=US"
```

### Step 3 — Issue a certificate with a short validity window

```bash
openssl x509 -req \
  -in labs/week-05/submissions/expiration-stretch/test_csr.pem \
  -signkey labs/week-05/submissions/expiration-stretch/test_key.pem \
  -out labs/week-05/submissions/expiration-stretch/test_cert_short.pem \
  -days 1 \
  -set_serial 01
```

### Step 4 — Read the validity window

```bash
openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_short.pem \
  -dates -noout
```

You will see output similar to:

```
notBefore=Fri Mar 27 00:00:00 2026 GMT
notAfter=Sat Mar 28 00:00:00 2026 GMT
```

Record: How long is this certificate valid? What would happen to any system trusting this certificate after the `notAfter` date?

---

### Step 5 — Check whether the certificate is still valid

The `openssl x509 -checkend` command checks whether a certificate will expire within a given number of seconds. This is the command used in automated monitoring scripts.

```bash
# Check if the cert expires within the next 3600 seconds (1 hour)
openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_short.pem \
  -checkend 3600

# Check if the cert expires within the next 86400 seconds (1 day)
openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_short.pem \
  -checkend 86400
```

The command returns:
- `Certificate will not expire` (exit code 0) — still valid within the window
- `Certificate will expire` (exit code 1) — expiring within the window

Record: What output do you see for each check? How would you use this command in a monitoring script to alert on certificates expiring within 30 days?

---

## Part 2 — Simulate an Already-Expired Certificate

In production, you will encounter certificates that have already expired. This section creates one so you know what to expect.

### Step 6 — Create a certificate with a past validity window

```bash
openssl x509 -req \
  -in labs/week-05/submissions/expiration-stretch/test_csr.pem \
  -signkey labs/week-05/submissions/expiration-stretch/test_key.pem \
  -out labs/week-05/submissions/expiration-stretch/test_cert_expired.pem \
  -set_serial 02 \
  -startdate 20230101000000Z \
  -enddate 20230102000000Z
```

> This creates a certificate with a validity window of January 1–2, 2023 — definitively in the past.

### Step 7 — Verify the expired certificate's dates

```bash
openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_expired.pem \
  -dates -noout
```

### Step 8 — Confirm the certificate is detected as expired

```bash
# Check if expired (0 seconds from now means "right now")
openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_expired.pem \
  -checkend 0
```

You should see: `Certificate will expire`

### Step 9 — Attempt to verify the expired certificate

```bash
openssl verify \
  -untrusted labs/week-05/submissions/expiration-stretch/test_cert_expired.pem \
  labs/week-05/submissions/expiration-stretch/test_cert_expired.pem
```

Record: What error message does OpenSSL return for an expired certificate? How does this error map to what a browser or server would display to an end user?

---

## Part 3 — Execute the Replacement Workflow

When a certificate expires (or is about to), the correct response is **replacement** — not renewal with the same key. This section walks through the full replacement workflow.

### Step 10 — Generate a new private key

```bash
openssl genrsa \
  -out labs/week-05/submissions/expiration-stretch/replacement_key.pem 2048
```

> **Why a new key?** Renewal reuses the existing key pair. Replacement generates a new key pair — recommended for security hygiene and required after any potential key compromise.

### Step 11 — Generate a new CSR with the new key

```bash
openssl req -new \
  -key labs/week-05/submissions/expiration-stretch/replacement_key.pem \
  -out labs/week-05/submissions/expiration-stretch/replacement_csr.pem \
  -subj "/CN=shortlived.cvi.internal/O=CyberVisionaries Institute/C=US"
```

### Step 12 — Issue the replacement certificate

```bash
openssl x509 -req \
  -in labs/week-05/submissions/expiration-stretch/replacement_csr.pem \
  -signkey labs/week-05/submissions/expiration-stretch/replacement_key.pem \
  -out labs/week-05/submissions/expiration-stretch/test_cert_replacement.pem \
  -days 365 \
  -set_serial 03
```

### Step 13 — Confirm the replacement certificate is valid

```bash
openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_replacement.pem \
  -dates -noout

openssl x509 \
  -in labs/week-05/submissions/expiration-stretch/test_cert_replacement.pem \
  -checkend 0
```

You should see: `Certificate will not expire`

Record: Compare the `notAfter` dates of `test_cert_expired.pem` and `test_cert_replacement.pem`. Document the full replacement workflow you just completed — what changed and why.

---

## Submission

### What you need to submit

| File | Description |
|---|---|
| `test_cert_short.pem` | Short-lived certificate (1-day validity) |
| `test_cert_expired.pem` | Certificate with a past validity window |
| `test_cert_replacement.pem` | Replacement certificate issued with a new key |
| `lab-03-expiration-stretch.md` | Your completed observations |

> Do NOT commit `test_key.pem` or `replacement_key.pem` — add `*_key.pem` to your `.gitignore`.

---

### Step 1 — Confirm your artifact directory exists

```bash
ls labs/week-05/submissions/expiration-stretch/
```

Your folder should contain the four files listed above (plus key files locally, which should not be committed).

---

### Step 2 — Complete your observations file

Open `labs/week-05/submissions/expiration-stretch/lab-03-expiration-stretch.md` and fill in the template below:

```markdown
# Lab 03 — Simulate a Certificate Expiration Scenario

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept or system behavior were you investigating?

---

## Environment
- Operating System:
- Terminal Used:
- OpenSSL Version (`openssl version`):

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

- What did the `notBefore` and `notAfter` dates look like for your short-lived certificate?
- What output did `openssl x509 -checkend` produce for each certificate?
- What error message did `openssl verify` return for the expired certificate?
- What changed in the replacement certificate compared to the expired one?

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

- Why does certificate expiration still cause outages despite being 100% predictable?
- What is the difference between renewal and replacement, and when would you choose each?
- How would you use `openssl x509 -checkend` in a real monitoring script?
- What does "certificate inventory" mean, and why is it necessary at enterprise scale?

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

---

## Artifacts
List the files generated during this lab.

- test_cert_short.pem
- test_cert_expired.pem
- test_cert_replacement.pem
```

---

### Step 3 — Commit and push to GitHub

**Option A — Terminal (Git CLI)**

```bash
git add labs/week-05/submissions/expiration-stretch/
git commit -m "Week 5 Lab 03 — Simulate a Certificate Expiration Scenario (Stretch)"
git push
```

**Option B — GitHub Web (Upload via Browser)**

1. Go to your repo on [github.com](https://github.com)
2. Navigate to `labs/week-05/submissions/expiration-stretch/`
3. Click **Add file → Upload files**
4. Drag and drop your files into the upload area
5. Scroll down to **Commit changes**, enter `Week 5 Lab 03 — Simulate a Certificate Expiration Scenario (Stretch)`, and click **Commit changes**

> **Don't see the folder yet?** Click **Add file → Create new file** and type the full path in the filename field: `labs/week-05/submissions/expiration-stretch/lab-03-expiration-stretch.md`. This creates the folder and file at the same time.

---

### Step 4 — Submit in the CVI Lab Tracker

1. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
2. Go to **Week 5 → Lab 03: Simulate a Certificate Expiration Scenario**
3. Click **Copy** next to the file path — confirm your file exists at that exact location in your repo
4. Click **Submit**

> Your submission is not complete until you click Submit in the tracker. Pushing to GitHub alone does not mark the lab as submitted.

---

*CVI PKI Career Pathway — Foundations Phase*
