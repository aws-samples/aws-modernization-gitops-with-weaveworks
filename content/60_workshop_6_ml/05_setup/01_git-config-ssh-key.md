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

The default disk size for a cloud9 instance is 10gb but we need more than this for the tools needed to build ML projects. So we need to increase the size of the EBS volume and grow the Linux filesytem. To do this use the aws console to locate the EBS volume used by your cloud9 instance and increase the size of the EBS volume from 10gb to 100gb

```bash
# Grow filesystem
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
```
