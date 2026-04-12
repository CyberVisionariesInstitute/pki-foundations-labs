# Week 7 Submissions — Quick Reference

This folder contains your completed lab write-ups and any artifacts generated during Week 7 labs.

---

## Folder Structure

```
labs/
  week-07/
    submissions/
      enterprise-analysis/
        enterprise_cert.pem                  ← Lab 01 (retrieved certificate)
        lab-01-enterprise-certificate-analysis.md  ← Lab 01
      pki-toolkit/
        lab-02-pki-toolkit.md               ← Lab 02 (Stretch)
```

---

## Templates

| Lab | Template |
|---|---|
| Lab 01 | `_templates/lab-submission-template.md` (standard template — not the W6 incident summary template) |
| Lab 02 (Stretch) | No template — see lab guide for document structure |

Copy the Lab 01 template into `enterprise-analysis/` and rename it before you start writing.

Lab 02 uses its own Markdown structure described in the lab guide. Start the document from scratch using the format specified in `lab-02-pki-toolkit.md`.

---

## File Naming Convention

Lab write-ups must follow this format exactly:

```
lab-01-enterprise-certificate-analysis.md
lab-02-pki-toolkit.md
```

Incorrect names will not be recognized by the lab tracker.

---

## Artifact Rules

- Commit the `enterprise_cert.pem` file from Lab 01 alongside your write-up
- Do not commit private key files — add `*.key` and `*_key.pem` to your `.gitignore`
- Lab 02 is a Markdown document only — no certificate files needed

---

## A Note on Lab 02

Lab 02 — Build Your PKI Toolkit — is a stretch lab, but it is one of the most useful artifacts you will produce in Phase 1.

The document should cover every tool you used across all seven weeks of labs. The entries should use real commands from your actual lab history — not reconstructed from documentation. The descriptions should be written in your own language.

This is a career artifact. It lives in your portfolio. It should read like something you would be comfortable showing to a hiring manager or referencing in a technical interview.

---

## Submitting

1. Push all files to GitHub
2. Log in at [cvi-lab-tracker.lovable.app](https://cvi-lab-tracker.lovable.app)
3. Go to **Week 7** and submit each lab individually
4. Confirm the file path matches exactly before clicking Submit

> Pushing to GitHub alone does not mark the lab as submitted. You must click Submit in the tracker.

---

*CVI PKI Career Pathway — Foundations Phase*
