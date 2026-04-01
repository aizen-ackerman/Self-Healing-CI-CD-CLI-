# Self-Healing CI/CD Pipeline using GitOps and Kubernetes

This directory contains a complete CLI-based DevOps project:

- Python Flask application
- Docker container build
- Kubernetes manifests
- Argo CD GitOps application
- Kind cluster bootstrap scripts
- Failure simulation and Git-based recovery scripts
- GitHub Actions workflow for CI/CD

## Structure

```text
.
├── .github/workflows/ci-cd.yaml
├── .tools/bin/
├── app/
│   ├── app.py
│   └── requirements.txt
├── argocd/
│   └── application.yaml
├── k8s/
│   ├── deployment.yaml
│   ├── namespace.yaml
│   └── service.yaml
├── kind/
│   └── cluster.yaml
├── scripts/
│   ├── bootstrap_local.sh
│   ├── common.sh
│   ├── configure_gitops.sh
│   ├── install_argocd.sh
│   ├── load_image_into_kind.sh
│   ├── recover.sh
│   ├── simulate_failure.sh
│   ├── start_local_git_remote.sh
│   └── status.sh
├── tests/
│   └── test_app.py
├── .dockerignore
├── .gitignore
├── Dockerfile
└── Makefile
```

## Quick Start

Run tests:

```bash
make test
```

Bootstrap a local Kind cluster, build the image, deploy the app, and install Argo CD:

```bash
make bootstrap-local
```

Check runtime status:

```bash
make status
```

## Configure GitOps Source

Point Argo CD at your Git repository and optionally set the published image:

```bash
./scripts/configure_gitops.sh https://github.com/USER/REPO.git docker.io/USER/self-healing-cicd:latest 1.0.0
```

## Self-Healing Demo

Introduce a broken image:

```bash
make break
git push origin main
```

Recover by Git revert:

```bash
make heal
git push origin main
```

## GitHub Actions Secrets

Add these repository secrets before using the workflow:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

## Expected Healthy Output

```json
{"message":"Self-healing CI/CD pipeline is running","status":"healthy","version":"1.0.0"}
```
