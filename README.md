# ServiceExampleCI

CI/CD configuration for ServiceExample .NET application <https://github.com/vvitkovsky/ServiceExample>

## GitHub Actions Workflow

This repository contains a GitHub Actions workflow that builds, tests, and deploys the ServiceExample application.

### Workflow Features

- **Runs on**: Changes to `versionfile` pushed to the `main` branch
- **Steps**:
  1. Checks out the ServiceExample repository
  2. Restores .NET dependencies
  3. Builds the solution
  4. Runs unit tests (fails the workflow if tests fail)
  5. Reads version from `versionfile`
  6. Builds Docker image
  7. Pushes the image to Docker Hub with the version from `versionfile` and `latest` tag

### Required Configuration

#### Repository Variables

- `APPLICATION_URL`: The GitHub repository URL for ServiceExample
- `REGISTRY_URL`: The Docker registry URL

#### Repository Secrets

- `DOCKER_USERNAME`: Your Docker Hub username
- `DOCKER_PASSWORD`: Your Docker Hub access token

### Usage

To trigger a new build and deployment:

1. Update the version in `versionfile` (e.g., change `1.0.0` to `1.0.1`)
2. Commit and push to the `main` branch:

```bash
echo "1.0.1" > versionfile
git add versionfile
git commit -m "Bump version to 1.0.1"
git push origin main
```
