# 🤖 AI Self-Healing CI/CD Pipeline

 An AI-powered DevOps automation pipeline that automatically detects GitHub CI/CD failures, identifies the root cause, generates a fix using an LLM, commits the changes to a new branch, creates a Pull Request, and notifies the developer via email.

---

## 🚀 Project Overview

Traditional CI/CD pipelines stop whenever tests fail, requiring developers to manually inspect logs, identify the issue, fix the code, create a commit, and open a Pull Request.

This project automates that entire recovery process using **GitHub Actions**, **n8n**, **GitHub REST APIs**, and **OpenAI**.

When a CI pipeline fails:

1. GitHub sends a webhook to n8n.
2. n8n fetches workflow logs.
3. The failed file is identified.
4. Source code is fetched automatically.
5. OpenAI analyzes the failure.
6. AI generates the corrected code.
7. A new Git branch is created.
8. The fix is committed automatically.
9. A Pull Request is opened.
10. The developer receives an email notification.

The entire recovery happens without any manual intervention.

---

# 📌 Problem Statement

Developers spend significant time debugging failed CI/CD pipelines.

Typical workflow:

```
Pipeline Fails
      ↓
Read Logs
      ↓
Find Root Cause
      ↓
Fix Code
      ↓
Commit
      ↓
Push
      ↓
Open PR
```

This project automates all these repetitive steps.

---

# ✨ Features

- ✅ GitHub Actions CI Pipeline
- ✅ Automatic failure detection
- ✅ GitHub Webhook integration
- ✅ Fetch workflow logs via GitHub REST API
- ✅ Detect modified files automatically
- ✅ Retrieve source code from GitHub
- ✅ AI-generated bug fixes using OpenAI
- ✅ Automatic branch creation
- ✅ Automatic code commit
- ✅ Automatic Pull Request creation
- ✅ Email notification after successful fix
- ✅ Fully automated end-to-end workflow

---

# 🛠️ Tech Stack

| Category | Technologies |
|-----------|--------------|
| CI/CD | GitHub Actions |
| Automation | n8n |
| AI | OpenAI GPT |
| APIs | GitHub REST API |
| Language | JavaScript |
| Runtime | Node.js |
| Version Control | Git & GitHub |
| Notifications | Gmail API / SMTP |

---

# 🏗️ System Architecture

```
Developer Pushes Code
          │
          ▼
 GitHub Actions CI
          │
          ▼
    Tests Fail ❌
          │
          ▼
 GitHub Failure Webhook
          │
          ▼
        n8n
          │
          ├─────────────► Fetch Workflow Logs
          │
          ├─────────────► Fetch Changed Files
          │
          ├─────────────► Fetch Source Code
          │
          ├─────────────► Generate AI Prompt
          │
          ▼
      OpenAI GPT
          │
          ▼
 Generates Fixed Code
          │
          ▼
 Create New Branch
          │
          ▼
 Commit Changes
          │
          ▼
 Create Pull Request
          │
          ▼
 Email Notification
```
<img width="1505" height="148" alt="Screenshot 2026-08-05 231306" src="https://github.com/user-attachments/assets/55ea632b-0a03-48eb-a805-d0a08e74f520" />

---

# ⚙️ Workflow

## Step 1 — GitHub Actions

A GitHub Actions workflow executes the project tests.

If all tests pass:

```
Pipeline ✅
```


If any test fails:

```
Pipeline ❌
```

GitHub triggers a webhook.
<img width="1747" height="850" alt="Screenshot 2026-08-05 230337" src="https://github.com/user-attachments/assets/8f64a4bb-74f2-4293-bb93-50fe920f2d71" />

---

## Step 2 — GitHub Failure Webhook

n8n receives:

- Repository
- Commit SHA
- Branch
- Workflow Run ID

This starts the self-healing workflow.
<img width="1845" height="846" alt="Screenshot 2026-08-05 210837" src="https://github.com/user-attachments/assets/9d381545-0cc8-427d-b987-3c8120ede392" />

---

## Step 3 — Fetch Workflow Logs

Using GitHub REST API:

```
GET /actions/runs/{run_id}/jobs
```

The workflow retrieves:

- Failed Job
- Failed Step
- Logs
- Stack Trace
<img width="1857" height="906" alt="Screenshot 2026-08-05 230517" src="https://github.com/user-attachments/assets/6ba07b28-eebe-48c4-ace1-45eb713597db" />

---

## Step 4 — Fetch Changed Files

The workflow compares the latest commit with the previous commit.

GitHub Compare API:

```
GET /compare/{base}...{head}
```

Only files modified in the latest commit are considered.
<img width="1863" height="907" alt="Screenshot 2026-08-05 230552" src="https://github.com/user-attachments/assets/dea4af14-b1a8-494d-a692-31c33503a188" />

---

## Step 5 — Select File to Fix

A JavaScript node filters the changed files.

Supported extensions:

- .js
- .jsx
- .ts
- .tsx
- .py
- .java
- .go
- .rb

The workflow selects the target source file.
<img width="1867" height="896" alt="Screenshot 2026-08-05 230622" src="https://github.com/user-attachments/assets/b8c3b16b-2e5d-4a50-8426-3f2c55cbda0b" />

---

## Step 6 — Fetch Source Code

GitHub Contents API downloads the current file contents.

The AI receives:

- Source code
- Error logs
- Failed step
- Commit information
<img width="1877" height="890" alt="Screenshot 2026-08-05 230646" src="https://github.com/user-attachments/assets/d61332bc-5bed-4bc7-83be-35e3ae40c2ea" />

---

## Step 7 — AI Root Cause Analysis

An OpenAI prompt is dynamically generated containing:

- CI logs
- Failed step
- Repository
- Branch
- Commit
- File contents

The AI identifies:

- Root cause
- Correct implementation
- Updated source code
- Commit message
- PR title
- PR description
- Branch name
<img width="1872" height="901" alt="Screenshot 2026-08-05 230715" src="https://github.com/user-attachments/assets/5564d154-3d95-41d7-af4d-ae1f8cc7582e" />

---

## Step 8 — Create Branch

GitHub API:

```
POST /git/refs
```

Creates:

```
ai-fix/<issue-name>
```
<img width="1865" height="886" alt="Screenshot 2026-08-05 230755" src="https://github.com/user-attachments/assets/1c585d06-2dfe-47c0-a912-f0995d7fd78a" />

---

## Step 9 — Commit Fixed Code

Workflow:

- Get latest file SHA
- Base64 encode updated source
- Push via GitHub Contents API

```
PUT /contents/{file}
```
<img width="1848" height="902" alt="Screenshot 2026-08-05 230832" src="https://github.com/user-attachments/assets/cab8f7a8-628a-4643-8f0f-62434f23043f" />
<img width="1862" height="897" alt="Screenshot 2026-08-05 230905" src="https://github.com/user-attachments/assets/d3c68a4b-e46f-4958-bcbc-c7c7d2b95614" />

---

## Step 10 — Open Pull Request

GitHub API:

```
POST /pulls
```
<img width="1857" height="903" alt="Screenshot 2026-08-05 230927" src="https://github.com/user-attachments/assets/ba88b9be-db38-43d1-9aa1-6f8dc176f688" />
<img width="1320" height="513" alt="Screenshot 2026-08-05 230954" src="https://github.com/user-attachments/assets/93de52da-dcd7-42ce-bb87-e612f000ba60" />

Automatically creates:

- Branch
- Commit
- Pull Request

Example:

```
fix: initialize express app in test.js
```
<img width="1256" height="665" alt="Screenshot 2026-08-05 231102" src="https://github.com/user-attachments/assets/6c7ecaff-e188-4ee1-860d-1a1a48e96ad6" />

---

## Step 11 — Notify Developer

Finally the workflow sends an email including:

- Repository
- Failure
- Root Cause
- PR Title
- Pull Request Link
<img width="1525" height="210" alt="Screenshot 2026-08-05 231147" src="https://github.com/user-attachments/assets/47690114-337c-4fae-b857-06b0ae3a58f9" />

---

# 📂 n8n Workflow

The workflow consists of the following nodes:

```
GitHub Failure Webhook
        │
        ▼
Fetch Logs
        │
        ▼
Fetch Changed Files
        │
        ▼
Select File To Fix
        │
        ▼
Fetch File Content
        │
        ▼
Generate AI Prompt
        │
        ▼
OpenAI
        │
        ▼
Extract Response
        │
        ▼
Create Branch
        │
        ▼
Get File SHA
        │
        ▼
Push Fixed File
        │
        ▼
Open Pull Request
        │
        ▼
Send Email
```

---

# 📸 Demo

## CI Failure

- GitHub Actions detects a failed test.
- Workflow stops at the failing step.

---

## AI Analysis

The workflow:

- retrieves logs
- identifies the changed file
- fetches source code
- generates an AI prompt

---

## Automated Fix

OpenAI generates:

- corrected code
- branch name
- commit message
- PR description

---

## Pull Request Created

A new branch is automatically created.

GitHub opens a Pull Request containing the AI-generated fix.

---

## CI Success

After merging:

```
✅ All checks passed
```

---

# 🔒 Safety Measures

The workflow only modifies:

- files changed in the latest commit

It never attempts to modify unrelated project files.

Additional safeguards include:

- isolated AI branch
- Pull Request review before merge
- original branch remains unchanged

---

# 📈 Future Improvements

- Support multiple file fixes
- Docker deployment
- Kubernetes integration
- Slack notifications
- Microsoft Teams integration
- Jira ticket creation
- Automatic PR review
- Rollback on failed AI fixes
- Support for multiple programming languages
- Retrieval-Augmented Generation (RAG) for project-aware fixes

---

# 💡 Key Learnings

This project helped me gain hands-on experience with:

- GitHub Actions
- CI/CD automation
- GitHub REST APIs
- Webhooks
- n8n workflow automation
- OpenAI API integration
- Prompt Engineering
- Automated Pull Request creation
- DevOps automation
- AI-assisted software engineering

---

# 📊 Resume Highlights

- Designed and implemented an AI-powered Self-Healing CI/CD pipeline.
- Automated CI failure detection and recovery using GitHub Actions and n8n.
- Integrated OpenAI to analyze workflow failures and generate code fixes.
- Automated Git branch creation, commits, and Pull Requests using GitHub REST APIs.
- Reduced manual debugging effort by automating the complete CI recovery workflow.

---

# 👨‍💻 Author

**Dilli Prathap**

If you found this project interesting, consider giving it a ⭐ on GitHub.
