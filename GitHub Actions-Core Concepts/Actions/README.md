# GitHub Actions – Actions 

---

## 1. What are Actions in GitHub Actions?

**Actions** are **reusable automation units** in GitHub Actions.

They allow you to **reuse common tasks** like:

* Checking out code
* Setting up languages (Java, Node.js, Python)
* Building Docker images
* Deploying to cloud platforms

In simple words:

> An action is a pre-built step that performs a specific task in a workflow.

---

## 2. Why Actions are Important

Actions help you:

* Avoid writing repetitive shell scripts
* Reuse community-tested automation
* Build CI/CD pipelines faster
* Follow best practices easily

---

## 3. Where Actions are Used

Actions are used inside **steps** of a job.

Example:

```yaml
steps:
  - uses: actions/checkout@v4
```

Here:

* `uses` → tells GitHub to use an action
* `actions/checkout` → action name
* `@v4` → version of the action

---

## 4. Types of Actions in GitHub Actions

### 4.1 Official Actions

These actions are maintained by GitHub and are **highly trusted**.

Common official actions:

* `actions/checkout` – Checkout repository code
* `actions/setup-node` – Setup Node.js
* `actions/setup-java` – Setup Java
* `actions/cache` – Cache dependencies
* `actions/upload-artifact` – Upload build artifacts

Example:

```yaml
- uses: actions/checkout@v4
- uses: actions/setup-node@v4
  with:
    node-version: 20
```

---

### 4.2 Community Actions

Community actions are created by developers and organizations.

They are available in the **GitHub Marketplace**.

Examples:

* Docker build and push actions
* Terraform actions
* Security scanning actions (Trivy, Snyk)

Example:

```yaml
- uses: aquasecurity/trivy-action@v0.24.0
```

⚠️ Production Tip:

* Always check action popularity
* Prefer pinned versions (avoid `@main`)

---

### 4.3 Custom Actions

Custom actions are **created by you** for your organization or project.

Used when:

* You need custom logic
* Company-specific automation
* Reuse internal scripts

Custom actions can be written in:

* JavaScript
* Docker
* Composite YAML

---

## 5. Types of Custom Actions

### 5.1 JavaScript Action

* Runs directly on the runner
* Fast execution

Used for:

* API calls
* Small automation logic

---

### 5.2 Docker Action

* Runs inside a Docker container
* Full control over environment

Used for:

* Complex tools
* Custom CLIs
* Security scanners

---

### 5.3 Composite Action (Most Used)

* Combines multiple steps
* Written in YAML

Used for:

* Standardizing pipelines
* Reusing multiple shell commands

---

## 6. Action Versioning (Very Important for Interview)

Actions should always be versioned.

Good practice:

```yaml
- uses: actions/checkout@v4
```

Avoid:

```yaml
- uses: actions/checkout@main
```

Why?

* Prevents breaking changes
* Ensures stable pipelines

---

## 7. Inputs and Outputs of Actions

Actions can accept **inputs** and produce **outputs**.

Example:

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 18
```

---

## 8. Actions vs Run Commands

| Action        | Run Command     |
| ------------- | --------------- |
| Reusable      | One-time script |
| Cleaner YAML  | More shell code |
| Maintained    | User maintained |
| Best practice | Quick tasks     |

---

## 9. Actions in Production CI/CD

Typical production usage:

1. Checkout code (action)
2. Setup language (action)
3. Cache dependencies (action)
4. Security scan (action)
5. Build & push image (action)
6. Upload artifacts (action)

---



End of README
