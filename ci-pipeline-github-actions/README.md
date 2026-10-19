# GitHub Actions CI Pipeline

## Project Overview

This project demonstrates a Continuous Integration (CI)
pipeline using GitHub Actions, Python Flask, pytest,
Docker, and Docker Hub.

## CI Pipeline

The workflow runs automatically on every push to the dev branch.

Pipeline steps:
1. Checkout source code
2. Set up Python
3. Install dependencies
4. Run automated tests
5. Authenticate to Docker Hub using GitHub Secrets
6. Build a Docker image
7. Push the image using the Git commit SHA as its tag

## Secrets Management

Docker Hub credentials are stored securely in GitHub
Actions repository secrets:

- DOCKER_USERNAME
- DOCKER_PASSWORD

Secrets are referenced by the workflow and are not
hardcoded into the repository.

## Best Practices Questions

### 1. Why should kubectl apply not be used in CI?

CI should focus on validating code, running tests,
building artifacts, and publishing them.

Running kubectl apply directly in CI mixes integration
and deployment responsibilities, requires Kubernetes
cluster credentials, and can cause uncontrolled changes.

Deployment should be handled separately through CD,
preferably using GitOps tools such as Argo CD or Flux.

### 2. Why is latest a bad Docker tag?

The latest tag is mutable and does not identify a
specific version of an application.

It makes debugging, reproducibility, and rollbacks harder.

Using a Git commit SHA provides traceability between
a Docker image and its source code.

### 3. What is the difference between CI and CD?

CI (Continuous Integration) automatically tests and
validates code changes.

CD can mean Continuous Delivery or Continuous Deployment.

Continuous Delivery ensures software is ready for release,
while Continuous Deployment automatically releases
validated changes to the target environment.

### 4. How does this pipeline support GitOps?

This pipeline produces versioned Docker images
associated with Git commits.

A GitOps process can update the image tag in Kubernetes
manifests or Helm values stored in Git.

Tools such as Argo CD or Flux can synchronize the
cluster with the desired state defined in Git.

This pipeline supports GitOps but does not perform
GitOps deployment itself.
