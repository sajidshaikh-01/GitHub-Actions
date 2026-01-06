# Continuous Deployment (CD) Using GitHub Actions

---

## 1. What is Continuous Deployment (CD)?

**Continuous Deployment (CD)** is the practice of **automatically deploying validated code changes to an environment** after they successfully pass the CI stage.

In simple words:

> CD takes the output of CI and deploys it automatically in a controlled and safe way.

---

## 2. CI vs CD (Quick Context)

| CI (Continuous Integration) | CD (Continuous Deployment) |
| --------------------------- | -------------------------- |
| Build & test code           | Deploy application         |
| Runs frequently             | Runs carefully             |
| Fail fast                   | Safe & controlled          |
| No production impact        | Direct environment impact  |

---

## 3. Continuous Deployment Architecture with GitHub Actions

### High-Level CD Architecture

```
Developer merges code to main branch
        │
        ▼
CI Pipeline (Build, Test, Scan)
        │
        ▼
Artifact / Image Created
        │
        ▼
CD Workflow Triggered
        │
        ├── Authenticate to Cloud / Cluster
        ├── Deploy Application
        ├── Run Post-Deploy Checks
        ▼
Application Updated in Environment
```

---

## 4. How GitHub Actions Enables CD

GitHub Actions provides:

* Event-based triggers
* Secure secrets & OIDC authentication
* Environment protection rules
* Job sequencing and conditions

These features make GitHub Actions suitable for **production-grade CD**.

---

## 5. Common CD Triggers Used in Production

### 5.1 `workflow_dispatch` (Most Common)

Used for:

* Production deployments
* Manual approvals
* Controlled rollouts

```yaml
on:
  workflow_dispatch:
```

---

### 5.2 `push` to Main Branch

Used for:

* Auto-deploy to dev or staging

```yaml
on:
  push:
    branches: [ main ]
```

---

### 5.3 `release` Event

Used for:

* Versioned production deployments

```yaml
on:
  release:
    types: [ published ]
```

---

## 6. Standard CD Steps Using GitHub Actions

Below is the **real production CD flow**.

---

## Step 1: Trigger CD Workflow

### Purpose

* Decide when deployment should start

Triggers:

* Manual (`workflow_dispatch`)
* Release-based

---

## Step 2: Fetch Build Artifact or Image

### Purpose

* Deploy exactly what CI validated

Methods:

* Pull Docker image
* Download artifact

---

## Step 3: Authenticate Securely

### Purpose

* Access cloud or cluster securely

Production Best Practice:

* Use **OIDC authentication**
* Avoid long-lived secrets

---

## Step 4: Deploy to Target Environment

### Purpose

* Update application safely

Examples:

* Kubernetes deployment
* Cloud service deployment
* VM-based deployment

---

## Step 5: Post-Deployment Verification

### Purpose

* Ensure deployment success

Checks:

* Health checks
* Smoke tests
* Rollback on failure

---

## 7. Example CD Workflow (Simplified)

```yaml
name: CD Pipeline

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Authenticate
        run: echo "Authenticating to cloud"

      - name: Deploy
        run: echo "Deploying application"

      - name: Verify
        run: echo "Verifying deployment"
```

---

## 8. Environment Protection in CD (Very Important ⭐)

GitHub Environments allow:

* Manual approvals
* Environment-specific secrets
* Deployment history

Used to protect:

* Production deployments

---

## 9. Why CD is More Controlled Than CI

Reasons:

* Direct production impact
* Risk of outages
* Compliance requirements

Therefore:

* Manual triggers
* Approval gates
* Rollback strategies are used

---

## 10. Continuous Deployment vs GitOps

| CD with GitHub Actions | GitOps               |
| ---------------------- | -------------------- |
| Push-based             | Pull-based           |
| Pipeline deploys       | Cluster syncs        |
| Faster feedback        | Strong drift control |

In production, both are often used together.

---

## 11. Production Best Practices for CD

* Never deploy directly from feature branches
* Deploy only CI-validated artifacts
* Use environment approvals
* Log and monitor deployments
* Prefer GitOps for Kubernetes

---

