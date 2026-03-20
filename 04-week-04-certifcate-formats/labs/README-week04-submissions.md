# Week 4 Submissions — Quick Reference

This folder contains your completed lab write-ups and cryptographic artifacts for Week 4.

---

## Folder Structure

```
labs/
  week-04/
    submissions/
      convert-formats/
        leaf_cert.pem                      ← Lab 01
        leaf_cert.der                      ← Lab 01
        leaf_cert_restored.pem             ← Lab 01
        test_cert.pem                      ← Lab 01
        test_bundle.pfx                    ← Lab 01
        lab-01-convert-certificate-formats.md  ← Lab 01
      trust-store-inspection/
        root_cas.pem                       ← Lab 02 (macOS only)
        lab-02-inspect-trust-stores.md     ← Lab 02
      install-validate/                    ← Lab 03 (Stretch, optional)
        test-root-ca.crt                   ← Lab 03
        test-signed.crt                    ← Lab 03
        lab-03-install-and-validate.md     ← Lab 03
```

---

## File Naming Convention

Lab write-ups must follow this format exactly:

```
lab-01-convert-certificate-formats.md
lab-02-inspect-trust-stores.md
lab-03-install-and-validate.md
```

Incorrect names will not be recognized by the lab tracker.

---

## Artifact Rules

- Commit actual certificate files alongside your write-ups
- Use the `assets/` folder for screenshots only
- **Never commit private key files** — add `*.key` to your `.gitignore`
- This applies to `test_key.pem` and `test-root-ca.key` generated in Labs 01 and 03

---

## Submitting

1. Push all files to GitHub
2. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
3. Go to **Week 4** and submit each lab individually
4. Confirm the file path matches exactly before clicking Submit

> Pushing to GitHub alone does not mark the lab as submitted. You must click Submit in the tracker.

---

*CVI PKI Career Pathway — Foundations Phase*
