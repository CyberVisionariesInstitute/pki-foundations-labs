# Week 5 Submissions — Quick Reference

This folder contains your completed lab write-ups and cryptographic artifacts for Week 5.

---

## Folder Structure

```
labs/
  week-05/
    submissions/
      generate-csr/
        test_csr.pem                       ← Lab 01
        test_cert.pem                      ← Lab 01
        lab-01-generate-csr.md             ← Lab 01
      revocation-status/
        leaf_cert.pem                      ← Lab 02
        issuer_cert.pem                    ← Lab 02
        ocsp_response.txt                  ← Lab 02
        lab-02-revocation-status.md        ← Lab 02
      expiration-stretch/                  ← Lab 03 (Stretch, optional)
        test_cert_short.pem                ← Lab 03
        test_cert_expired.pem              ← Lab 03
        test_cert_replacement.pem          ← Lab 03
        lab-03-expiration-stretch.md       ← Lab 03
```

---

## File Naming Convention

Lab write-ups must follow this format exactly:

```
lab-01-generate-csr.md
lab-02-revocation-status.md
lab-03-expiration-stretch.md
```

Incorrect names will not be recognized by the lab tracker.

---

## Artifact Rules

- Commit actual certificate and CSR files alongside your write-ups
- Use the `assets/` folder for screenshots only
- **Never commit private key files** — add `*.key` and `*_key.pem` to your `.gitignore`
- This applies to any `test_key.pem` files generated in these labs

---

## Submitting

1. Push all files to GitHub
2. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
3. Go to **Week 5** and submit each lab individually
4. Confirm the file path matches exactly before clicking Submit

> Pushing to GitHub alone does not mark the lab as submitted. You must click Submit in the tracker.

---

*CVI PKI Career Pathway — Foundations Phase*
