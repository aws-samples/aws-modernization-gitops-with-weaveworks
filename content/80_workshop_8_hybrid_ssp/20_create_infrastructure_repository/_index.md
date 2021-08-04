+++
title = "Create your SSP platform repository"
chapter = true
weight = 20
+++

# Create your SSP platform repository

Your infrastructure repository will be the central location for you to manage the teams that are deploying workloads across your different environments, as well as the core services you will make available to those teams as part of your shared services platform.

A sample repository is available at https://github.com/weaveworks-gitops-demo/team-ssp.git

In our scenario, we will have two sample teams:

* [**Team Blue**](https://github.com/weaveworks-gitops-demo/team-blue.git)
* [**Team Green**](https://github.com/weaveworks-gitops-demo/team-green.git).

Each team is using an individual repository and will have full freedom to deploy any workload within a specifically assigned namespace.

## Fork all sample repositories

Go to https://github.com/weaveworks-gitops-demo/team-ssp.git and make a copy of this repository to your account by clicking the `Fork` button.

Do the same for both Team Blue and Team Green repositories shown above. You should end up with three repos copied over to your GitHub Account.

## Create a GitHub Personal Access Token

Follow the instructions [here](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) and create a personal access token with `Repo` scope. Copy and paste the token (you will not be able to see it after it's just been created), and set it in your environment:

```shell
export GITHUB_TOKEN=<your token>
export GITHUB_USER=<your user>
```

## Clone all repositories to your Cloud9 IDE

You will need a local copy of all three repositories, use git to clone them:

```shell
git clone https://github.com/${GITHUB_USER}/team-ssp.git
git clone https://github.com/${GITHUB_USER}/team-green.git
git clone https://github.com/${GITHUB_USER}/team-blue.git
```

You should be able to see now the three directories in your Cloud9 IDE:


## SSP Infrastructure Walkthrough

This is the structure of the SSP repository, let's go over each directory and its purpose.

<pre>
.
├── README.md
├── environments
│   ├── dev
│   │   ├── blue-patches.yaml
│   │   ├── green-patches.yaml
│   │   └── kustomization.yaml
│   └── prod
│       ├── blue-patches.yaml
│       ├── green-patches.yaml
│       └── kustomization.yaml
├── platform
│   ├── core
│   │   └── ingress.yaml
│   └── team
│       ├── applications.yaml
│       ├── kustomization.yaml
│       ├── namespace.yaml
│       ├── repository.yaml
│       ├── role.yaml
│       ├── rolebinding.yaml
│       └── serviceaccount.yaml
└── teams
    ├── team-blue
    │   ├── kustomization.yaml
    │   └── patches.yaml
    └── team-green
        ├── kustomization.yaml
        └── patches.yaml
</pre>

The SSP Repository has three directories at its root:

* `environments/`: For each one of our clusters, we will have a subdirectory inside environments, in our case that means two subdirectories: `prod/` which will be tracked by our EKS Distro cluster, and `dev/` that will be tracked by our managed EKS cluster in the cloud. Each environment specific subdirectory will hold environment specific patches to the various base components we want to deploy on that specific target.
* `platform/`: In platform we have our reusable bases. The `core/` subdirectory is the place for us to place any resources we want available in the cluster for shared use by all teams, think for example `Ingress` controllers. The `team/` subdirectory holds all the resources that we will want to deploy for every team, we'll go over these in details shortly.
* `teams/`: This is where we declare our teams, by _kustomizing_ our `platform/team` base.