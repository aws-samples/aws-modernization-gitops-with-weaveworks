+++
title = "Set Up Your GIT Repository"
chapter = true
weight = 20
+++

The most important ingredient using eksctl enable repo is the configuration of your repository (which will include your workload manifests, HelmReleases, etc). You can start with an empty repository and push that to Git, or use the one you intend to deploy to the cluster.

The main point of GitOps is to keep everything (config, alerts, dashboards, apps, literally everything) in Git and use it as a single source of truth. To keep your cluster configuration in Git, please go ahead and create an empty repository. On GitHub, for example, [follow these steps](https://help.github.com/articles/create-a-repo).

{{% notice info %}}
In the **Cloud9** environment, a user SSH key pair is not automatically created. [Use these instructions](https://help.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys) to create an SSH key pair for use as your git repository deploy keys.
{{% /notice %}}

Open a new terminal in your Cloud9 workspace and run the following commands.

To create a new ssh key, run the following. Substitute your GitHub email address where specified. Use the default path suggested, this will make the key your default.

```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Once your new SSH key-pair is created, you will need to get the public pair of the key, and add it to the repository you created earlier. Run the following command and use the output as `Deploy key`, as indicated below. 

```bash
cat ~/.ssh/id_rsa.pub
```

You can then create a deploy key in your repository by going to: `Settings > Deploy keys`. Click on `Add deploy key`, and check `Allow write access`. Then paste your public key and click `Add key`. This will allow us to clone this repo to our Cloud9 environment. Instructions to do so [are here](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account).

Validate that the key is working as expected, by running the following command:

```bash
ssh -T git@github.com
```

You likely will need to add the host to `known_hosts`, and should see a `You've successfully authenticated` message from GitHub.