+++
title = "Build Pipeline"
chapter = false
weight = 503
+++

## Install 

See [building a pipeline](https://www.kubeflow.org/docs/guides/pipelines/build-pipeline/) to install the Kubeflow Pipelines SDK.
The following command will install the tools required

```sh
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
source /home/ec2-user/.bashrc
conda create --name mlpipeline python=3.7
pip3 install --user kfp --upgrade
rm Miniconda3-latest-Linux-x86_64.sh 
```

## Compiling the pipeline template

```sh
cd aws-titanic
sed s/mlops-kubeflow-pipeline-data/$BUCKET_NAME/g titanic-survival-prediction.py | sed s/aws-region/$EMR_REGION/ > build/titanic-survival-prediction.py
dsl-compile --py build/titanic-survival-prediction.py --output build/titanic-survival-prediction.tar.gz
aws s3api put-object --bucket $BUCKET_NAME --key emr/titanic/titanic-survival-prediction.tar.gz --body build/titanic-survival-prediction.tar.gz
```
