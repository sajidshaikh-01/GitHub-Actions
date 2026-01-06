# Reusable Workflows & Reporting in GitHub Actions

---

## 1. What are Reusable Workflows?

**Reusable workflows** allow you to define a complete workflow once and **reuse it across multiple repositories or workflows**.

They help teams:

* Avoid duplicating CI/CD logic
* Enforce standard pipelines
* Maintain changes in one central place

In simple words:

> Reusable workflows are shared CI/CD pipelines that can be called by other workflows.

---

## 2. Why Reusable Workflows Are Used in Production

In production environments:

* Multiple repositories follow the same CI/CD standards
* Security, scanning, and build steps must be consistent

Reusable workflows help by:

* Centralizing pipeline logic
* Reducing maintenance effort
* Improving reliability and governance

---

## 3. How Reusable Workflows Work (Architecture)

```
Caller Workflow (App Repo)
        │
        │ uses: org/repo/.github/workflows/ci.yml@v1
        ▼
Reusable Workflow (Central Repo)
        │
        ▼
Jobs Executed
```

---

## 4. Creating a Reusable Workflow

A reusable workflow must:

* Be inside `.github/workflows/`
* Use the `workflow_call` trigger

### Example: Reusable CI Workflow

```yaml
name: Reusable CI

on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Node version: ${{ inputs.node-version }}"
```

---

## 5. Calling a Reusable Workflow

A caller workflow uses the `uses` keyword.

### Example: Caller Workflow

```yaml
jobs:
  ci:
    uses: my-org/shared-workflows/.github/workflows/ci.yml@v1
    with:
      node-version: 20
```

---

## 6. Passing Data Between Caller and Reusable Workflow

### Inputs

* Used to customize behavior

```yaml
inputs:
  environment:
    type: string
```

### Outputs

* Used to return values back to caller

```yaml
outputs:
  image-tag:
    value: ${{ jobs.build.outputs.tag }}
```

---

## 7. What is Reporting in GitHub Actions?

**Reporting** refers to collecting and exposing results from CI/CD pipelines so teams can:

* See build/test status
* Review quality and security results
* Audit pipeline executions

In simple words:

> Reporting tells us what happened in the pipeline.

---

## 8. Types of Reporting in GitHub Actions

### 8.1 Workflow & Job Status Reporting

* Pass/Fail status shown in:

  * Pull Requests
  * GitHub Actions UI

This is the **default and most important report**.

---

### 8.2 Logs Reporting

* Every step produces logs
* Logs help in debugging failures

---

### 8.3 Artifacts (Most Common Reporting Method)

Artifacts are used to store:

* Test reports
* Coverage reports
* Scan results

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: reports/
```

---

### 8.4 Test & Coverage Reporting

Common examples:

* JUnit test results
* Code coverage reports

These reports are:

* Uploaded as artifacts
* Consumed by tools like SonarQube

---

## 9. Reporting with Reusable Workflows (Production Pattern)

### Centralized Reporting

Reusable workflows often:

* Run tests
* Generate reports
* Upload artifacts

All repositories get **consistent reporting**.

---

## 10. Example: Reusable Workflow with Reporting

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests"
      - run: mkdir reports && echo "ok" > reports/result.txt
      - uses: actions/upload-artifact@v4
        with:
          name: test-report
          path: reports/
```

---

## 11. How Teams Use This in Production

* One repo hosts reusable workflows
* All app repos call them
* Reports are uploaded in a standard format
* Teams review results via PR checks & artifacts

---

