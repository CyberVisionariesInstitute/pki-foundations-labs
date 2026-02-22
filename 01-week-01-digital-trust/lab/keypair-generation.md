# Lab — Generate a Key Pair

## Goal
Generate a cryptographic key pair and identify:
- Which key is public
- Which key is private
- Why the private key must never be shared

This lab reinforces the identity model introduced in Lesson 3.
---

## Part 1 — Generate the Key Pair

Option A — OpenSSL (Recommended)

Run:

openssl genrsa -out private.key 2048
openssl rsa -in private.key -pubout -out public.key

You should now have:
- private.key
- public.key

Option B — Browser-based Generator
If OpenSSL is not installed, use a browser-based RSA key generator.

Generate a 2048-bit key pair.

---

## Part 2 — Identify the Keys

Answer:
1. Which file is the public key?
2. Which file is the private key?
3. What would happen if someone obtained your private key?

---

## Part 3 — Reflection

Explain in 3–5 sentences:

Why must the private key remain secret in a PKI system?

Focus on identity and impersonation.

---

## Submission (Portfolio Repo)

Create:

- labs/week-01/keypair-generation.md

Include:

- Screenshots 
- Notes
- Short explanation of outcome

