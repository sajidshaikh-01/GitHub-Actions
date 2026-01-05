# GitHub Actions – Multiple Jobs, Sequential Execution & Data Sharing

---

## 1. Workflow with Multiple Jobs

A **workflow** can contain **multiple jobs**. Each job runs on its own runner.

By default:

* Jobs run **in parallel**
* To run jobs **in sequence**, we use the `needs` keyword

---

## 2. Execute Multiple Jobs in Sequence (Using `needs`)

### Example: Test → Build → Deploy (Sequential Execution)

```yaml
name: Multi-Job Sequential Workflow

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run tests
        run: echo "Running unit tests"

  build:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - name: Build application
        run: echo "Building application"

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy application
        run: echo "Deploying application"
```

### Explanation

* `test` job runs first
* `build` job waits for `test` to succeed
* `deploy` job waits for `build` to succeed

If any job fails, the next job **will not run**.

---

## 3. Why Jobs Cannot Share Files Directly

Important concept:

* Each job runs on a **separate runner**
* Runners are **isolated**
* Files created in one job are **not available** in another job automatically

To share data, we must use **outputs**, **artifacts**, or **environment variables**.

---

## 4. Sharing Data Between Jobs (Methods)

### Method 1: Job Outputs (Best for Small Data)

Use this when you want to pass:

* Version numbers
* Image tags
* Environment names

#### Example: Pass Version from Build Job to Deploy Job

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      app_version: ${{ steps.set_version.outputs.version }}
    steps:
      - name: Set version
        id: set_version
        run: echo "version=1.0.0" >> $GITHUB_OUTPUT

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Use version from build job
        run: echo "Deploying version ${{ needs.build.outputs.app_version }}"
```

### Explanation

* `outputs` expose data from one job
* `needs.<job_id>.outputs.<output_name>` is used to access it

---

### Method 2: Artifacts (Best for Files)

Use this when you want to share:

* Build artifacts
* Reports
* Compiled binaries

#### Upload Artifact

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: ./dist
```

#### Download Artifact in Another Job

```yaml
- uses: actions/download-artifact@v4
  with:
    name: build-output
```

---

### Full Example: Build Job → Deploy Job Using Artifacts

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Create build file
        run: echo "Hello World" > app.txt

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: app-file
          path: app.txt

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: app-file

      - name: Deploy
        run: cat app.txt
```

---

### Method 3: Environment Variables (Limited Scope)

Environment variables **do not persist across jobs** by default.

They are useful **only inside the same job**.

---

## 5. Which Method to Use in Production

| Use Case                     | Recommended Method    |
| ---------------------------- | --------------------- |
| Version / Tag / Small values | Job Outputs           |
| Files / Reports / Binaries   | Artifacts             |
| Same job steps               | Environment Variables |

---

