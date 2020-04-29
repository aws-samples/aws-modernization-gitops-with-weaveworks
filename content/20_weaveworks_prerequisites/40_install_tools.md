---
title: "Install Pre-requisite Tools"
chapter: false
weight: 40

---

We have provided some simple UNIX shell scripts for this workshop that will automate some of the installation and configuration steps.  You are welcome to explore these scripts if you wish.

1. In the AWS UNIX terminal, First clone this repo that contains the setup scripts from the home directory

    ```
    cd ~
    git clone https://github.com/robertjahn/dynatrace-k8s.git
    ```

1. Run the **install-workshop-tools** script that will:

    * Install [eksctl](https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz) EKS CLI
    * Install [jq](https://stedolan.github.io/jq) json query utility
    * Install [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) command line tool for Kubernetes
    * Install [aws-iam-authenicator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html that allows IAM to provide authentication to your Kubernetes cluster

    ```
    cd ~/dynatrace-k8s
    ./install-workshop-tools.sh
    ```

1. Verify

    Run this command to verify you are using aws cli version 2.  You should see **aws-cli/2.x.y** in the output

    ```
    aws --version
    ```
    If the version is not 2.x, <span style="color: red;">**DO NOT PROCEED**</span>. Go back and confirm the steps on this page.