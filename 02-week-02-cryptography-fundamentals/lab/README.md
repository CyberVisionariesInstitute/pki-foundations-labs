# Week 2 — Cryptography Fundamentals Labs

## Phase 1: PKI Foundations

This folder contains the hands-on labs for Week 2.

This week focuses on applying the three core security properties that make digital trust possible:

- **Confidentiality** → Symmetric Encryption  
- **Integrity** → Hashing  
- **Authenticity** → Digital Signatures  

---

## Lab Structure

Complete the labs in order:

1. `lab-01-symmetric-encryption.md`
2. `lab-02-hashing-integrity.md`
3. `lab-03-digital-signatures.md`

Each lab builds on the previous one.

---

## Submission Requirements

Create the following structure under the `submissions/` directory:
submissions/your-name/
encrypted/
hashes/
signatures/

Place your artifacts in the appropriate folders:

### encrypted/
- Encrypted file(s)
- Decrypted output (if required)

### hashes/
- Original hash output
- Tampered hash output

### signatures/
- Public key
- Signature file
- Verification output

**Do NOT commit private keys.**

---

## Completion Criteria

You have successfully completed Week 2 labs when:

- All required artifacts are present
- Signature verification succeeds before tampering
- Signature verification fails after tampering
- You can clearly explain:
  - What encryption protects
  - Why hashes change when data changes
  - Why digital signatures fail after modification

---

## Why This Matters

Everything in PKI relies on these primitives:

- Certificates are digitally signed
- TLS uses symmetric encryption
- Trust validation depends on hashing

Mastering these fundamentals prepares you for the X.509 deep dive in Week 3.
