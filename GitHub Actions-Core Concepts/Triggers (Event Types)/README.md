# GitHub Actions – Triggers (Event Types) Used in Production

---

## 1. What are Triggers in GitHub Actions?

**Triggers (Events)** define **WHEN a workflow runs**.

In production, triggers are chosen carefully to:

* Avoid unnecessary pipeline runs
* Protect production deployments
* Support CI, CD, and GitOps workflows

In simple words:

> Triggers decide *when automation should start*.

---

## 2. Most Common Triggers Used in Production

---

## 2.1 `push` Event (Most Used for CI)

### When it triggers

* When code is pushed to a branch

### Production Usage

* CI pipelines (build, test, lint)
* Feature branch validation
* Non-production deployments

### Example

```yaml
on:
  push:
    branches:
      - main
      - develop
```

### Best Practice

* Avoid deploying to production directly on `push`
* Use branch filters

---

## 2.2 `pull_request` Event (Mandatory in Production)

### When it triggers

* When a PR is opened, updated, or reopened

### Production Usage

* Run tests before merge
* Code quality checks
* Security scans

### Example

```yaml
on:
  pull_request:
    branches: [ main ]
```

### Why Important

* Prevents broken code from reaching main branch

---

## 2.3 `workflow_dispatch` (Manual Trigger)

### When it triggers

* Manually triggered from GitHub UI

### Production Usage

* Production deployments
* Hotfix deployments
* Rollbacks

### Example

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        required: true
        type: choice
        options:
          - staging
          - production
```

### Production Tip

* Always use manual approval for prod deployments

---

## 2.4 `schedule` (Cron Jobs)

### When it triggers

* Runs on a scheduled time (cron)

### Production Usage

* Nightly builds
* Dependency scans
* Security scans
* Backup automation

### Example

```yaml
on:
  schedule:
    - cron: "0 2 * * *"
```

---

## 2.5 `release` Event

### When it triggers

* When a release is created or published

### Production Usage

* Production deployments
* Versioned releases
* Artifact publishing

### Example

```yaml
on:
  release:
    types: [ published ]
```

---

## 2.6 `workflow_call` (Reusable Workflows)

### When it triggers

* When called by another workflow

### Production Usage

* Centralized CI/CD logic
* Standard pipelines across teams

### Example

```yaml
on:
  workflow_call:
```

---

## 2.7 `repository_dispatch` (External Trigger)

### When it triggers

* Triggered via GitHub API

### Production Usage

* Trigger pipelines from:

  * Jenkins
  * Argo CD
  * External tools

### Example

```yaml
on:
  repository_dispatch:
    types: [ deploy-prod ]
```

---

## 3. Triggers Rarely Used in Production (FYI)

| Trigger         | Reason                  |
| --------------- | ----------------------- |
| `fork`          | Security risk           |
| `issues`        | Not CI/CD related       |
| `issue_comment` | Mostly automation       |
| `watch`         | Not production relevant |

---

## 4. Recommended Trigger Strategy (Real Production)

| Use Case               | Trigger                         |
| ---------------------- | ------------------------------- |
| CI (tests, lint)       | `pull_request`                  |
| Dev environment deploy | `push`                          |
| Prod deployment        | `workflow_dispatch` / `release` |
| Security scans         | `schedule`                      |
| Shared pipelines       | `workflow_call`                 |

---
