# Contributing to Nexus

Welcome! This guide will walk you through **everything** you need to contribute to Nexus — from installing the tools on your computer for the first time, all the way to getting your content live on the site.

You do **not** need to know how to code. You only edit JSON files (structured text files). This guide assumes you have never used Git, GitHub, or VS Code before.

---

## Table of Contents

1. [What you will be doing](#1-what-you-will-be-doing)
2. [One-time setup — Install the tools](#2-one-time-setup--install-the-tools)
3. [One-time setup — Configure Git](#3-one-time-setup--configure-git)
4. [One-time setup — Get the code onto your computer](#4-one-time-setup--get-the-code-onto-your-computer)
5. [One-time setup — Open the project in VS Code](#5-one-time-setup--open-the-project-in-vs-code)
6. [Every time — Create your working branch](#6-every-time--create-your-working-branch)
7. [Every time — Add your content](#7-every-time--add-your-content)
8. [Every time — Validate your JSON before submitting](#8-every-time--validate-your-json-before-submitting)
9. [Every time — Submit your work (Pull Request)](#9-every-time--submit-your-work-pull-request)
10. [JSON content rules](#10-json-content-rules)
11. [Domain ownership](#11-domain-ownership)
12. [Playbook guidelines](#12-playbook-guidelines)
13. [Commit message format](#13-commit-message-format)
14. [Need help?](#14-need-help)

---

## 1. What you will be doing

Nexus is a web app that shows structured troubleshooting questions and playbooks for Microsoft support engineers. All the content lives in simple text files called **JSON files** inside the `domains/` folder.

Your job as a tech lead is to **add or improve content** in those JSON files. You never touch the website code itself.

The flow looks like this:

```
Your computer  →  GitHub (staging: ver1.8)  →  GitHub (live site: main)
```

- You write content on your computer
- You send it to GitHub for review (this is called a **Pull Request**)
- The repo owner reviews and publishes it to the live site

---

## 2. One-time setup — Install the tools

You need three tools. Install them in this order.

---

### 2a. Git — version control

Git is the tool that tracks changes to files and lets you send your work to GitHub.

**Windows:**
1. Go to https://git-scm.com/download/win
2. Click the first download link (the `.exe` file)
3. Run the installer — click **Next** on every screen, keeping all defaults
4. When asked about the default editor, select **Use Visual Studio Code** (you will install VS Code next)
5. Click **Install**, then **Finish**

**Mac:**
1. Open **Terminal** (press `Cmd + Space`, type `Terminal`, press Enter)
2. Type `git --version` and press Enter
3. If Git is not installed, a popup will appear asking you to install **Xcode Command Line Tools** — click **Install** and wait for it to finish

**Verify it worked:** Open a terminal and run:
```
git --version
```
You should see something like `git version 2.x.x`.

---

### 2b. VS Code — text editor

VS Code is the editor you will use to write and edit JSON files. It highlights errors as you type.

1. Go to https://code.visualstudio.com
2. Click the big **Download** button for your operating system
3. Run the installer with all defaults
4. When asked, tick **Add to PATH** and **Open with Code** options if shown

**Verify it worked:** Open VS Code from your Start Menu or Applications folder. You should see a welcome screen.

---

### 2c. Node.js — needed for JSON validation

Node.js lets you run the validation tool that checks your JSON before you submit.

1. Go to https://nodejs.org
2. Click the **LTS** (Long Term Support) download button
3. Run the installer with all defaults

**Verify it worked:** Open a terminal and run:
```
node --version
```
You should see something like `v20.x.x`.

---

### 2d. Install the JSON validator

Open a terminal and run this one command:

```bash
npm install -g ajv-cli ajv-formats
```

This installs the same validator that the CI pipeline uses to check your files.

---

## 3. One-time setup — Configure Git

Git needs to know who you are so your contributions are credited to you.

Open a terminal and run these two commands, replacing the placeholders with your own name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@microsoft.com"
```

**Verify it worked:**
```bash
git config --global user.name
git config --global user.email
```
Both should print back what you entered.

---

## 4. One-time setup — Get the code onto your computer

This is called **cloning** the repository. It downloads a full copy of the project to your computer.

1. Open a terminal
2. Navigate to a folder where you want to store the project. For example, to put it on your Desktop:

   **Windows:**
   ```bash
   cd %USERPROFILE%\Desktop
   ```
   **Mac:**
   ```bash
   cd ~/Desktop
   ```

3. Run this command to download the project:
   ```bash
   git clone https://github.com/iwrik00100/nexus.git
   ```

4. Move into the project folder:
   ```bash
   cd nexus
   ```

5. Switch to the staging branch (this is where all contributions go):
   ```bash
   git checkout ver1.8
   ```

**Verify it worked:** Run `ls` (Mac) or `dir` (Windows). You should see folders like `domains/`, `src/`, `schema/`.

> **You only do steps 2–5 once.** After that, the project is on your computer permanently.

---

## 5. One-time setup — Open the project in VS Code

1. Open VS Code
2. Click **File → Open Folder**
3. Navigate to the `nexus` folder you just cloned and click **Open**

You will see the project file tree on the left side. The folders you care about are:

```
nexus/
└── domains/
    ├── networking/
    ├── directory_services/
    ├── performance/
    ├── user_experience/
    ├── device_deployment/
    ├── storage_ha/
    └── collaboration/
```

Each folder has an `_index.json` (the list of technologies) and one `.json` file per technology.

**Recommended VS Code extension:** Install **Prettier - Code formatter** for automatic JSON formatting:
1. Click the Extensions icon on the left sidebar (looks like four squares)
2. Search for `Prettier`
3. Click **Install** on the first result

---

## 6. Every time — Create your working branch

> **Never edit files directly on `ver1.8` or `main`.** Always create a new branch for your work.

A **branch** is your own private copy of the project where you can make changes safely without affecting anyone else.

**Before starting any new piece of work**, open a terminal in the project folder and run:

```bash
# Make sure you have the latest version of ver1.8
git checkout ver1.8
git pull origin ver1.8

# Create your own branch — name it something descriptive
git checkout -b feature/user-experience-logon
```

Branch naming convention:
```
feature/<domain>-<technology>

Examples:
  feature/user-experience-logon
  feature/directory-services-ad-cs
  feature/performance-cpu
```

**Verify it worked:** Run `git branch`. You should see your new branch name with a `*` next to it.

---

## 7. Every time — Add your content

### Step 1 — Create the technology JSON file

In VS Code, right-click the correct domain folder (e.g. `domains/user_experience/`) and select **New File**. Name it `<tech_key>.json` — for example `logon.json`.

Paste this template and fill it in:

```json
{
  "key": "logon",
  "label": "Logon & Authentication",
  "icon": "🔑",
  "domain": "user_experience",
  "scoping": {
    "l1": [
      "Question 1 — at least 10 characters long",
      "Question 2 — at least 10 characters long",
      "Question 3 — at least 10 characters long"
    ],
    "l2": [
      "Advanced question 1 — at least 10 characters long",
      "Advanced question 2 — at least 10 characters long",
      "Advanced question 3 — at least 10 characters long"
    ]
  },
  "probing": {
    "l1": ["Command or question 1", "Command or question 2", "Command or question 3"],
    "l2": ["Advanced command 1", "Advanced command 2", "Advanced command 3"]
  },
  "troubleshooting": {
    "l1": ["Resolution step 1", "Resolution step 2", "Resolution step 3"],
    "l2": ["Advanced fix 1", "Advanced fix 2", "Advanced fix 3"]
  },
  "datacollection": {
    "l1": ["Export log X", "Collect trace Y", "Run command Z"],
    "l2": ["Advanced collection 1", "Advanced collection 2", "Advanced collection 3"]
  },
  "playbooks": {
    "symptom_slug": {
      "title": "Short description of the symptom",
      "severity": "high",
      "phases": [
        {
          "name": "Verify",
          "icon": "✅",
          "steps": [
            { "action": "Command to confirm the problem exists", "expect": "What you should see" }
          ]
        },
        {
          "name": "Isolate",
          "icon": "🔍",
          "steps": [
            { "action": "Command to narrow down the cause", "expect": "What to look for" }
          ]
        },
        {
          "name": "Fix",
          "icon": "🛠",
          "steps": [
            { "action": "Command or action to fix the issue", "expect": "Confirmation the fix worked" }
          ]
        },
        {
          "name": "Confirm",
          "icon": "🎯",
          "steps": [
            { "action": "Command to verify the healthy state", "expect": "What a healthy result looks like" }
          ]
        }
      ]
    }
  }
}
```

**Key rules to remember while filling this in:**

| Field | Rule |
|---|---|
| `key` | Lowercase letters, numbers, underscores only — e.g. `logon`, `ad_ds`, `dot1x_wired` |
| `domain` | Must exactly match the folder name — e.g. `user_experience` |
| `l1` / `l2` arrays | Must have **at least 3 items** each |
| Each question/step | Must be **at least 10 characters** long |
| `severity` | Must be exactly `"high"`, `"medium"`, or `"low"` |

---

### Step 2 — Register the technology in `_index.json`

Open `domains/<your_domain>/_index.json` and add your technology to the `technologies` array:

```json
{
  "key": "user_experience",
  "label": "User Experience",
  "icon": "👤",
  "description": "Logon, User Profiles, Group Policy, App Compatibility, AVD/WVD, RDS",
  "technologies": [
    { "key": "rdp",   "label": "Unable to take RDP",    "icon": "🔑" },
    { "key": "logon", "label": "Logon & Authentication", "icon": "🔐" }
  ]
}
```

> Make sure you add a comma after the previous entry before adding yours.

---

### Step 3 — Update `app.js` tech count and description

Open `src/app.js` and find the two places that reference your domain:

**1. `DOMAINS` array** — update the `description` to include your new technology:
```js
{ key: 'user_experience', label: 'User Experience', icon: '👤', description: 'Logon, RDP / Remote Desktop Services' },
```

**2. `DOMAIN_TECH_COUNTS` map** — update the count:
```js
user_experience: '2 technologies',
```

---

## 8. Every time — Validate your JSON before submitting

Always validate locally before opening a PR. This catches errors before CI does.

Open a terminal in the project root folder and run:

```bash
# Validate your technology file
ajv validate -s schema/technology.schema.json -d domains/user_experience/logon.json --spec=draft7 --strict=false

# Validate the domain index file
ajv validate -s schema/domain-index.schema.json -d domains/user_experience/_index.json --spec=draft7 --strict=false
```

**If it says `valid`** — you are good to go.

**If it shows errors** — read the error message carefully. Common mistakes:

| Error message | What it means | How to fix |
|---|---|---|
| `"key" must match pattern "^[a-z0-9_]+$"` | Your `key` has uppercase letters or spaces | Change to lowercase with underscores |
| `"l1" must NOT have fewer than 3 items` | You have fewer than 3 questions | Add more questions |
| `"string" must NOT have fewer than 10 characters` | A question is too short | Make it longer |
| `"severity" must be equal to one of the allowed values` | Wrong severity value | Use exactly `"high"`, `"medium"`, or `"low"` |
| `Unexpected token` | Invalid JSON syntax | Check for missing commas or quotes |

**Tip — catching JSON syntax errors in VS Code:** VS Code will underline JSON errors in red as you type. Look for red squiggles and hover over them to see what is wrong.

---

## 9. Every time — Submit your work (Pull Request)

A **Pull Request (PR)** is how you send your work to the repo owner for review. Here is the full flow:

### Step 1 — Save your changes in Git

Open a terminal in the project folder and run:

```bash
# See which files you changed
git status

# Stage all your changed files
git add domains/user_experience/logon.json
git add domains/user_experience/_index.json
git add src/app.js

# Save the changes with a descriptive message
git commit -m "feat(user_experience): add Logon and Authentication questions and playbook"
```

### Step 2 — Send your branch to GitHub

```bash
git push origin feature/user-experience-logon
```

The first time you push, Git may ask you to log in to GitHub. Follow the prompts.

### Step 3 — Open the Pull Request on GitHub

1. Go to https://github.com/iwrik00100/nexus in your browser
2. You will see a yellow banner saying **"Your branch had recent pushes"** — click **Compare & pull request**
3. Make sure the **base branch** is set to `ver1.8` (not `main`)
4. Fill in the title and description:
   - **Title:** `feat(user_experience): add Logon and Authentication questions and playbook`
   - **Description:** Briefly describe what you added and why
5. Click **Create pull request**

### Step 4 — Wait for CI checks

GitHub will automatically run validation checks on your PR. You will see a section at the bottom of the PR page showing the check results:

- ✅ Green checkmark = passed
- ❌ Red cross = failed — click **Details** to see what went wrong, fix it on your computer, commit, and push again (the PR updates automatically)

### Step 5 — Get it reviewed and merged

Tag `@iwrik00100` in a comment on the PR. Once approved and all checks pass, the repo owner will merge it into `ver1.8` and then promote it to `main` for the live site.

---

## 10. JSON content rules

CI will **block your PR** if any of these fail:

| Rule | Requirement |
|---|---|
| JSON syntax | Valid JSON — no trailing commas, no comments |
| `key` format | Snake_case only: `a-z`, `0-9`, `_` — e.g. `dns_server`, `dot1x_wired` |
| `domain` field | Must match one of: `networking`, `directory_services`, `performance`, `user_experience`, `device_deployment`, `storage_ha`, `collaboration` |
| Minimum questions | At least **3 items** in every `l1` and `l2` array |
| Question length | Each question/step must be at least **10 characters** |
| Severity | Must be exactly `"high"`, `"medium"`, or `"low"` |
| Playbook phases | At least 1 phase, each phase at least 1 step |

---

## 11. Domain ownership

| Domain | Folder | Owner |
|---|---|---|
| Networking | `domains/networking/` | @iwrik00100 |
| Directory Services | `domains/directory_services/` | TBD |
| Performance | `domains/performance/` | TBD |
| User Experience | `domains/user_experience/` | TBD |
| Device & Deployment | `domains/device_deployment/` | TBD |
| Storage & HA | `domains/storage_ha/` | TBD |
| Collaboration | `domains/collaboration/` | TBD |

Your PR must be approved by your domain owner before it can merge. If your domain is `TBD`, tag `@iwrik00100` for review.

---

## 12. Playbook guidelines

- Always use **4 phases**: Verify → Isolate → Fix → Confirm
- `action` — the exact command or action the engineer should perform
- `expect` — what a healthy or fixed result looks like
- One command per step — keep it focused
- Severity guide:
  - `high` — production is down or severely impacted
  - `medium` — service is degraded, workaround exists
  - `low` — cosmetic or minor impact

---

## 13. Commit message format

Use this format for all commits:

```
feat(domain): short description of what you added
fix(domain): short description of what you fixed
docs: what you updated in documentation
```

Examples:
```
feat(user_experience): add Logon and Authentication questions and playbook
feat(directory_services): add AD CS scoping questions
fix(networking): correct IKEv2 probing command typo
docs: update CONTRIBUTING.md
```

---

## 14. Need help?

- **JSON syntax error you can't figure out** — paste your JSON into https://jsonlint.com — it will highlight exactly where the problem is
- **Git or GitHub question** — open a GitHub Issue with the `question` label at https://github.com/iwrik00100/nexus/issues
- **Direct help** — tag `@iwrik00100` in your PR or issue
