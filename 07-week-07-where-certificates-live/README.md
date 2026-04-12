# Week 7 — PKI in Enterprise Environments & Your Career Path

## Focus

This week you zoom out.

Weeks 1 through 6 built the technical foundation: how certificates work, how to read them, how to manage their lifecycle, and how to diagnose failures when they occur. Week 7 places all of that inside the environment where it actually runs.

You will map where certificates live in enterprise infrastructure — seven distinct layers, from web servers to code signing. You will understand the architectural decisions that determine who issues those certificates: public CAs, internal CAs, and cloud-native services. You will work through the lifecycle problem created when TLS terminates at a load balancer or CDN rather than at the application server. And in the final lesson, you will revisit the PKI career landscape — something you first saw in Week 1, before any of it made sense.

Metro General Hospital continues as the through-line. The PKI engineer from Week 6 is now involved in the organization's cloud migration — not just responding to incidents, but advising on architecture.

Week 8 is the Phase 1 Reflection Project. No new lessons. Your portfolio is the deliverable.

---

## Outcomes

By the end of this week, you can:

- Map the seven common enterprise certificate deployment layers and identify the ownership and lifecycle risks at each
- Explain when to use a public CA, an internal CA, and a cloud-native certificate service — and why most enterprises use all three
- Describe TLS termination and the ownership gap it creates, and explain how the diagnostic framework's Step 1 catches this failure
- Connect Phase 1 skills to specific PKI-adjacent career roles and job requirements
- Articulate what your Phase 1 GitHub portfolio demonstrates as a career artifact

---

## The PKI Enterprise Map

Before starting, internalize where certificates appear in enterprise infrastructure. You will see all seven layers in this week's lessons.

| Layer | Common CA Type | Typical Owner | Key Risk |
|---|---|---|---|
| Web Servers | Public CA | Application team | Ownership ambiguity |
| Load Balancers | Public or Internal | Networking / Infra | Not in CLM, no alert |
| APIs / Microservices | Internal CA | Platform / DevOps | Programmatic, untracked |
| Email (S/MIME) | Internal or Public | IT / End user | Per-user lifecycle at scale |
| VPN / Remote Access | Internal CA | IT / Security | Unrevoked departed-staff certs |
| Device Identity | Internal CA | Endpoint / IT | Trust store deployment gaps |
| Code Signing | Internal CA | DevOps / Security | Expired cert still in use |

---

## What You Will Submit

| Deliverable | Location |
|---|---|
| Lab write-ups | `labs/week-07/submissions/` |
| Lesson notes | `notes/week-07-key-concepts.md` |
| Reflection | `reflections/week-07.md` |

Before starting, copy the following templates into your portfolio repo:

- `_templates/lab-submission-template.md` — use this for Lab 01
- `_templates/reflection-template.md`
- `_templates/lesson-notes-template.md`

> Lab 02 (Stretch) uses its own document structure — see the lab guide for format details. There is no separate template to copy.

---

## Checklist

- [ ] Review the seven enterprise certificate layers and identify who owns each one in a typical organization
- [ ] Lab 01 — Choose a well-known enterprise site and retrieve the live certificate
- [ ] Lab 01 — Parse certificate fields, analyze the chain, and determine where TLS terminates
- [ ] Lab 01 — Run SSL Labs analysis and review CT logs for the domain
- [ ] Lab 01 — Produce a structured Certificate Profile covering all six analysis sections
- [ ] Lab 02 (Stretch) — Inventory every tool used across Phase 1 labs
- [ ] Lab 02 (Stretch) — Write one complete entry per tool: description, use case, real example, source week
- [ ] Lab 02 (Stretch) — Commit `lab-02-pki-toolkit.md` to your portfolio
- [ ] Complete lesson notes
- [ ] Write reflection for Week 7
- [ ] Submit all labs in the CVI Lab Tracker
- [ ] Commit with a meaningful message

---

## What "Good" Looks Like

- Lab 01 Certificate Profile identifies the CA type correctly — public CA vs internal CA — and explains the evidence
- Termination analysis identifies whether TLS terminates at the app server, a load balancer, or a CDN, and names the specific indicator (server headers, issuer, SAN pattern)
- SSL Labs grade is noted and any deprecated TLS version support is flagged
- CT log analysis goes beyond "I found certificates" — it describes what the pattern of issuers and validity periods suggests about the organization's certificate strategy
- Architecture assessment (2–3 sentences) reads like a PKI engineer's observation — specific, evidence-based, not generic
- Lab 02 entries are written in your own language, use real commands from your lab history, and cover every tool used across all seven weeks

---

## Connecting to Prior Weeks

Week 7 is the lens that makes Phase 1 coherent:

- **Week 3** — Certificate anatomy: every field you read in Lab 01 is a Week 3 concept applied in a new context
- **Week 4** — Formats: the PEM output you capture in Lab 01 is Week 4 in action
- **Week 5** — Lifecycle: the load balancer expiration scenario in Lesson 3 is a Week 5 lifecycle failure at an infrastructure layer you had not yet mapped
- **Week 6** — Diagnostic framework: Step 1 of the framework catches TLS termination mismatches — Week 7 is the architectural context for why that step exists

---

*CVI PKI Career Pathway — Foundations Phase*
