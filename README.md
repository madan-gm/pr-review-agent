# 🤖 PR Review Agent — Reva

> **Reva** is a git-native AI code reviewer that lives inside your repository, automatically reviews every pull request, and enforces **Segregation of Duties (SOD)** so reviewers can never approve their own findings.

Built using the **GitAgent open standard** for the GitAgent Hackathon by Lyzr AI × HackCulture.

---

## 🚀 What It Does

Every time a pull request is opened or updated, Reva:

1. 🔐 **Security-first analysis**
   Detects secrets, SQL injection, XSS, auth bypasses, and unsafe patterns

2. 🧠 **Code quality review**
   Identifies bugs, edge cases, performance issues, and missing validations

3. 📝 **Structured feedback**
   Produces clear findings with severity levels and actionable fixes

4. ⚖️ **Segregation of Duties (SOD)**
   A separate **fact-checker agent** makes the final approval decision

5. 📊 **Audit logging**
   Maintains a persistent review trail for traceability and compliance

---

## 🏆 Why This Stands Out

| Capability             | Implementation                                                |
| ---------------------- | ------------------------------------------------------------- |
| GitAgent Standard      | `agent.yaml`, `SOUL.md`, `RULES.md`, `DUTIES.md`, `AGENTS.md` |
| Multi-skill pipeline   | `security-scan → code-review → review-summary`                |
| Multi-agent SOD        | reviewer agent + fact-checker approver                        |
| Workflow orchestration | `workflows/pr-review-flow.yaml`                               |
| Tooling                | GitHub PR + shell tools                                       |
| Memory & audit         | `memory/runtime/dailylog.md`                                  |
| CI Integration         | GitHub Actions workflow                                       |
| Compliance             | audit checklist + validation                                  |

---

## 📁 Project Structure

```
pr-review-agent/
├── agent.yaml
├── SOUL.md
├── RULES.md
├── DUTIES.md
├── AGENTS.md
├── skills/
├── tools/
├── workflows/
├── agents/fact-checker/
├── knowledge/
├── memory/runtime/
├── hooks/
├── compliance/
├── examples/
├── .github/workflows/gitagent-ci.yml
└── .gitignore
```

---

## ⚡ Quick Start

```bash
# Validate agent
npx @open-gitagent/gitagent@latest validate

# Run agent (Lyzr adapter)
npx @open-gitagent/gitagent@latest run \
  --dir . \
  --adapter lyzr
```

---

## 🔧 Environment Setup

Create a `.env` file (never commit this):

```
GEMINI_API_KEY=your_api_key
LYZR_API_KEY=your_api_key
GITHUB_TOKEN=your_token
```

---

## ⚙️ GitHub Actions Integration

1. Go to **Settings → Secrets → Actions**
2. Add:

* `GEMINI_API_KEY`
* `LYZR_API_KEY`
* `GITHUB_TOKEN` (auto-provided)

3. Create or update a PR → Reva runs automatically ✅

---

## 🔐 Segregation of Duties (SOD)

```
PR → Reviewer Agent → Fact-Checker Agent → Decision
```

* Reviewer: analyzes code
* Fact-checker: approves/rejects
* No overlap = enforced trust model

---

## 🧠 Architecture

Reva follows a **modular agent design**:

* **SOUL** → identity & reasoning style
* **RULES** → hard constraints
* **DUTIES** → role separation (SOD)
* **Skills** → reusable capabilities
* **Tools** → external actions
* **Workflows** → execution flow

---

## 🛠 Built With

* GitAgent (open agent standard)
* Lyzr AI (execution adapter)
* Gemini API (LLM backend)
* GitHub Actions (CI integration)

---

## 📌 Submission

* Git-native AI agent for automated PR review
* Security-first design with compliance focus
* Demonstrates multi-agent architecture with SOD

---

*Submitted for GitAgent Hackathon — Lyzr AI × HackCulture*
