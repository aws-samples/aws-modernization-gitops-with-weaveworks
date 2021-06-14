+++
title = "Enable GitHub Access"
chapter = false
weight = 11
+++

In order for `eksctl` to automatically deploy Flux v2 in your cluster and enable GitOps at the time of cluster creation, it will need to have access to create and manage repositories in your GitHub account.

{{% notice info %}}
Flux v2 also support GitLab as a git service provider. Using GitLab and other supported providers is not covered in this workshop series.
{{% /notice %}}

Flux v2 will create a repository and store the entire configuration of your cluster in it, which will include your workload manifests, HelmReleases, etc.

{{% notice info %}}
You can also specify an existing repo in the cluster configuration file we will pass to `eksctl`, and Flux v2 will use it instead of creating a new one.
{{% /notice %}}

The main point of GitOps is to keep everything (config, alerts, dashboards, apps, literally everything) in Git and use it as a single source of truth. To keep your cluster configuration in Git, we will provide `eksctl` and Flux v2 a `personal access token` with enough permissions to create and configure the repository for us.

You can [follow these instructions](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) to create a Personal Access Token for your GitHub Account.

Your token will need to have the `repo` scope authorized only, no other scope is necessary.

![Personal Access Token required scope](/images/personal_token_scopes.png)

Note that once you've created this token, it will only be visible once. Store the token in your Cloud9 environment, so you can use it during the workshop.

```
echo "export GITHUB_TOKEN=<replace with your token>" > ~/github.env
```

{{% notice warning %}}
Your GitHub Personal Access Token allows anybody to authenticate as your account against GitHub and clone private repositories. Make sure you Delete the token you created for the workshop once you have completed it.  
{{% /notice %}}


