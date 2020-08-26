+++
title = "Set up git repo"
chapter = true
weight = 15
+++

# Set up Git repo

## Github SSH key

* Add ssh key to your Github account
  * On Cloud9, run: `ssh-keygen -t rsa -b 4096` and accept defaults
  * Run `cat ~/.ssh/id_rsa.pub` and copy the output
  * Go to [Github > Settings > SSH Keys > New SSH key](https://github.com/settings/ssh/new) and paste your public key

## Create git repo

* Create new repo Github called `aws-gitops-multicloud` and turn on "Initialize this repository with a README"
* Init the repo

```sh
mkdir aws-gitops-multicloud
cd aws-gitops-multicloud
echo "# aws-gitops-multicloud" >> README.md
git init
git add README.md
git commit -m "first commit"
```

* Push the changes. Make sure you set the <YOUR-REPO> url correctly

```sh
git remote add origin <YOUR-REPO>
git push -u origin master
```

* Create two directories in the repo: `mkdir flux-mgmt` and `mkdir flux-ec2`

## Clone Workshop Repo to use examples

We'll use some examples from the below repo

* Clone the [workshop repo](https://github.com/saada/gitops-cluster-management)

```sh
cd ..
git clone git@github.com:saada/gitops-cluster-management.git
```