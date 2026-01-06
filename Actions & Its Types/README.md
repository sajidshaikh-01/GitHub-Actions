# GitHub Actions – Runners Explained 

---

## 1. What is a Runner in GitHub Actions?

A **runner** is a **machine that executes GitHub Actions jobs**.

When a workflow is triggered:

* GitHub assigns a runner
* The runner executes all the steps defined in the job
* Logs and results are sent back to GitHub

In simple words:

> A runner is the worker that runs your CI/CD pipeline.

---

## 2. How Runners Fit into GitHub Actions Architecture

```
Workflow Triggered
        │
        ▼
GitHub Actions Scheduler
        │
        ▼
Runner (VM or Host)
        │
        ▼
Jobs & Steps Executed
```

---

## 3. Types of Runners in GitHub Actions

GitHub Actions supports **two main types of runners**:

1. GitHub-hosted runners
2. Self-hosted runners

---

## 4. GitHub-Hosted Runners

### What They Are

* Fully managed by GitHub
* Run on GitHub-provided virtual machines
* Automatically provisioned and destroyed

### Supported Operating Systems

* `ubuntu-latest`
* `windows-latest`
* `macos-latest`

### Example Usage

```yaml
runs-on: ubuntu-latest
```

---

### Advantages

* Zero setup
* Auto-scaling
* Always clean environment
* Easy to start

---

### Limitations

* No access to private networks
* Limited customization
* Usage limits & cost considerations

---

## 5. Self-Hosted Runners

### What They Are

* Runners installed and managed by **your organization**
* Can run on:

  * On-prem servers
  * Cloud VMs
  * Kubernetes pods

### Example Usage

```yaml
runs-on: self-hosted
```

---

### Advantages

* Access to private networks (VPC, on-prem)
* Custom hardware & tooling
* Better control over security
* No GitHub-hosted execution limits

---

### Challenges

* Requires setup and maintenance
* Scaling responsibility is on you
* Security hardening needed

---

## 6. Runner Labels (Important ⭐)

Self-hosted runners can have **labels**.

```yaml
runs-on: [self-hosted, linux, prod-runner]
```

Labels help:

* Target specific runners
* Separate CI and CD workloads

---

## 7. Which Runner is Mostly Used in Production?

### ✅ Self-Hosted Runners (Mostly Used)

In real production environments:

* **Self-hosted runners** are preferred for:

  * Deployments
  * Infrastructure changes
  * Accessing private clusters or VPCs

### Why?

* Secure network access
* Better compliance
* Full environment control

---

### GitHub-Hosted Runners in Production

Still used for:

* CI (build, test, scan)
* Open-source projects
* Non-sensitive workloads

---

## 8. Production Usage Pattern (Very Common)

| Pipeline Stage       | Runner Type   |
| -------------------- | ------------- |
| CI (build/test/scan) | GitHub-hosted |
| CD (deploy/infra)    | Self-hosted   |

---

## 9. Runners vs Containers (Clarification)

| Runner        | Container            |
| ------------- | -------------------- |
| Executes jobs | Provides environment |
| VM or host    | Docker container     |
| Required      | Optional             |

You can run **containers inside runners**, but runners always exist.

---

## 10. Security Considerations

* Use self-hosted runners for sensitive deployments
* Isolate runners per environment
* Avoid running untrusted PRs on self-hosted runners

---


