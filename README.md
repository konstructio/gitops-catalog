# gitops-catalog

The marketplace bounds to your GitOps desired state.

## Community-driven Cloud Native GitOps

The kubefirst gitops catalog repository is a community-driven series of cloud native apps that can be added onto the kubefirst platform easily once the platform has been provisioned.

## Contributing

To make a new application available for installation, you'll need to:

- Fork this repository.
- Add a new entry to the [index.yaml](index.yaml) file with:
  - **name**: application name as described in your YAML file.
  - **displayName**: name to be displayed in the GitOps catalog.
  - **website**: application website or GitHub repository.
  - **imageUrl**: full web URL for the application's logo. It will be displayed in the GitOps catalog. _For now, it needs to be located on a third-party server, but we'll update this field to grab them from the GitHub repository soon, so it doesn't depend on external URL._
  - description: an insightful description about your application. It will be displayed in the GitOps Catalog.
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

## Need help

As always, we are on our [Slack community](https://kubefirst.io/slack) in the #gitops-catalog channel  if you need any help. We also welcome any constructive feedback or feature suggestions.
