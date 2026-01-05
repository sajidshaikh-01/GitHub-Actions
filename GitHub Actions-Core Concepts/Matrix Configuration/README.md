# GitHub Actions – Matrix Configuration Explained & Production Usage

---

## 1. What is Matrix Configuration in GitHub Actions?

**Matrix configuration** allows you to run the **same job multiple times with different inputs** (like OS, language versions, environments) automatically.

Instead of writing multiple jobs, you define a **matrix of variables**, and GitHub Actions creates one job per combination.

In simple words:

> Matrix lets you test or run the same pipeline across multiple configurations in parallel.

---

## 2. Why Matrix Configuration is Important

Matrix is used to:

* Test applications on multiple versions
* Validate compatibility across environments
* Reduce YAML duplication
* Speed up CI by running jobs in parallel

---

## 3. Basic Matrix Example

### Example: Test on Multiple Node.js Versions

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

### What Happens Here

* Two jobs are created:

  * Node.js 18
  * Node.js 20
* Both run **in parallel**

---

## 4. Matrix with Multiple Dimensions

### Example: OS + Language Version

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    python-version: [3.10, 3.11]
```

This creates **4 jobs**:

* ubuntu + 3.10
* ubuntu + 3.11
* windows + 3.10
* windows + 3.11

---

## 5. Using `include` and `exclude` (Important ⭐)

### Exclude Combinations

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node-version: [18, 20]
    exclude:
      - os: windows-latest
        node-version: 18
```

### Include Custom Combinations

```yaml
strategy:
  matrix:
    os: [ubuntu-latest]
    node-version: [20]
    include:
      - os: ubuntu-latest
        node-version: 22
```

---

## 6. Matrix with Sequential Jobs (CI → CD)

Matrix is usually used in **CI**, not directly in production deployments.

### Example: CI with Matrix, CD without Matrix

```yaml
jobs:
  test:
    strategy:
      matrix:
        node-version: [18, 20]
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing on ${{ matrix.node-version }}"

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploy only once"
```

---

## 7. Matrix in Production (Real-World Usage)

### 7.1 Multi-Environment Validation

Used to:

* Validate Terraform plans
* Run smoke tests across environments

```yaml
matrix:
  environment: [dev, staging]
```

---

### 7.2 Multi-Region Infrastructure Checks

Used in:

* Terraform plan
* Cloud validation

```yaml
matrix:
  region: [us-east-1, eu-west-1]
```

---

### 7.3 Multi-Container or Microservice Testing

Used to:

* Test multiple services
* Run contract tests

```yaml
matrix:
  service: [auth, payment, orders]
```

---

## 8. Best Practices for Matrix in Production

* Use matrix mainly for **CI and validation**
* Avoid using matrix for **production deploy jobs**
* Combine with `fail-fast: false` if needed

```yaml
strategy:
  fail-fast: false
```

---

## 9. Matrix vs Separate Jobs

| Matrix             | Separate Jobs    |
| ------------------ | ---------------- |
| Less YAML          | More YAML        |
| Easy to scale      | Hard to maintain |
| Parallel execution | Manual control   |

---


