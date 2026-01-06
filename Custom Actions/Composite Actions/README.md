# GitHub Actions – Composite Actions Explained

---

## 1. What is a Composite Action?

A **composite action** is a type of custom GitHub Action that allows you to **combine multiple steps (run commands and actions) into a single reusable action**, written entirely in **YAML**.

In simple words:

> A composite action is a reusable group of steps packaged as one action.

---

## 2. Why Composite Actions Exist

Composite actions solve these problems:

* Repeating the same steps in many workflows
* Copy-pasting CI/CD logic across repositories
* Maintaining pipeline logic in multiple places

They help teams:

* Standardize CI/CD steps
* Reduce duplication
* Improve maintainability

---

## 3. When We Use Composite Actions in Production

Composite actions are used in production when:

* The same steps are repeated across many repos
* Logic is simple and shell-based
* We want transparency and auditability

Typical production use cases:

* Build steps
* Test steps
* Docker build & push
* Security scans

---

## 4. Composite Action Directory Structure

A composite action lives inside a directory with an `action.yml` file.

Example:

```
.github/actions/common-ci/
├── action.yml
```

---

## 5. Basic Composite Action Example

### `action.yml`

```yaml
name: Common CI Steps

description: Run common build and test steps

inputs:
  app_name:
    required: true
    description: Application name

runs:
  using: composite
  steps:
    - name: Print app name
      run: echo "Building ${{ inputs.app_name }}"
      shell: bash

    - name: Run tests
      run: echo "Running tests"
      shell: bash
```

---

## 6. How to Use a Composite Action in a Workflow

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run common CI
        uses: ./.github/actions/common-ci
        with:
          app_name: my-app
```

---

## 7. Inputs in Composite Actions

Inputs make composite actions reusable.

```yaml
inputs:
  environment:
    type: string
    required: true
```

Used as:

```
${{ inputs.environment }}
```

---

## 8. Outputs in Composite Actions

Composite actions can expose outputs.

```yaml
outputs:
  result:
    value: ${{ steps.step1.outputs.result }}
```

Used by caller workflows for chaining logic.

---

## 9. Composite Actions vs Other Action Types

| Action Type | Description               |
| ----------- | ------------------------- |
| Composite   | YAML-based reusable steps |
| JavaScript  | Node.js-based logic       |
| Docker      | Runs inside container     |

---

## 10. Composite Actions vs Reusable Workflows

| Composite Action | Reusable Workflow      |
| ---------------- | ---------------------- |
| Reuses steps     | Reuses entire workflow |
| Used inside jobs | Used as a job          |
| Fine-grained     | High-level             |

---

## 11. Limitations of Composite Actions

* Cannot define jobs
* Cannot run on different runners
* No service containers

They are meant for **step reuse**, not full pipelines.

---

## 12. Production Best Practices

* Keep composite actions small and focused
* Version composite actions (`v1`, `v2`)
* Avoid hardcoding secrets
* Store shared actions in a central repo

---

