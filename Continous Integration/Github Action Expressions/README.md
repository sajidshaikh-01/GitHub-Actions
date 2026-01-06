# GitHub Actions – Expressions Explained 
---

## 1. What are GitHub Actions Expressions?

**Expressions** are used in GitHub Actions to **evaluate conditions, access variables, and perform logic** inside workflows.

They allow workflows to be **dynamic** instead of static.

In simple words:

> Expressions help GitHub Actions make decisions at runtime.

Expressions are written using the syntax:

```
${{ <expression> }}
```

---

## 2. Where Expressions Are Used

Expressions are commonly used in:

* `if:` conditions
* `env:` variables
* `with:` inputs
* `concurrency` groups
* Job outputs

---

## 3. GitHub Contexts (Very Important ⭐)

Expressions mainly work with **contexts**.

Common contexts:

| Context   | Purpose                    |
| --------- | -------------------------- |
| `github`  | Repo, branch, event info   |
| `env`     | Environment variables      |
| `secrets` | Encrypted secrets          |
| `matrix`  | Matrix values              |
| `needs`   | Outputs from previous jobs |
| `steps`   | Outputs from steps         |
| `inputs`  | workflow_dispatch inputs   |

---

## 4. Common Expression Examples

### 4.1 Access Branch Name

```yaml
${{ github.ref }}
${{ github.ref_name }}
```

---

### 4.2 Access Repository Information

```yaml
${{ github.repository }}
${{ github.actor }}
```

---

### 4.3 Use Secrets Safely

```yaml
${{ secrets.AWS_ACCESS_KEY_ID }}
```

---

### 4.4 Use Matrix Values

```yaml
${{ matrix.node-version }}
```

---

### 4.5 Access Job Outputs

```yaml
${{ needs.build.outputs.app_version }}
```

---

## 5. Expressions with `if:` Conditions

### Example: Run Job Only on Main Branch

```yaml
if: github.ref == 'refs/heads/main'
```

### Example: Combine Conditions

```yaml
if: github.ref == 'refs/heads/main' && github.event_name == 'push'
```

---

## 6. Expressions in Environment Variables

```yaml
env:
  ENVIRONMENT: ${{ github.ref_name }}
```

---

## 7. Expressions in `with:` Inputs

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: ${{ matrix.node-version }}
```

---

## 8. Expressions in Concurrency

```yaml
concurrency:
  group: deploy-${{ github.ref_name }}
  cancel-in-progress: true
```

---

## 9. Expression Operators

### Logical Operators

| Operator | Meaning    |   |    |
| -------- | ---------- | - | -- |
| `==`     | Equals     |   |    |
| `!=`     | Not equals |   |    |
| `&&`     | AND        |   |    |
| `        |            | ` | OR |
| `!`      | NOT        |   |    |

---

## 10. Useful Built-in Functions

| Function       | Use Case        |
| -------------- | --------------- |
| `contains()`   | Check substring |
| `startsWith()` | Prefix match    |
| `endsWith()`   | Suffix match    |
| `format()`     | Format strings  |
| `toJSON()`     | Debug values    |

### Example

```yaml
if: startsWith(github.ref, 'refs/tags/')
```

---

## 11. Expressions in Steps Output

### Set Output

```yaml
- name: Set version
  id: version
  run: echo "version=1.0.0" >> $GITHUB_OUTPUT
```

### Use Output

```yaml
${{ steps.version.outputs.version }}
```

---

## 12. Common Mistakes (Interview Important ⚠️)

* ❌ Using `${{ }}` inside `run` shell scripts unnecessarily
* ❌ Forgetting correct context names
* ❌ Using secrets directly in logs

---

