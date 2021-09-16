+++
title = "Spin up infrastructure"
chapter = true
weight = 60
+++

# Spin up your infrastructure

With a multi-stage shared services platform, your aim is to be able to create and operate multiple environments, one for each of the stages your development team usually needs in the software development life cycle (SDLC).

For most teams, the minimum stages for a complete development lifecycle are:

`development -> staging -> production`

For this workshop, we are going to provision only two of those (`development` and `production`) but you will see how easy it is to create additional stages.

## Terraform Codebase

Let's dig into the `multistage-ssp` [repository](https://github.com/weaveworks-gitops-demo/multistage-ssp) you have already cloned to your Cloud9 Development Environment.

In the `terraform/ssp` directory you will find a terraform module, that provisions the following resources:

- A VPC and all required related resources such as subnets, internet gateway, route tables, etc.
- A EKS Cluster
- Weave GitOps Core and the required configurations to track your SSP repository.

You will use that module once for every stage you want to provision, and declare each stage in `/main.tf` in the root of the repo:

```hcl
module "dev_stage" {
  source = "./terraform/ssp"
  region  = var.region
  stage  = "development"
  owner  = var.owner
  vpc_cidr  = var.vpc_cidr
  private_subnet_cidrs  = var.private_subnet_cidrs
  public_subnet_cidrs  = var.public_subnet_cidrs
  availability_zones  = var.availability_zones
  kubernetes_version  = var.kubernetes_version
  instance_type  = var.development_instance_type
  asg_max_size  = var.development_asg_max_size
  git_repository  = "ssh://git@github.com/${var.repository_owner}/${var.repository_name}"
  private_key  = file("./key.pem")
  known_hosts  = file("./known_hosts")
  path  = "./environments/prod"
  branch  = "main"
}
```

Adding more stages requires adding additional instances of the module, as shown above, one for every stage you want to create.

## Configure your environment

You will find a sample `terraform.tfvars` file which holds various parameters used to define the configuration of your SSP environments. Open the file in your Cloud9 IDE and uncomment `repository_owner` and `repository_name` and define their values to match your GitHub username and the [name of the repository you created in the previous step](), respectively.

## Initialize terraform

We need terraform to fetch the various modules we will use to spin up our multiple stages:

```shell
terraform init
```

## Let's create our infrastructure

Now let's have terraform create our infrastructure, and automatically bootstrap our clusters with our Shared Services Repository.

```shell
terraform apply
```