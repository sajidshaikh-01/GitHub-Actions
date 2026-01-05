# GitHub Actions – Architecture, Core Components & Jenkins Comparison

---

## 1. What is GitHub Actions?

GitHub Actions is a **CI/CD and automation platform** provided by GitHub that allows you to **build, test, secure, and deploy applications directly from a GitHub repository**.

It is **event-driven**, meaning workflows run automatically when something happens in the repository (for example: push, pull request, release, or manual trigger).

In simple terms:

> GitHub Actions automates software workflows directly inside GitHub.

---

## 2. GitHub Actions Architecture

### High-Level Architecture Flow

1. Developer pushes code or creates a pull request
2. An **event** triggers a workflow
3. GitHub Actions picks the workflow YAML file
4. A **runner** is assigned to execute the job
5. Jobs execute steps sequentially
6. Results are reported back to GitHub UI

### Key Architecture Elements

* **GitHub Repository** – Contains source code and workflow files
* **Workflow Engine** – Orchestrates jobs and steps
* **Event System** – Detects triggers like push or PR
* **Runners** – Machines that execute workflows
* **GitHub UI & Logs** – Shows execution status and logs

---

## 3. Core Components of GitHub Actions

### 3.1 Workflow

* A workflow is a **YAML file** stored in:
  `.github/workflows/`
* Defines **when** and **how** automation runs

Example:

```yaml
name: CI Pipeline
on: push
jobs:
  build:
    runs-on: ubuntu-latest
```

---

### 3.2 Events (Triggers)

Events decide **when a workflow runs**.

Common events:

* `push`
* `pull_request`
* `workflow_dispatch` (manual)
* `schedule` (cron)

Example:

```yaml
on:
  pull_request:
    branches: [ main ]
```

---

### 3.3 Jobs

* A workflow contains **one or more jobs**
* Jobs run **in parallel by default**
* Jobs can depend on other jobs using `needs`

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
  build:
    needs: test
```

---

### 3.4 Steps

* Steps run **sequentially inside a job**
* Each step can:

  * Run a shell command (`run`)
  * Use a predefined action (`uses`)

Example:

```yaml
steps:
  - uses: actions/checkout@v4
  - run: npm install
  - run: npm test
```

---

### 3.5 Actions

* Reusable units of automation
* Types of actions:

  * Official actions (maintained by GitHub)
  * Community actions
  * Custom actions

Example:

```yaml
- uses: actions/setup-node@v4
```

---

### 3.6 Runners

* Runners are **machines that execute jobs**

Types of runners:

* **GitHub-hosted runners**

  * ubuntu-latest
  * windows-latest
  * macos-latest

* **Self-hosted runners**

  * Used in production
  * Used for private networks, heavy workloads, compliance

Example:

```yaml
runs-on: ubuntu-latest
```

---

### 3.7 Secrets and Variables

#### Secrets

* Used for sensitive data
* Encrypted
* Stored at repository, environment, or organization level

Example:

```yaml
${{ secrets.AWS_ACCESS_KEY_ID }}
```

#### Variables

* Used for non-sensitive configuration values

---

### 3.8 Artifacts & Cache

#### Artifacts

* Store build outputs (logs, binaries, reports)

#### Cache

* Speeds up pipelines by reusing dependencies

---

## 4. GitHub Actions vs Jenkins

| Feature       | GitHub Actions          | Jenkins                       |
| ------------- | ----------------------- | ----------------------------- |
| Setup         | No setup required       | Requires server setup         |
| Hosting       | Managed by GitHub       | Self-hosted                   |
| Configuration | YAML                    | Groovy (Jenkinsfile)          |
| UI            | GitHub-native UI        | Jenkins Dashboard             |
| Scalability   | Automatic               | Manual scaling                |
| Maintenance   | GitHub managed          | User maintained               |
| Plugins       | Actions marketplace     | Large plugin ecosystem        |
| Best For      | GitHub-centric projects | Complex, enterprise pipelines |

---

## 5. GitHub Actions vs Jenkins (Interview Answer)

**GitHub Actions** is best when:

* Code is hosted on GitHub
* You want fast setup and native integration
* You follow GitOps and cloud-native practices

**Jenkins** is best when:

* You need highly customized pipelines
* You manage on-prem or hybrid infrastructure
* You require advanced pipeline control

---

