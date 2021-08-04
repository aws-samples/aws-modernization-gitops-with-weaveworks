+++
title = "Create your EKS-D Cluster"
chapter = true
weight = 10
+++

# Create your "production" EKS-Distro Cluster on EC2

To simulate the on-premises production environment, you will create an EKS-Distro cluster on Ubuntu running on an EC2 instance in AWS. We will use the default VPC automatically available in your Event Engine AWS Account.

## Create an SSH key

You will need to create a SSH key to be able to log into the virtual machine that will be used to simulate our EKS-D cluster. In your Cloud9 Environment, run the following command:

```shell
aws ec2 create-key-pair --key-name weaveworks-workshop --query 'KeyMaterial' --output text > ~/.ssh/id_rsa
chmod 0400 ~/.ssh/id_rsa
```

## Get the ID for the EKS security group, VPC and Subnet

To simplify access from your Cloud9 to both the EKS Cluster that was provisioned as part of your Event Engine configuration, as well as to the EKS-D Cluster you're about to create, we will use the same Security Group for everything. Let's get the security group ID for it:

```shell
export VPC_ID=$(curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/${INTERFACE_MAC}/vpc-id 2>&1 | grep vpc)
export SECURITY_GROUP_NAME=$(curl http://169.254.169.254/latest/meta-data/security-groups 2>&1 | grep eks)
export SECURITY_GROUP_ID=$(aws ec2 describe-security-groups --filter Name=vpc-id,Values=${VPC_ID} Name=group-name,Values=${SECURITY_GROUP_NAME} --query 'SecurityGroups[*].[GroupId]' --output text)
export INTERFACE_MAC=$(ifconfig eth0 | grep ether | awk '{print $2}')
export SUBNET_ID=$(curl http://169.254.169.254/latest/meta-data/network/interfaces/macs/${INTERFACE_MAC}/subnet-id 2>&1 | grep subnet)

```

## Create an EC2 Ubuntu Machine

In our real world scenario, our production environment would run in a dedicated on-premises cluster, with EKS-D for Kubernetes distribution.

It does not take trivial effort to set up a production grade cluster on VMs or bare metal, therefore, we will simulate our EKS-D production environment by running a minimal EKS-D instance on top of Ubuntu 20.04, using the convenient microk8s snap EKS-D installer.

The AMI ID you will need to use for your instance will vary depending on your region, get the official Canonical AMI ID for Ubuntu 20.04 with the following command.

```shell
export AMI_ID="ami-019212a8baeffb0fa"
```

Now, run one `c4.large` instance using the correct AMI and the SSH key created above.

```shell
aws ec2 run-instances --image-id $AMI_ID --count 1 --instance-type c4.large --key-name weaveworks-workshop --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=weaveworks-workshop}]' --security-group-ids $SECURITY_GROUP_ID --subnet-id $SUBNET_ID
```

The instance may take a few seconds to be ready for you to SSH into it, you can check the status in the EC2 Console.

Now lets SSH into the instance we just created.

```shell
export INSTANCE_IP=$(aws ec2 describe-instances --filters 'Name=tag:Name,Values=weaveworks-workshop' 'Name=instance-state-name,Values=running' | jq -r .Reservations[0].Instances[0].PrivateIpAddress)
ssh ubuntu@${INSTANCE_IP}
```

## Install EKS-D Using Snap

Now we're ready to install EKS-Distro in the EC2 instance. We're going to be using a MicroK8s based installation of EKS-D for sake of simplicity and speed.

```shell
sudo snap install kubectl --classic
sudo snap install eks --classic --edge
```

Once completed, you can check that the cluster is running by using the `eks` command:

```shell
sudo eks status --wait-ready
```

Last step is to get your `kubectl` configuration in place so you can interact with the cluster.

```shell
sudo usermod -a -G eks $USER
mkdir -p $HOME/.kube
sudo chown -f -R $USER ~/.kube
sudo eks config > .kube/config
```

To confirm you can communicate with the EKS-D cluster try `kubectl`:

```shell
kubectl get all -A
```

## Copy over the `kubectl config` file to your Cloud9 Environment

In order for you to manage the cluster from your Cloud9 Environment, you'll have to copy over the file. `exit` your SSH session and copy the configuration over.

```shell
mkdir -p ~/.kube
scp ubuntu@${INSTANCE_IP}:~/.kube/config ~/.kube/config
```

You should now be able to see your EKS-D Cluster from your Cloud9 IDE, running `kubectl get all -A` should show the same output as the last run of the command.