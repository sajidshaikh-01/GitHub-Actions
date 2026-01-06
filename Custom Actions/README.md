# GitHub Actions – Custom Actions (Types & Production Usage)

---

## 1. What is a Custom Action in GitHub Actions?

A **custom action** is a **user-defined action** created to perform a **specific, reusable task** that is not fully covered by existing marketplace actions.

In simple words:

> A custom action is your own reusable automation logic packaged as an action.

Custom actions are used when:

* Company-specific logic is required
* Standard actions are not flexible enough
* The same logic is reused across many workflows

---

## 2. Why Custom Actions Are Used in Production

In production environments, custom actions help teams:

* Standardize CI/CD steps across repositories
* Reduce duplicated shell scripts
* Enforce security and compliance rules
* Maintain automation logic in one place

---

## 3. Where Custom Actions Are Stored

Custom actions are stored in:

* The same repository **OR**
* A dedicated shared repository

Typical structure:

```
.github/actions/my-custom-action/
  ├── action.yml
  ├── script.sh / index.js / Dockerfile
```

---

## 4. Types of Custom Actions

GitHub Actions supports **three types of custom actions**:

1. JavaScript Actions
2. Docker Container Actions
3. Composite Actions

---

## 5. JavaScript Actions

### What They Are

* Written in JavaScript (Node.js)
* Run directly on the GitHub Actions runner

### Characteristics

* Fast execution
* No container startup time
* Good access to GitHub APIs

### Typical Use Cases

* GitHub API automation
* Repository management
* Small logic & validations

### Example (Conceptual)

```
runs:
  using: 'node20'
  main: 'index.js'
```

---

## 6. Docker Container Actions

### What They Are

* Run inside a Docker container
* Use a `Dockerfile`

### Characteristics

* Full control over environment
* Heavier and slower to start

### Typical Use Cases

* Complex tools
* Custom CLIs
* Security scanners

### Example (Conceptual)

```
runs:
  using: 'docker'
  image: 'Dockerfile'
```

---

## 7. Composite Actions (Most Used ⭐)

### What They Are

* Written entirely in YAML
* Combine multiple `run` and `uses` steps

### Characteristics

* Simple
* No Docker
* No JavaScript
* Easy to maintain

### Typical Use Cases

* Standard CI steps
* Build/test/scan sequences
* Organization-wide pipeline standards

### Example (Conceptual)

```
runs:
  using: 'composite'
  steps:
    - run: echo "Build"
    - run: echo "Test"
```

---

## 8. Which Custom Action Is Used Mostly in Production?

### ✅ Composite Actions (Most Common)

Why composite actions dominate production:

* Easy to read and audit
* No compiled code
* Works well with reusable workflows
* Best for standardization

---

## 9. Production Usage Comparison

| Action Type | Production Usage             |
| ----------- | ---------------------------- |
| Composite   | ⭐⭐⭐⭐⭐ (Most used)            |
| JavaScript  | ⭐⭐⭐ (Used when logic needed) |
| Docker      | ⭐⭐ (Used for special cases)  |

---

## 10. Custom Actions vs Reusable Workflows

| Custom Action    | Reusable Workflow       |
| ---------------- | ----------------------- |
| Reuses steps     | Reuses entire workflows |
| Used inside jobs | Used as a job           |
| Fine-grained     | High-level              |

---

## 11. Production Best Practices

* Prefer composite actions for most cases
* Version actions (`v1`, `v2`)
* Avoid `@main`
* Keep actions small and focused
* Store shared actions in a central repo

---


End of README
