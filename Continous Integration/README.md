# Continuous Integration (CI) with GitHub Actions – Architecture & Steps

---

## 1. What is Continuous Integration (CI)?

**Continuous Integration (CI)** is the practice of **frequently integrating code changes into a shared repository** and **automatically validating those changes** using builds, tests, and quality checks.

In simple words:

> CI ensures that every code change is automatically tested and validated before it is merged.

---

## 2. CI Architecture with GitHub Actions

### High-Level CI Architecture

```
Developer
   │
   │ git push / pull request
   ▼
GitHub Repository
   │
   │ (Event Trigger: push / pull_request)
   ▼
GitHub Actions Workflow
   │
   ├── Job 1: Checkout Code
   ├── Job 2: Install Dependencies
   ├── Job 3: Build Application
   ├── Job 4: Run Unit Tests
   ├── Job 5: Code Quality / Security Checks
   ▼
CI Result (Pass / Fail)
   │
   ▼
Feedback to Developer (GitHub UI)
```

### Architecture Explanation

* Developers push code or raise a PR
* GitHub triggers the CI workflow
* Workflow runs on GitHub-hosted or self-hosted runners
* Jobs validate the code
* Results are visible in GitHub (checks, logs)

---

## 3. CI Workflow Location

CI workflows **must be stored** at:

```
.github/workflows/
```

Example:

```
.github/workflows/ci.yml
```

---

## 4. CI Steps with GitHub Actions (Production Flow)

Below are the **standard CI steps used in real production projects**.

---

## Step 1: Trigger the CI Workflow

### Purpose

* Decide **when CI should run**

### Common Triggers

* `pull_request` (most important)
* `push` (feature branches)

### Example

```yaml
on:
  pull_request:
    branches: [ main ]
```

---

## Step 2: Checkout Source Code

### Purpose

* Download application source code to the runner

### Example

```yaml
- uses: actions/checkout@v4
```

---

## Step 3: Set Up Runtime Environment

### Purpose

* Install required language/runtime version

### Examples

* Node.js
* Java
* Python

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 20
```

---

## Step 4: Restore Cache (If Applicable)

### Purpose

* Speed up pipeline execution
* Avoid repeated downloads

```yaml
- uses: actions/cache@v4
```

---

## Step 5: Install Dependencies

### Purpose

* Install application dependencies

```yaml
- run: npm ci
```

---

## Step 6: Build the Application

### Purpose

* Compile or package the application

```yaml
- run: npm run build
```

---

## Step 7: Run Unit Tests

### Purpose

* Validate application functionality
* Catch bugs early

```yaml
- run: npm test
```

---

## Step 8: Code Quality & Security Checks

### Purpose

* Ensure code quality
* Detect vulnerabilities early

Examples:

* Linting
* Code coverage
* Static analysis

```yaml
- run: npm run lint
```

---

## Step 9: Generate & Upload Reports (Optional)

### Purpose

* Store test reports or coverage reports

```yaml
- uses: actions/upload-artifact@v4
```

---

## Step 10: CI Result & Feedback

### Purpose

* Provide fast feedback to developers

Outcomes:

* ✅ CI Passed → Code can be merged
* ❌ CI Failed → Fix required

Results are visible in:

* Pull Request checks
* GitHub Actions logs

---

## 5. Sample CI Workflow (Simplified)

```yaml
name: CI Pipeline

on:
  pull_request:
    branches: [ main ]

jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm test
```

---

## 6. CI Best Practices in Production

* Always run CI on `pull_request`
* Keep CI fast (<10 minutes)
* Fail fast on errors
* Do not deploy in CI
* Use caching carefully

---

## 7. CI vs CD (Quick Clarification)

| CI              | CD                 |
| --------------- | ------------------ |
| Build & test    | Deploy             |
| Runs frequently | Runs carefully     |
| Safe to repeat  | Must be controlled |

---

