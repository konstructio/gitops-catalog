<!-- markdownlint-disable MD041 -->
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
  - **categories**: one category amongst the following ones:
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
  - Add, and organize your Argo CD GitOps file(s) into it, if any.
- Create a pull request with the changes from your branch to our main branch.

### Acceptance Criteria

We will approve all GitOps Catalog application submissions as long as they are working with the latest version of kubefirst.

### Application Maintenance and Removal

We may remove an application from the GitOps Catalog with no notice if a severe vulnerability is discovered. Another reason for removing an application is if an application becomes abandoned or unmaintained by the upstream project.

### Testing

If you want to test the application you are adding to the catalog, you need to run a couple of things locally. The process is a bit complicated, so feel free to submit a pull request without all the local tests, and we'll happily do the testing for you. If you want to proceed by yourself, follow these steps.

Firstly, if it's not already done, you need to [create a fork](https://github.com/kubefirst/kubefirst-api/fork) of the gitops-catalog repository.

Secondly, you need to clone the [Kubefirst API](https://github.com/kubefirst/kubefirst-api/) repository locally, and edit the file `internal/gitShim/gitopsCatalog.go`:

1. Update the `KubefirstGitHubOrganization` constant with the organization or username on which you forked the gitops-catalog repository.
2. Update the `KubefirstGitopsCatalogRepository` constant if you change the repository name when you forked it into your account.

Once it's done, follow [the instructions from the README](https://github.com/kubefirst/kubefirst-api/#running-locally) to run the API locally.

Thirdly, you will also need to run the [console application](https://github.com/kubefirst/console) locally. To do so, clone the repository locally, and follow [the instructions from the README](https://github.com/kubefirst/console#setup-instructions). It will be the equivalent of using the CLI with the `launch up` command.

Lastly, you will need to create a new cluster like you would usually do, using the UI installation. Once your new cluster is created, you'll see the [GitOps Catalog](https://docs.kubefirst.io/civo/gitops-catalog) tab, and you should see your new application listed, and try to install it.

#### Debugging

If you need to refresh the GitOps catalog list of applications, instead of restarting the whole process, which can be cumbersome with the creation of a new cluster, you can connect to the MongoDB instance (using their [CLI](https://www.mongodb.com/docs/mongodb-shell/connect/) or [UI client](https://www.mongodb.com/docs/compass/current/connect/)), delete the `gitops-catalog` collection from the `api` database, and restart the API. You can now refresh the console browser tab, and you should see a new list. If you use the CLI, you can run `echo 'use api;\ndb.getCollection("gitops-catalog").drop();' | mongosh mongodb://<username>:<password>@<hostname>:<port>` in the terminal after replacing the `<username>`, `<password>`, `<hostname>`, and `<port>` with your database connection information.

If you already installed the application, to reinstall it another time without restarting the whole process, you'll need to follow these two steps:

1. Go into your `gitops` repository, the one created when you created your cluster from the console application. In the repository, go into the `registry` folder, followed by going into the folder named after your cluster name. You will need to delete the application YAML file inside this directory. Check also inside the `components` folder: if there is a folder with the name of the application you want to remove, delete it also. Once your changes are committed, Argo CD should sync, and remove the application. This is the usual process for removing an application from your cluster, GitOps catalog or not.
2. To be able to see the application as installable again in the catalog, you will need to update the `services` collection within the `api` database in MongoDB. You will need to remove the specific object associated with the application from the `services` array. If you use the CLI, you can run `echo 'use api;\ndb.services.updateOne({'cluster_name': "<clustername>" }, { $pull: { services: { name: "<appname>" } } } );' | mongosh mongodb://<username>:<password>@<hostname>:<port>` in the terminal after, as before, replacing the `<username>`, `<password>`, `<hostname>`, and `<port>` with your database connection information. You will also need to update the `<clustername>` with the name of the cluster you created, and the `<appname>` value in the query, with the name of the application to remove from the document.

### Need Help

As always, we are on our [Slack community](https://kubefirst.io/slack) in the #gitops-catalog channel if you need any help. We also welcome any constructive feedback, feature or application suggestions.
