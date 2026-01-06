# GitHub Actions – Cache Explained (Production Guide)

---

## 1. What is Cache in GitHub Actions?

**Cache** in GitHub Actions is a mechanism to **store and reuse files across workflow runs** to speed up pipelines.

Cache is mainly used for:

* Dependencies
* Tool downloads
* Build intermediates

In simple words:

> Cache avoids doing the same work again and again in CI/CD pipelines.

---

## 2. Why Cache is Important in Production

In production CI/CD environments:

* Pipelines run frequently
* Re-downloading tools and dependencies wastes time
* Slow pipelines reduce developer productivity

Cache helps by:

* Reducing pipeline execution time
* Reducing network usage
* Improving pipeline reliability

---

## 3. What Can Be Cached (Production Examples)

Common cache use cases:

| Category              | Examples                      |
| --------------------- | ----------------------------- |
| Language dependencies | Maven, Gradle, npm, pip       |
| Build tools           | Terraform plugins, Helm cache |
| Security tools        | Trivy DB cache                |
| Package managers      | apt, yum cache                |

---

## 4. What Should NOT Be Cached ⚠️

Do NOT cache:

* Secrets or credentials
* Dynamic build artifacts
* Temporary runtime files
* Database files

---

## 5. How Cache Works (Internals)

1. Workflow starts
2. GitHub checks if cache exists for the key
3. If cache exists → restore files
4. Job runs faster
5. Updated cache is saved at job end

---

## 6. Cache Key (Most Important Concept ⭐)

Cache is identified using a **key**.

Good cache key:

* Is deterministic
* Changes when underlying files change

### Example

```yaml
key: ${{ runner.os }}-tool-${{ hashFiles('**/*.lock') }}
```

---

## 7. Restore Keys (Fallback Mechanism)

Restore keys allow **partial cache matches**.

```yaml
restore-keys: |
  ${{ runner.os }}-tool-
```

Used when:

* Exact cache key not found

---

## 8. Using `actions/cache` (Generic Example)

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.cache/tool
    key: ${{ runner.os }}-tool-${{ hashFiles('**/*.lock') }}
    restore-keys: |
      ${{ runner.os }}-tool-
```

---

## 9. Cache vs Artifacts (Critical Difference)

| Cache                | Artifacts         |
| -------------------- | ----------------- |
| Speeds up workflows  | Shares files      |
| Reused across runs   | Used between jobs |
| Mutable              | Immutable         |
| Dependencies & tools | Build outputs     |

---

## 10. Cache Scope & Limits

* Cache is scoped to:

  * Repository
  * Branch
* Cache size limit exists (LRU eviction)

---

## 11. Cache in Real Production Pipelines

### Common Production Scenarios

* Cache Terraform plugins
* Cache Helm repository index
* Cache vulnerability DBs
* Cache language package managers

### Example (Generic Production Pattern)

```yaml
steps:
  - uses: actions/cache@v4
    with:
      path: ~/.cache
      key: ${{ runner.os }}-global-${{ github.ref_name }}
```

---

## 12. Common Mistakes (Interview Important ⚠️)

* ❌ Using static cache keys
* ❌ Caching large unnecessary directories
* ❌ Expecting cache to behave like artifacts

---

## 13. Best Practices (Production)

* Cache only what is expensive to download
* Keep cache size small
* Use lock files or hashes for keys
* Use restore keys wisely
* Monitor cache hit rate

---

