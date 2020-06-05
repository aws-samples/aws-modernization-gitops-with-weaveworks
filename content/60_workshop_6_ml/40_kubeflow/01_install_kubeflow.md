+++
title = "Deploy Kubeflow"
chapter = false
weight = 401
+++

Install the kubeflow:

```sh
cd $GITHUB_DIR/$GIT_ORG
wget https://github.com/kubeflow/kfctl/releases/download/v1.0.2/kfctl_v1.0.2-0-ga476281_linux.tar.gz
tar -zxvf kfctl_v1.0.2-0-ga476281_linux.tar.gz 
sudo mv kfctl /usr/bin
rm kfctl_v1.0.2-0-ga476281_linux.tar.gz 

export AWS_CLUSTER_NAME=$EKS_CLUSTER_NAME
export KF_NAME=${AWS_CLUSTER_NAME}
export BASE_DIR=$GITHUB_DIR/$GIT_ORG/kubeflow-config
export KF_DIR=${BASE_DIR}/${KF_NAME}
mkdir -p ${KF_DIR}
cd ${KF_DIR}
export CONFIG_URI=https://raw.githubusercontent.com/kubeflow/manifests/v1.0-branch/kfdef/kfctl_aws.v1.0.2.yaml
wget -O kfctl_aws.yaml $CONFIG_URI

grep -v eksctl kfctl_aws.yaml > kfctl.yaml
sed -i s/kubeflow-aws/${AWS_CLUSTER_NAME}/ kfctl.yaml  
sed -i s/roles:/enablePodIamPolicy\:\ true/ kfctl.yaml  
sed -i s/us-west-2/${AWS_DEFAULT_REGION}/ kfctl.yaml  
sed -i s/roles:/enablePodIamPolicy\:\ true/ kfctl.yaml

export CONFIG_FILE=${KF_DIR}/kfctl.yaml
kfctl apply -V -f ${CONFIG_FILE}
```

In a few minutes or so you will see deployments in 'kubeflow' namespace, verify using 'kubectl get all -n kubeflow'
