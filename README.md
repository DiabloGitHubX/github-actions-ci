# GitHub Actions CI Pipeline

## Overview

This project demonstrates a CI pipeline using GitHub Actions.

The pipeline runs automatically whenever code is pushed to the `dev` branch.

The pipeline:

1. Checks out the source code
2. Sets up Python
3. Installs dependencies
4. Runs automated tests
5. Logs into Docker Hub using GitHub Secrets
6. Builds a Docker image
7. Pushes the image to Docker Hub

## Application

The application is a simple Flask API.

Endpoint:

`GET /`

Response:

```text
Hello from CI/CD!
Project Structure
.
├── .github/
│   └── workflows/
│       └── ci.yaml
├── app.py
├── test_app.py
├── requirements.txt
├── Dockerfile
├── .gitignore
└── README.md
CI Workflow

The workflow is located at:

.github/workflows/ci.yaml

It runs on:

push:
  branches:
    - dev

The Docker image is tagged using the Git commit SHA:

<docker-username>/github-actions-ci:<commit-sha>

The pipeline does not use the latest tag.

GitHub Secrets

Docker Hub credentials are stored as GitHub repository secrets:

DOCKER_USERNAME
DOCKER_PASSWORD

The workflow references them using:

username: ${{ secrets.DOCKER_USERNAME }}
password: ${{ secrets.DOCKER_PASSWORD }}

Credentials are never hardcoded in the repository or printed in the workflow.

Testing

Tests are executed with pytest:

python -m pytest

The project currently contains one automated test verifying that the Flask / endpoint returns HTTP 200 and the expected response.

Docker

Build locally:

docker build -t ci-test .

Run:

docker run --rm -p 8080:8080 ci-test

Test:

curl http://localhost:8080

Expected response:

Hello from CI/CD!
Best Practices
1. Why should kubectl apply not be used in CI?

kubectl apply directly changes the Kubernetes cluster from the CI pipeline.

This tightly couples CI with deployment and can allow a build pipeline to make production changes.

A better approach is to separate CI and CD. CI should build and test immutable artifacts, while a CD or GitOps system handles deployment.

2. Why is latest a bad Docker tag?

latest is mutable.

The same tag can point to different images over time, making deployments difficult to reproduce and roll back.

Using the Git commit SHA creates an immutable reference to the exact version of the source code that produced the image.

For example:

github-actions-ci:a81c92f...

This makes the image traceable and reproducible.

3. What is the difference between CI and CD?

Continuous Integration (CI) automatically builds and tests code changes.

This project implements CI by:

Installing dependencies
Running tests
Building the Docker image
Publishing the image

Continuous Delivery/Deployment (CD) is responsible for delivering or deploying the tested artifact to an environment.

For example, CD could deploy the Docker image to Kubernetes.

4. How does this pipeline support GitOps?

The pipeline creates an immutable Docker image identified by the Git commit SHA.

A GitOps deployment system can then reference that exact image in Kubernetes manifests or Helm values.

Git remains the source of truth for the desired application state, while the CI pipeline produces the versioned artifact that the GitOps system deploys.

Result

The GitHub Actions workflow successfully completed:

Checkout
Python setup
Dependency installation
Automated tests
Docker authentication
Docker image build
Docker image push

The workflow completed successfully on the dev branch.


---