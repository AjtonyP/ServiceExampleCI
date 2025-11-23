# ServiceExampleCI

CI/CD configuration for ServiceExample .NET application <https://github.com/vvitkovsky/ServiceExample>

## Overview

This repository orchestrates the automated deployment pipeline:

1. Tests and builds ServiceExample application
2. Publishes Docker images to Docker Hub
3. Triggers Helm chart updates in ServiceExampleHelm repository

## GitHub Actions Workflow

### Workflow Features

- **Triggers on**: Changes to `versionfile` pushed to the `main` branch
- **Steps**:
  1. Checks out the ServiceExample repository
  2. Sets up .NET 9.0 environment
  3. Restores dependencies
  4. Builds the solution
  5. Runs unit tests (fails if tests don't pass)
  6. Reads version from `versionfile`
  7. Builds Docker image with Buildx (multi-arch support)
  8. Pushes image to Docker Hub with version tag and `latest`
  9. Triggers ServiceExampleHelm repository to update chart version

### Required Configuration

#### Repository Variables

Configure in Settings → Secrets and variables → Actions → Variables:

- **APPLICATION_URL**: The GitHub repository for ServiceExample
- **REGISTRY_URL**: The Docker registry URL (use `docker.io`)

#### Repository Secrets

- **DOCKER_USERNAME**: Your Docker Hub username
- **DOCKER_PASSWORD**: Docker Hub Personal Access Token with **Read & Write** permissions
  - Create at: <https://hub.docker.com/settings/security>
- **HELM_REPO_PAT**: GitHub Personal Access Token with `repo` scope
  - Used to trigger workflow in ServiceExampleHelm repository
  - Create at: <https://github.com/settings/tokens>

### Usage

To trigger a new build and deployment:

1. Update the version in `versionfile`:

```bash
echo "1.0.1" > versionfile
```

2. Commit and push to the `main` branch:

```bash
git add versionfile
git commit -m "Release version 1.0.1"
git push origin main
```

3. The automated pipeline will:
   - Run tests and build Docker image
   - Push to `ajtonyp/serviceexample:1.0.1` and `ajtonyp/serviceexample:latest`
   - Trigger Helm chart update
   - ServiceExampleHelm will publish new chart version

## Troubleshooting

### Workflow Not Running

- Verify you changed `versionfile` (not other files)
- Check you pushed to `main` branch
- Ensure GitHub Actions is enabled

### Docker Push Failed

- Verify `DOCKER_USERNAME` and `DOCKER_PASSWORD` are set correctly
- Ensure Docker Hub token has Read & Write permissions
- Check token hasn't expired

### Helm Trigger Failed

- Verify `HELM_REPO_PAT` secret is set
- Ensure token has `repo` scope
- Check ServiceExampleHelm has `update-chart.yml` workflow
- Verify token hasn't expired

## Related Repositories

- **ServiceExample**: Application source code
- **ServiceExampleHelm**: Helm chart and release automation
