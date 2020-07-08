+++
title = "Set up git repo"
chapter = true
weight = 15
+++

# Setup git repo

* Create new repo Github called `aws-gitops-multicloud` and turn on "Initialize this repository with a README"
* Clone the repo on your machine
* Create two directories in the repo: `mkdir flux-mgmt` and `mkdir flux-ec2`

## Github SSH key

* Add ssh key to your Github account
  * On Cloud9, run: `ssh-keygen -t rsa -b 4096` and accept defaults
  * Run `cat ~/.ssh/id_rsa.pub` and copy the output
  * Go to [Github > Settings > SSH Keys > New SSH key](https://github.com/settings/ssh/new) and paste your public key

## Clone Workshop Repo to use examples

We'll use some examples from the below repo

* Clone the [workshop repo](https://github.com/saada/gitops-cluster-management)

```sh
git clone git@github.com:saada/gitops-cluster-management.git
```