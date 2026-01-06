# GitHub Actions Security – Production & Interview Guide

---

## 1. Why Security is Important in GitHub Actions

GitHub Actions pipelines:

* Access **source code**
* Use **cloud credentials**
* Build and push **container images**
* Deploy to **production environments**

If CI/CD is compromised, attackers can:

* Steal secrets
* Push malicious code
* Deploy backdoors to production

👉 That’s why **pipeline security is as important as application security**.

---

## 2. GitHub Actions Security Model (High Level)

GitHub Actions security is based on:

* **Identity & permissions**
* **Secrets management**
* **Workflow trust boundaries**
* **Runner security**
* **Supply chain protection**

---

## 3. Permissions & Least Privilege (VERY IMPORTANT)

### Default Behavior

* GitHub provides a **GITHUB_TOKEN** to every workflow
* By default, it may have more permissions than required

### Best Practice: Explicit Permissions

```yaml
permissions:
  contents: read
  id-token: write
```

Why this matters:

* Follows **least privilege principle**
* Reduces blast radius if workflow is compromised

🔴 Interview Tip:

> Always restrict permissions at workflow or job level.

---

## 4. Secrets Management

### GitHub Secrets

* Stored encrypted
* Masked in logs automatically
* Available at:

```yaml
${{ secrets.SECRET_NAME }}
```

### Types of Secrets

* Repository secrets
* Environment secrets
* Organization secrets

### Best Practices

* Never hardcode secrets
* Rotate secrets regularly
* Use environment secrets for prod

---

## 5. OpenID Connect (OIDC) – Production Standard

### What is OIDC?

OIDC allows GitHub Actions to **authenticate to cloud providers without storing long-lived credentials**.

### How it Works (Conceptual Flow)

1. Workflow requests an ID token from GitHub
2. GitHub issues a short-lived token
3. Cloud provider validates token
4. Temporary credentials are granted

### Why OIDC is Preferred

* No static access keys
* Short-lived credentials
* Reduced secret leakage risk

### Required Permissions

```yaml
permissions:
  id-token: write
  contents: read
```

🔴 Interview Gold Line:

> In production, we prefer OIDC over static secrets for cloud authentication.

---

## 6. Environment Protection Rules

### GitHub Environments

Used to protect sensitive deployments like **staging** and **production**.

Features:

* Required reviewers
* Environment-level secrets
* Deployment history

Example:

```yaml
environment: production
```

Why this is important:

* Prevents accidental prod deployment
* Enforces approval workflow

---

## 7. Pull Request Security (VERY IMPORTANT)

### Problem

Forked repositories can open PRs, but **should not get access to secrets**.

### GitHub Protection

* Secrets are **NOT exposed to workflows from forks**
* Protects against malicious PRs

Best Practice:

* Avoid using secrets in `pull_request` from forks
* Use `pull_request_target` carefully

---

## 8. Runner Security

### GitHub-Hosted Runners

* Ephemeral (destroyed after job)
* Clean environment each run
* Lower risk of persistence attacks

### Self-Hosted Runners (HIGH RISK IF MISUSED)

Risks:

* Secrets remain on machine
* Malware persistence

Best Practices:

* Use ephemeral runners
* Isolate runners per repo/team
* Restrict network access

---

## 9. Third-Party Action Security (Supply Chain)

### Risk

Using untrusted actions can introduce malicious code.

### Best Practices

* Use verified actions
* Pin actions to **commit SHA**, not tags

Example:

```yaml
uses: actions/checkout@v4
```

Better:

```yaml
uses: actions/checkout@<commit-sha>
```

---

## 10. Secrets Masking & Log Safety

GitHub automatically masks secrets in logs.

Best Practices:

* Never echo secrets
* Avoid debug logging in prod pipelines

---

## 11. Branch Protection & CI Security

Branch protection rules:

* Require PR reviews
* Require status checks
* Prevent force push

Why this matters:

* Ensures CI pipelines pass before merge
* Prevents bypassing security checks

---

## 12. Security Scanning in GitHub Actions

Common security stages added in CI:

* Dependency scanning
* Container image scanning
* Static code analysis

Purpose:

* Shift-left security
* Catch vulnerabilities early

---

ons**.
