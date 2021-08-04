+++
title = "Create your SSP platform repository"
chapter = true
weight = 20
+++

# Create your SSP platform repository

Your infrastructure repository will be the central location for you to manage the teams that are deploying workloads across your different environments, as well as the core services you will make available to those teams as part of your shared services platform.

A sample repository is available at https://github.com/weaveworks-gitops-demo/team-ssp.git

In our scenario, we will have two teams:

* [**Team Blue**](https://github.com/weaveworks-gitops-demo/team-blue.git)
* [**Team Green**](https://github.com/weaveworks-gitops-demo/team-green.git).

Each team is using an individual repository and will have full freedom to deploy any workload within a specifically assigned namespace.

## Fork the sample SSP Team Repository

Go to https://github.com/weaveworks-gitops-demo/team-ssp.git and make a copy of this repository to your account by clicking the `Fork` button.



