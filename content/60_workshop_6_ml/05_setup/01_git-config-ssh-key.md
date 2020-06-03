+++
title = "Cloud9 setup"
chapter = false
weight = 501
+++

## Git setup

You need to create and clone github repositories during this workshop so it is recommended that you setup your git config and ssh keys.

```sh
vim ~/.gitconfig # Add your config
vim ~/.ssh/id_rsa # Add private key
chmod 600 ~/.ssh/id_rsa
vim ~/.ssh/id_rsa.pub # Add your public key
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
ssh-add
eval `ssh-agent`
 ssh-add
```

## Extend FileSystem size

The default disk size for a cloud9 instance is 10gb but we need more than this for the tools needed to build ML projects. So we need to increase the size of the EBS volume and grow the Linux filesytem. To do this use the aws console to locate the EC2 instance by clicking on EBS volume used by your cloud9 instance and increase the size of the EBS volume from 10gb to 100gb

- Click the **A** button next to the `Share` button in the upper right hand corner of your Cloud9 workspace
- Click **Manage EC2 Instance**
- Make sure your `aws-cloud9-*` instance is selected

Locate the root filesystem

![ebs1](/images/ebs1.png)

Click on it

![ebs2](/images/ebs-mod.png)

Click on EBS ID value to open ebs volume in console

On **Actions** pull down, select **Modify Volume** and change the size from 10gb to 100gb

Then in cloud9 terminal:

```bash
# Grow filesystem
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        483M   60K  483M   1% /dev
tmpfs           493M     0  493M   0% /dev/shm
/dev/xvda1       99G  7.6G   91G   8% /
```
