+++
title = "Add YAMLs to Repo"
chapter = true
weight = 50
+++

# Add the YAML Files to your Repository

In this section, we will be adding Kubernetes manifests to your git repository. Each one of the manifests installs specific features and applications.

You can either edit these files directly in your git repository using the "Create File" function in the git repository web user interface, or you can clone the repository into one of your **Cloud9** terminal sessions, and edit them there. If you use the `git clone` method, do not forget to `git commit .` your changes, and `git push` them to the repository.

The software agent, `flux`, will check for new commits to the git repository, and apply the changes to the manifests automatically to each of your clusters.
