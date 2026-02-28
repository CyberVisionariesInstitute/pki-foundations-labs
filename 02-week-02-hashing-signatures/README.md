# Week 2 — Cryptography Fundamentals Labs

#### Focus: Confidentiality, Integrity, and Authenticity

This week moves from theory to application. You will work directly with the cryptographic primitives that power PKI and secure communications.

By completing these labs, you will demonstrate practical understanding of:
 - #### Encryption → Confidentiality
 - #### Hashing → Integrity
 - #### Digital Signatures → Integrity + Authenticity


## Lab Overview

| Lab     | Topic                   | Security Property          |
|---------|-------------------------|----------------------------|
| Lab 01  | Symmetric Encryption    | Confidentiality            |
| Lab 02  | Hashing & Integrity     | Integrity                  |
| Lab 03  | Digital Signatures      | Integrity + Authenticity   |




## Lab Files
Complete each lab in order:
 - Lab 01 — Encrypt a File Symmetrically
 - Lab 02 — Create a Hash of a File
 - Lab 03 — Digitally Sign and Verify a File



## Submission Instructions
1. Create a folder under:
       submissions/<your-name>/

2. Organize your artifacts as follows:
     submissions/<your-name>/
        encrypted/
        hashes/
        signatures/

3. Commit required artifacts:
    - Encrypted file(s)
    - Hash output files
    - Signature file(s)
    - Public key (if applicable)
    - Short explanations in your README

4. #### Do NOT commit private keys.
5. Submit your work via Pull Request.



## Completion Criteria
You are considered complete when:
 - All required artifacts are present
 - Verification commands succeed
 - You can clearly explain:
    - What encryption protects
    - Why a hash changes when data changes
    - Why a digital signature fails after tampering
  


## Why This Matters
Everything in PKI relies on these primitives:
 - Certificates are digitally signed
 - TLS relies on symmetric encryption
 - Trust validation depends on hashing

Week 3 will build directly on this foundation with an X.509 deep dive.

Master these fundamentals before moving forward.
