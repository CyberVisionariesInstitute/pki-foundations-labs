# 00 – Getting Started

Welcome to the PKI Foundations Labs repository.

This repository provides the lab instructions used throughout the CVI PKI Career Pathway — Foundations Phase. It also explains how students will use GitHub to document their work and build a professional technical portfolio.

---

## 1. What You Need

Before starting the labs, ensure you have the following:
- A **GitHub account**
- Access to the **CVI lab repositories**
- A **local terminal environment**
    - Mac Terminal
    - Windows PowerShell
    - Git Bash or WSL
- **OpenSSL installed** for cryptography and certificate labs

All technical commands must be run locally on your machine.

**GitHub’s web interface cannot run terminal commands.**

---

## 2. How This Cohort Uses GitHub

GitHub serves three purposes in this program:

### 1. Lab Instruction Platform

This repository contains the **weekly lab instructions.**

### 2. Documentation Workspace

Students document their observations, notes, and reflections.

### 3. Professional Portfolio Builder

By the end of the program, your repository becomes a **demonstration of your PKI and security skills** that can be shared with employers.

---

## 3. Student Workflow Overview

Each week follows the same workflow.

### Step 1 — Review Lesson Material

Read the lesson notes and supporting materials.

---

### Step 2 — Complete the Lab

Follow the lab instructions in the labs/ folder.

All commands should be executed in your **local terminal environment.**

Labs may generate artifacts such as:
- encrypted files
- hash outputs
- certificates
- verification results

These artifacts must be saved in your portfolio repository.

---

### Step 3 — Document Your Work

Record your findings in your portfolio repository:
- **Notes →** notes/
- **Reflections →** reflections/
- **Lab artifacts →** labs/.../submissions/

Use clear explanations in your own words.

---

### Step 4 — Commit Your Work

Your work should be committed to your GitHub repository each week.

Example commit message:

Week 2 — Cryptography Fundamentals labs completed

---

## 4. Portfolio Repository

Each student will generate a personal portfolio repository from:

cvi-student-portfolio-template

This repository is where **all of your work lives.**

You will **not submit work in the curriculum repository.**

Instead:
 - **Labs** live in the labs/ folder (Lab instructions and submissions)
 - **Notes** live in the notes/ folder (Technical notes and explanations)
 - **Reflections**  live in the reflections/ folder (Weekly reflections)
 - **Projects** live in the projects/ folder (Capstone or extended work)
 - **Assets** live in the assests/ folder (Screenshots or supporting visuals)

For a complete breakdown of the portfolio structure, refer to:

👉 The README.md file inside your portfolio repository.

That file is your structural guide for where all work should be documented.

---

## 5. How to Save Your Work to GitHub

After completing a lab, you must **save your work to your GitHub repository** so it becomes part of your portfolio.

Think of Git like a **three-step save process:**

**1. Select the files you want to save**
**2. Record a snapshot of the changes**
**3. Upload that snapshot to GitHub**

You will do this using your terminal.

---

### Step 1 — Go to Your Portfolio Repository
Open your terminal and navigate to your portfolio folder.

Example:

cd pki-portfolio

Your terminal should now be inside your repository.

You can confirm by running:

git status

This command shows any files that have changed.

---

### Step 2 — Select the Files You Want to Save
Next, tell Git which files should be included in your update.

For most labs, you can simply add the entire labs folder.

git add labs/

This prepares the files to be saved.

You can check what will be saved by running:

git status

Files listed under **Changes to be committed** are ready to be recorded.

---

### Step 3 — Create a Snapshot of Your Work
Now create a commit.

A **commit** is a snapshot of your work at that moment.

Example:

git commit -m "Week 3 Lab 01 — Inspect certificate fields"

The message should briefly describe what you completed.

Examples:
- Week 2 — Hashing lab completed
- Week 3 — Certificate inspection lab
- Week 4 — X.509 notes

---

### Step 4 — Upload Your Work to GitHub
Now upload your commit to GitHub.

git push origin main

If your repository uses **master** instead of main:

git push origin master

Your work is now stored on GitHub.

---

### Step 5 — Verify Your Work
Open your GitHub repository in your browser.

Confirm that your new files appear in the correct folder.

Example:

labs/03-week-03-certificate-anatomy/submissions/

You should also see your **commit message** in the repository history.

---

### Example Workflow (What You Will Usually Run)
Most labs will look like this:

git add labs/
git commit -m "Week 3 Lab 01 — Inspect certificate fields"
git push origin main

That's it.

---

### Important Rules
- Always push your work after completing a lab
- Use clear commit messages
- Keep files organized in the correct folders
- **Never upload private keys**

---

## 6. Professional Standards
This program models **real-world technical environments.**

Expectations include:
- Clear documentation
- Structured thinking
- Organized repositories
- Meaningful commit messages
- Professional communication

Treat your repository as a **technical portfolio** that demonstrates your skills.

---

CyberVisionaries Institute  
**PKI Career Pathway – Foundations Phase**

