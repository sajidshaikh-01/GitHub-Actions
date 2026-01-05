# GitHub Actions – Job Concurrency Explained (README)

---

## 1. What is Job Concurrency in GitHub Actions?

**Job concurrency** controls **how many workflow runs or jobs can run at the same time** for a given concurrency group.

It is mainly used to:

* Prevent multiple deployments running together
* Avoid race conditions
* Protect shared resources (production, staging, databases)

In simple words:

> Job concurrency ensures that only one job or workflow runs at a time for a specific environment or resource.

---

## 2. Why Job Concurrency is Important in Production

Without concurrency control:

* Two deployments may run at the same time
* Latest deployment may get overridden by older one
* Infrastructure state can break

With concurrency:

* Only **one deployment runs at a time**
* Older runs are queued or cancelled

---

## 3. How Job Concurrency Works

GitHub Actions uses a **concurrency group name**.

If two jobs/workflows use the **same concurrency group**:

* Only one runs at a time
* Others wait or get cancelled

---

## 4. Basic Concurrency Example (Workflow Level)

### Use Case: Prevent Parallel Production Deployments

```yaml
name: Production Deployment

on:
  workflow_dispatch:

concurrency:
  group: production
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: echo "Deploying to production"
```

### Explanation

* `group: production` → defines concurrency group
* `cancel-in-progress: true` → cancels older running jobs
* Only **one production deployment** can run at a time

---

## 5. Job-Level Concurrency Example

Concurrency can also be applied **per job**.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    concurrency:
      group: prod-deploy
      cancel-in-progress: true
    steps:
      - run: echo "Deploying safely"
```

---

## 6. Dynamic Concurrency Groups (Very Important ⭐)

Dynamic groups are used to control concurrency **per branch or environment**.

### Example: One Deployment Per Branch

```yaml
concurrency:
  group: deploy-${{ github.ref }}
  cancel-in-progress: true
```

### Example: One Deployment Per Environment

```yaml
concurrency:
  group: deploy-${{ inputs.environment }}
  cancel-in-progress: true
```

---

## 7. cancel-in-progress Explained

| Value | Behavior                               |
| ----- | -------------------------------------- |
| true  | Cancels older running jobs             |
| false | Queues new jobs until current finishes |

---

## 8. Concurrency vs Parallel Jobs (Common Confusion)

| Parallel Jobs         | Concurrency      |
| --------------------- | ---------------- |
| Run jobs at same time | Restricts jobs   |
| Default behavior      | Explicit control |
| Improves speed        | Improves safety  |

---

## 9. Real Production Use Cases

| Scenario               | Why Concurrency          |
| ---------------------- | ------------------------ |
| Production deployments | Avoid conflicts          |
| Terraform apply        | Prevent state corruption |
| Database migrations    | Ensure one execution     |
| GitOps sync jobs       | Avoid drift              |

---

