# PBTest_GitOps

## Overview

Repository for learning and testing Learning, Testing and understanding Gitops

### Repositories

- **[PBTest_AutoSemantic](https://github.com/MTBonde/PBTest_AutoSemantic)**: Learn automatic semantic versioning with go-semantic-release
- **[PBTest_GitOps](https://github.com/MTBonde/PBTest_GitOps)** (this repo): Learn GitOps workflows and environment promotion
- **[PBTest_Infra](https://github.com/MTBonde/PBTest_Infra)**: Learn Infrastructure as Code with Terraform

## Prerequisites

- MicroK8s installed and running
- Docker installed
- GitHub account with repository access
- Completed Phase 1 (PBTest_AutoSemantic)

Quick overview:
1. Install Flux CLI
2. Bootstrap Flux on MicroK8s
3. Configure Flux to watch this repository
4. Deploy to dev and prod namespaces
5. Connect to Phase 1 for automatic deployments

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

# Initial quick test with flux

```
flux bootstrap github \
  --owner=MTBonde \
  --repository=PBTest_GitOps \
  --branch=main \
  --path=clusters/dev \
  --personal 
```