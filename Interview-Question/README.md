# GitHub Actions – Interview Questions & Mock Interview (Production Focused)

---

## PART 1: GitHub Actions – Interview Questions & Answers

### 1. What is GitHub Actions?

**Answer:**
GitHub Actions is a CI/CD and automation platform that allows you to build, test, scan, and deploy applications directly from GitHub repositories using event-driven workflows.

---

### 2. What are the core components of GitHub Actions?

**Answer:**
The core components are:

* Workflows
* Events (triggers)
* Jobs
* Steps
* Actions
* Runners

---

### 3. Where are GitHub Actions workflows stored?

**Answer:**
Workflows must be stored in the `.github/workflows/` directory at the root of the repository.

---

### 4. What is the difference between a workflow and an action?

**Answer:**
A workflow defines the entire CI/CD pipeline, while an action is a reusable unit that performs a specific task inside a workflow.

---

### 5. What are runners in GitHub Actions?

**Answer:**
Runners are machines that execute GitHub Actions jobs. They can be GitHub-hosted or self-hosted.

---

### 6. Which runner is mostly used in production?

**Answer:**
Self-hosted runners are mostly used in production, especially for deployments and infrastructure changes.

---

### 7. What is the difference between GitHub-hosted and self-hosted runners?

**Answer:**
GitHub-hosted runners are fully managed by GitHub and are best for CI, while self-hosted runners are managed by organizations and are used for secure production deployments.

---

### 8. What is a job and how do jobs run by default?

**Answer:**
A job is a set of steps executed on the same runner. Jobs run in parallel by default unless controlled using `needs`.

---

### 9. How do you run jobs sequentially?

**Answer:**
By using the `needs` keyword to define job dependencies.

---

### 10. How do you share data between jobs?

**Answer:**
By using job outputs for small values and artifacts for files.

---

### 11. What is a matrix strategy?

**Answer:**
Matrix strategy allows running the same job multiple times with different configurations like OS or language versions.

---

### 12. Do we use matrix for production deployments?

**Answer:**
No, matrix is mainly used for CI testing and validation, not for production deployments.

---

### 13. What is job concurrency?

**Answer:**
Job concurrency ensures that only one workflow or job runs at a time for a specific concurrency group, preventing conflicts.

---

### 14. What are reusable workflows?

**Answer:**
Reusable workflows allow sharing complete CI/CD pipelines across repositories using the `workflow_call` trigger.

---

### 15. How are reusable workflows different from composite actions?

**Answer:**
Reusable workflows reuse entire pipelines, while composite actions reuse steps within a job.

---

### 16. What are custom actions?

**Answer:**
Custom actions are user-defined reusable automation units created when existing actions are insufficient.

---

### 17. Which custom action type is most used in production?

**Answer:**
Composite actions are most commonly used in production.

---

### 18. What is caching in GitHub Actions?

**Answer:**
Caching stores reusable files across workflow runs to speed up pipelines, mainly used in CI.

---

### 19. Do we use cache in production?

**Answer:**
Yes, cache is used in production CI pipelines for speed, but not in deployment or runtime stages.

---

### 20. What is the difference between cache and artifacts?

**Answer:**
Cache speeds up workflows across runs, while artifacts are used to share files between jobs.

---

### 21. What is Continuous Integration in GitHub Actions?

**Answer:**
CI automatically builds, tests, and validates every code change before it is merged.

---

### 22. What tools are commonly used in CI with GitHub Actions?

**Answer:**
Trivy, OWASP Dependency Check, and SonarQube are commonly used for security and quality scanning.

---

### 23. What is Continuous Deployment using GitHub Actions?

**Answer:**
CD automates the deployment of CI-validated artifacts to target environments using controlled workflows.

---

### 24. Why is GitOps preferred over direct deployment from GitHub Actions?

**Answer:**
GitOps is more secure because CI pipelines do not need cluster credentials; Argo CD pulls changes from Git.

---

### 25. Does GitHub Actions need kubeconfig when using Argo CD?

**Answer:**
No, Argo CD handles cluster access in a GitOps model.

---

## PART 2: Mock Interview – Real Interviewer Style Q&A

---

### Interviewer: Explain GitHub Actions in simple terms.

**Candidate:** GitHub Actions is GitHub’s native CI/CD tool that automates build, test, security scanning, and deployment workflows based on repository events.

---

### Interviewer: How do you structure CI and CD using GitHub Actions in production?

**Candidate:** CI handles build, test, and security scans using GitHub-hosted runners, while CD is either controlled with self-hosted runners or delegated to GitOps tools like Argo CD.

---

### Interviewer: How do you prevent multiple production deployments from running at the same time?

**Candidate:** By using job concurrency with a production-specific concurrency group and environment approvals.

---

### Interviewer: How do you securely deploy to Kubernetes from GitHub Actions?

**Candidate:** For direct deployments, we use secure credentials or OIDC with self-hosted runners. For GitOps, GitHub Actions does not access the cluster; Argo CD handles deployment.

---

### Interviewer: Where do you run security scans and why?

**Candidate:** Security scans are run in the CI stage to fail fast and prevent vulnerable code from reaching production.

---

### Interviewer: What happens if a GitHub Actions workflow fails?

**Candidate:** The job stops, the workflow is marked failed, and feedback is immediately visible in pull requests and logs.

---

### Interviewer: How do you standardize CI across multiple repositories?

**Candidate:** By using reusable workflows and composite actions maintained in a central repository.

---

### Interviewer: Why do companies move from Jenkins to GitHub Actions?

**Candidate:** GitHub Actions reduces operational overhead, integrates natively with GitHub, and supports modern GitOps and cloud-native workflows.

---

### Interviewer: What is your biggest production learning with GitHub Actions?

**Candidate:** CI should be fast and fail early, while CD should be controlled, secure, and auditable.

---

