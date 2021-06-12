+++
title = "Manage your cluster using GitOps"
chapter = true
weight = 50
+++

# Modify the desired state of your cluster using GitOps

In this section, we will be modifying the desired state of our clusters, by adding Kubernetes manifests to your git repository. Each one of the manifests installs specific features and applications.

You can either edit these files directly in your git repository using the "Create File" function in the web user interface of your Git service provider of choice, or you can clone the repository into your **Cloud9** development environment, and edit them there.

If you use the `git clone` method, do not forget to `git add .` and `git commit -m "add commit message"` your changes, and `git push` them to the repository.

The software agent, `flux`, will check for new commits to the git repository, and apply any changes automatically to your cluster.

You must pay attention to the directory structure in the repository. When running the Flux CLI previously, you specified a `--path` option, this defines the root path where all manifests will be stored.

You will find a `flux-system/` sub-directory, this is where we will be adding the various manifests in the following sections.