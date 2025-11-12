# PBTest_GitOps

## Overview

Repository for learning and testing Learning, Testing and understanding Gitops

### Repositories

- **[PBTest_AutoSemantic](https://github.com/MTBonde/PBTest_AutoSemantic)**: Learn automatic semantic versioning with go-semantic-release
- **[PBTest_GitOps](https://github.com/MTBonde/PBTest_GitOps)** (this repo): Learn GitOps workflows and environment promotion
- **[PBTest_Infra](https://github.com/MTBonde/PBTest_Infra)**: Learn Infrastructure as Code with Terraform

## Prerequisites

- MicroK8s or kubernetes for docker desktop installed and running
- Docker installed
- GitHub account with repository access

Quick overview:
1. Install Flux CLI
2. Bootstrap Flux 
3. Configure Flux to watch repository

### Initial quick test with flux

```
flux bootstrap github \
  --owner=MTBonde \
  --repository=PBTest_GitOps \
  --branch=main \
  --path=clusters/dev \
  --personal
```

## The Complete Flow

```
Code Commit (PBTest_AutoSemantic)
    ↓
Semantic Release (v3.2.0)
    ↓
Docker Build (myapp:v3.2.0)
    ↓
GitOps Repo Update (this repo)
    ↓
Flux Detects Change
    ↓
Kubernetes Deployment (MicroK8s)
```

## Setup for Docker Desktop 

```bash
# Install Flux CLI (in WSL)
curl -s https://fluxcd.io/install.sh | sudo bash

# Bootstrap Flux to Docker Desktop Kubernetes
#works best with public repo and will ask for PAT!
flux bootstrap github \
  --context=docker-desktop \     # Use Docker Desktop Kubernetes cluster
  --owner=MTBonde \               # GitHub organization/user
  --repository=PBTest_GitOps \   # GitOps repository name
  --branch=main \                 # Branch to sync from
  --path=./manifests \            # Path in repo containing Kubernetes manifests
  --personal                      # Use personal GitHub account (not organization)

# Verify it works
flux check --context=docker-desktop
# Expected: All checks should pass with green checkmarks
# ✔ helm-controller: deployment ready
# ✔ kustomize-controller: deployment ready
# ✔ notification-controller: deployment ready
# ✔ source-controller: deployment ready

kubectl get pods -n hagi --context=docker-desktop
# Expected: hello-nginx pod running (if using test_deployment.yml)
# NAME                           READY   STATUS    RESTARTS   AGE
# hello-nginx-ff8dbc888-756jg    1/1     Running   0          2m
```