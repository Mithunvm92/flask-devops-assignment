# Flask CI/CD DevOps Assignment

## Overview

This project demonstrates CI/CD automation for a Python Flask application using **Jenkins** and **GitHub Actions**.

The application is based on the provided Flask practice repository and is configured with automated testing, application build, staging deployment, and production deployment workflows.

**Repository:** https://github.com/Mithunvm92/flask-devops-assignment

---

# Assignment 1 – Jenkins CI/CD Pipeline

## Objective

Build a Jenkins CI/CD pipeline that automates:

1. Installing Python application dependencies.
2. Running automated tests using `pytest`.
3. Deploying the application to staging after successful tests.
4. Triggering a new build when changes are pushed to the `main` branch.
5. Providing build success/failure notification support.

## Jenkins Pipeline

The Jenkins pipeline is defined in:

```text
Jenkinsfile
```

### Pipeline Stages

```text
Build
  ↓
Test
  ↓
Deploy to Staging
```

### Build Stage

The Build stage:

- Creates a Python virtual environment.
- Upgrades `pip`.
- Installs dependencies from `requirements.txt`.
- Installs `pytest`.

### Test Stage

The Test stage runs:

```bash
pytest -v
```

Deployment proceeds only when the tests pass.

### Deploy to Staging

The staging deployment creates a staging directory and copies:

```text
app.py
requirements.txt
templates/
```

---

# Assignment 2 – GitHub Actions CI/CD

## Objective

Implement a GitHub Actions CI/CD workflow that:

1. Supports `main` and `staging` branches.
2. Installs application dependencies.
3. Runs automated tests using `pytest`.
4. Builds the application after successful tests.
5. Deploys to staging when code is pushed to `staging`.
6. Deploys to production when a GitHub Release is published.

## Branches

The repository contains:

```text
main
staging
```

### Staging Flow

```text
Push to staging
      ↓
Install & Test
      ↓
Build Application
      ↓
Deploy to Staging
```

### Production Flow

```text
Create GitHub Release
        ↓
Install & Test
        ↓
Build Application
        ↓
Deploy to Production
```

## GitHub Actions Workflow

The workflow is located at:

```text
.github/workflows/flask-ci-cd.yml
```

## Workflow Jobs

### 1. Install and Test

The workflow:

- Checks out the source code.
- Sets up Python 3.11.
- Starts a MongoDB 7 service container.
- Configures the test MongoDB URI.
- Installs Python dependencies.
- Runs the Flask test suite using `pytest`.

Test MongoDB URI:

```text
mongodb://localhost:27017/test_student_db
```

The application MongoDB configuration handles local MongoDB without TLS while retaining the certificate configuration for remote MongoDB connections.

### 2. Build Application

The build job runs only after the test job succeeds.

The application package contains:

```text
app.py
requirements.txt
templates/
```

The package is uploaded as a GitHub Actions artifact named:

```text
flask-application
```

### 3. Deploy to Staging

The staging deployment runs when the workflow is triggered by a push to:

```text
staging
```

The build artifact is downloaded and deployed to the staging environment.

### 4. Deploy to Production

The production deployment runs when a GitHub Release is published.

Example release:

```text
v1.0.1
```

The production job downloads the build artifact and performs the production deployment.

---

# Testing

The application contains automated tests in:

```text
test_app.py
```

The test suite covers:

- Home page loading.
- Adding a student.
- Updating a student.
- Deleting a student.

Tests are executed using:

```bash
pytest -v
```

---

# Project Structure

```text
flask-devops-assignment/
│
├── app.py
├── test_app.py
├── requirements.txt
├── Jenkinsfile
├── README.md
│
├── .github/
│   └── workflows/
│       └── flask-ci-cd.yml
│
├── templates/
│   └── ...
│
└── screenshots/
    ├── 01-repository.png
    ├── 02-local-tests.png
    ├── 03-jenkins-pipeline.png
    ├── 04-jenkins-build.png
    ├── 05-jenkins-test.png
    ├── 06-jenkins-deploy.png
    ├── 07-github-actions.png
    ├── 08-github-actions-test.png
    ├── 09-github-actions-staging.png
    └── 10-github-actions-production.png
```

> Jenkins screenshots should be added after completing the Jenkins pipeline execution. GitHub Actions screenshots document the successful CI/CD runs.

---

# GitHub Actions Execution Evidence

## Staging Deployment

The staging workflow was successfully executed from the `staging` branch.

### Successful Stages

```text
Install and Test       ✓
Build Application      ✓
Deploy to Staging      ✓
Deploy to Production   Skipped
```

![GitHub Actions Staging Deployment](screenshots/07-github-actions.png)

## Production Deployment

A GitHub Release was created using tag:

```text
v1.0.1
```

The release successfully triggered the production workflow.

### Successful Stages

```text
Install and Test          ✓
Build Application         ✓
Deploy to Production      ✓
Deploy to Staging         Skipped
```

![GitHub Actions Production Deployment](screenshots/10-github-actions-production.png)

---

# CI/CD Workflow Summary

| Event | Test | Build | Staging | Production |
|---|---:|---:|---:|---:|
| Push to `staging` | ✓ | ✓ | ✓ | - |
| Push to `main` | ✓ | ✓ | - | - |
| GitHub Release | ✓ | ✓ | - | ✓ |

---

# GitHub Actions Configuration

The workflow is configured to trigger for:

```yaml
on:
  push:
    branches:
      - main
      - staging
  release:
    types:
      - published
```

This provides separate CI/CD paths for normal development, staging deployment, and production releases.

---

# GitHub Secrets and Deployment Configuration

For a real remote deployment environment, sensitive deployment credentials such as SSH keys, tokens, passwords, and API credentials should be stored in **GitHub Repository Secrets** rather than committed to the repository.

Example secret categories:

```text
DEPLOY_HOST
DEPLOY_USER
DEPLOY_SSH_KEY
DEPLOY_TOKEN
```

No sensitive credentials should be stored directly in the workflow YAML or source code.

---

# Technologies Used

- Python
- Flask
- Flask-PyMongo
- MongoDB
- Pytest
- Jenkins
- Jenkins Pipeline
- GitHub Actions
- Git
- GitHub
- Linux
- Docker / GitHub Actions service containers

---

# Assignment Deliverables

## Assignment 1 – Jenkins

- [x] Working GitHub repository
- [x] `Jenkinsfile`
- [x] Build stage
- [x] Test stage
- [x] Staging deployment stage
- [ ] Jenkins build/test/deployment screenshots
- [ ] Jenkins email notification configuration evidence

## Assignment 2 – GitHub Actions

- [x] `main` branch
- [x] `staging` branch
- [x] `.github/workflows/flask-ci-cd.yml`
- [x] Dependency installation
- [x] Pytest execution
- [x] Application build
- [x] Staging deployment
- [x] Production deployment on GitHub Release
- [x] Successful staging workflow evidence
- [x] Successful production workflow evidence
- [x] Release tag `v1.0.1`

---

# Conclusion

This project demonstrates a complete CI/CD workflow for a Flask application using both Jenkins and GitHub Actions.

The GitHub Actions implementation validates the application, builds an artifact, deploys changes to staging from the `staging` branch, and deploys a released version to production through a GitHub Release.
