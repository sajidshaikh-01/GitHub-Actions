# GitHub Actions – Conditions (`if:`) with Matrix Strategy

---

## 1. What are Conditions (`if:`) in GitHub Actions?

The `if:` keyword is used to **control whether a job or step should run** based on a condition.

Conditions help you:

* Skip unnecessary jobs
* Control deployments
* Apply logic on matrix combinations

In simple words:

> `if:` decides *whether something should execute or not*.

---

## 2. Why `if:` is Important with Matrix

Matrix creates **multiple job combinations**.

Using `if:` with matrix allows you to:

* Run jobs only for specific matrix values
* Avoid deploying multiple times
* Apply environment-specific logic

---

## 3. Basic `if:` with Matrix (Job Level)

### Example: Run Job Only for Node.js 20

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20]
    if: matrix.node-version == 20
    steps:
      - run: echo "Running only for Node 20"
```

### What Happens

* Job runs **only once**
* Node.js 18 job is skipped

---

## 4. `if:` at Step Level with Matrix

### Example: Conditional Step Execution

```yaml
steps:
  - run: echo "Common step"

  - name: Run only on Node 18
    if: matrix.node-version == 18
    run: echo "This runs only on Node 18"
```

---

## 5. Using `if:` with OS Matrix

### Example: OS-Specific Logic

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]

runs-on: ${{ matrix.os }}

steps:
  - name: Linux step
    if: matrix.os == 'ubuntu-latest'
    run: echo "Linux specific command"
```

---

## 6. Combining Matrix Conditions with GitHub Context

### Example: Run Matrix Job Only on Main Branch

```yaml
if: github.ref == 'refs/heads/main'
```

### Example: Matrix + Branch Condition

```yaml
if: github.ref == 'refs/heads/main' && matrix.node-version == 20
```

---

## 7. Real Production Pattern (CI + CD)

### Scenario

* Test on multiple versions (matrix)
* Deploy only once

### Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20]
    steps:
      - run: echo "Testing on ${{ matrix.node-version }}"

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying only once"
```

---

## 8. Common Mistakes (Interview Important ⚠️)

* ❌ Using matrix in deploy jobs
* ❌ Forgetting `if:` leading to multiple deployments
* ❌ Putting `if:` incorrectly at step vs job level

---
