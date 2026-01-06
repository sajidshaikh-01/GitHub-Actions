# GitHub Actions – Docker Composite Action Example 
---

## 1. What is a Docker Composite Action?

A **Docker composite action** is a **composite action (YAML-based)** that **orchestrates Docker-related steps** such as:

* Building Docker images
* Tagging images
* Pushing images to a registry

⚠️ Important clarification:

* A **composite action does NOT run its own Dockerfile**
* It **reuses Docker commands or existing Docker actions**

In simple words:

> Docker composite actions standardize Docker build & push logic using YAML.

---

## 2. Why Use a Docker Composite Action in Production?

In production teams:

* Every service builds Docker images
* Docker steps are mostly identical

Composite actions help by:

* Avoiding repeated Docker commands
* Enforcing standard image tags
* Centralizing Docker logic

---

## 3. Directory Structure (Standard)

```
.github/actions/docker-build-push/
├── action.yml
```

This action can be reused across **multiple repositories**.

---

## 4. Docker Composite Action – `action.yml`

```yaml
name: Docker Build & Push

description: Build and push Docker image using composite action

inputs:
  image_name:
    required: true
    description: Docker image name

  image_tag:
    required: true
    description: Docker image tag

runs:
  using: composite
  steps:
    - name: Login to Docker Registry
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Build Docker Image
      run: |
        docker build -t ${{ inputs.image_name }}:${{ inputs.image_tag }} .
      shell: bash

    - name: Push Docker Image
      run: |
        docker push ${{ inputs.image_name }}:${{ inputs.image_tag }}
      shell: bash
```

---

## 5. Explanation of Each Section

### Inputs

```yaml
inputs:
  image_name:
  image_tag:
```

* Makes the action reusable
* Different repos can pass different image names/tags

---

### Docker Login Step

```yaml
uses: docker/login-action@v3
```

* Authenticates to Docker registry
* Uses secrets (secure)
* Required before pushing images

---

### Build Step

```bash
docker build -t image:tag .
```

* Builds Docker image
* Uses repository Dockerfile

---

### Push Step

```bash
docker push image:tag
```

* Pushes image to registry
* Image is now ready for deployment

---

## 6. How to Use This Composite Action in a Workflow

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build & Push Image
        uses: ./.github/actions/docker-build-push
        with:
          image_name: sajid/app
          image_tag: v1.0.0
```

---

## 7. How This Is Used in Production (Real Pattern)

Production pattern:

1. CI builds Docker image using composite action
2. Image tag = commit SHA or version
3. Image pushed to registry
4. GitOps repo updated with new tag
5. Argo CD deploys

---

## 8. Why Composite Action (Not Docker Action)

| Composite Action  | Docker Action               |
| ----------------- | --------------------------- |
| Simple YAML       | Requires Dockerfile         |
| Easy to audit     | Harder to debug             |
| Faster startup    | Container startup overhead  |
| Most used in prod | Used only for special tools |

---

## 9. Production Best Practices

* Use composite actions for Docker workflows
* Version the action (`v1`, `v2`)
* Never hardcode credentials
* Use commit SHA as image tag

---

