# 00 – Getting Started

Welcome to the PKI Foundations Labs repository.

This folder explains how to work within the CVI GitHub environment.

---

## 1. What You Need

- A GitHub account
- Access to the CVI repositories
- Basic comfort navigating GitHub (no advanced Git required)

---

## 2. How This Cohort Uses GitHub

GitHub is used as:

- A lab instruction platform
- A documentation workspace
- A professional portfolio builder

Students will:
1. Follow lab instructions from this repository
2. Complete labs locally or conceptually
3. Document results in their personal portfolio repository
4. Commit their work weekly

---

## 3. Student Workflow Overview

Each week:

1. Review the lesson notes
2. Complete the lab exercise
3. Document findings in your portfolio repository
4. Commit changes with a clear message

Example commit message:

Add Week 1 digital trust reflection

---

4. Portfolio Repository

Each student will generate a personal portfolio repository from:

cvi-student-portfolio-template

This repository is where all of your work lives.

You will not submit work in the curriculum repository.

Instead:

Labs live in the labs/ folder

Notes live in the notes/ folder

Reflections live in the reflections/ folder

Projects live in the projects/ folder

Screenshots and supporting files live in the assets/ folder

For a complete breakdown of the portfolio structure, refer to:

👉 The README.md file inside your portfolio repository.

That file is your structural guide for where all work should be documented.
---

## 5. Embedding Screenshots (Use Relative Paths Only)

When documenting labs, you will often include screenshots of terminal output, certificate details, or command results.

Follow this standard:

### Step 1 — Store Screenshots Properly

All screenshots must be stored in:

#### assets/screenshots/week-01/

(Adjust the week number accordingly.)

Do not store screenshots in the root directory.

### Step 2 — Embed Using a Relative Path

When embedding an image inside a Markdown file (for example inside labs/week-01/key-pair-generation.md), use a relative path.

Example:

![Key Pair Output](../../assets/screenshots/week-01/keypair-output.png)

Explanation:
- .. moves up one folder level
- From labs/week-01/ you move up twice to reach the repository root
- Then navigate into assets/screenshots/week-01/

#### This is called a relative file path.

## ❌ Do NOT Use:

Absolute file paths from your computer
- (C:\Users\YourName\Desktop\image.png)
- Drag-and-drop links that reference your local machine
- Public URL links unless explicitly instructed

### Why Relative Paths Matter
Using relative paths ensures:
- Your repository renders correctly for others
- Reviewers can view images without errors
- Your documentation follows professional Git standards

If your image does not render, your path is incorrect.
Debug the path before submitting.

## 6. Professional Standards

This program models real-world technical environments.

Expectations:
- Clear documentation
- Structured thinking
- Meaningful commit messages
- Professional communication

---

CyberVisionaries Institute  
PKI Career Pathway – Foundations Phase

