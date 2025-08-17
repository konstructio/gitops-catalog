# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

The GitOps Catalog is a community-driven collection of cloud native applications that can be easily added to the Kubefirst platform. Each application follows a structured format with Argo CD manifests and standardized metadata.

## Common Development Tasks

### Validation Commands

Run these commands before submitting a pull request:

```bash
# Validate YAML syntax
yamllint -c .yamllint.yml .

# Check for broken links in markdown files
# (Automated in CI, manual validation recommended)

# Verify Git commits are signed
git log --show-signature
```

### Adding a New Application

When adding a new application to the catalog, follow this structure:

1. Create directory: `<app-name>/`
2. Add App of Apps manifest: `<app-name>/<app-name>.yaml`
3. Create components directory: `<app-name>/components/<app-name>/`
4. Add Argo CD application manifest: `<app-name>/components/<app-name>/application.yaml`
5. Add logo: `logos/<app-name>.svg` (or .png/.jpeg)
6. Update `index.yaml` with application metadata

### Key Files to Update

- **index.yaml**: Central registry of all applications with metadata (name, displayName, website, imageUrl, description, category)
- **logos/**: Directory containing application logos (32x32 display size)
- **CONTRIBUTING.md**: Detailed instructions for contributors

## Architecture & Patterns

### Application Structure

Each application follows the "App of Apps" pattern:
- Root manifest (`<app-name>.yaml`) deploys to ArgoCD namespace
- Points to components folder in the GitOps repository
- Components contain the actual application manifests

### Token Substitution

Applications use Kubefirst tokens that get replaced at deployment time:
- `<CLUSTER_NAME>`: Kubernetes cluster name
- `<GITOPS_REPO_URL>`: GitOps repository URL
- `<REGISTRY_PATH>`: Path in the registry
- `<CLUSTER_DESTINATION>`: Target cluster
- `<PROJECT>`: ArgoCD project
- `<DOMAIN_NAME>`: Domain for ingress
- Full list in CONTRIBUTING.md

### Required Annotations

All Argo CD applications must include:
```yaml
annotations:
  kubefirst.konstruct.io/application-name: <appName>
  kubefirst.konstruct.io/source: catalog-templates
```

### Categories

Applications must belong to one of these categories:
- App management
- Architecture
- CI/CD
- Database
- End user application
- FinOps
- Infrastructure
- Miscellaneous
- Monitoring
- Observability
- Security
- Storage
- Testing

## Important Conventions

1. **Commit Signing**: All commits must be signed (enforced by CI)
2. **YAML Linting**: Follow `.yamllint.yml` rules (line-length disabled, truthy check-keys disabled)
3. **Logo Requirements**: SVG preferred, PNG/JPEG accepted, displayed at 32x32 pixels
4. **Character Limits**: displayName (120 chars max), description (200 chars max)
5. **Sync Wave**: Use annotation `argocd.argoproj.io/sync-wave: '100'` for proper ordering

## CI/CD Workflow

GitHub Actions validate:
- YAML syntax (yamllint)
- Markdown formatting
- Link validity
- Commit signatures (PRs only)

No build or deployment steps - this is a catalog of manifests that get deployed by Kubefirst platform.