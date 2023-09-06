<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="img/kubefirst-light.svg" alt="Kubefirst Logo">
    <img alt="" src="img/kubefirst.svg">
  </picture>
</p>

# Community-driven Cloud Native GitOps

The kubefirst GitOps Catalog repository is a community-driven series of cloud native apps that can be added onto the kubefirst platform easily once the platform has been provisioned.

## Contributing

To make a new application available for installation, you'll need to:

- Fork this repository.
- Add a new entry to the [index.yaml](index.yaml) file with:
  - **name**: application name as described in your YAML file.
  - **displayName**: name to be displayed in the GitOps catalog (120 characters maximum).
  - **website**: application website or GitHub repository.
  - **imageUrl**: full web URL for the application's logo. It will be displayed in the GitOps catalog. _For now, it needs to be located on a third-party server, but we'll update this field to grab them from the GitHub repository soon, so it doesn't depend on external URL._
  - **description**: an insightful description about your application. It will be displayed in the GitOps Catalog (200 characters maximum).
  - **categories**: one category amongts the following ones:
    - App management
    - Architecture
    - CI/CD
    - Database
    - FinOps
    - Infrastructure
    - Monitoring
    - Observability
    - Security
    - Storage
    - Testing
- Create a new directory with your new application's name in your fork.
  - Add, and organize your Argo CD gitops file(s) into it, if any.
- Create a pull request with the changes from your branch to our main branch.

### Acceptance Criteria

We will approve all GitOps Catalog application submissions as long as they are working with the latest version of kubefirst.

### Application Maintenance and Removal

We may remove an application from the GitOps Catalog with no notice if a severe vulnerability is discovered. Another reason for removing an application is if an application becomes abandoned or unmaintained by the upstream project.

### Need Help

As always, we are on our [Slack community](https://kubefirst.io/slack) in the #gitops-catalog channel  if you need any help. We also welcome any constructive feedback or feature suggestions.
