+++
title = "Build Model"
chapter = false
weight = 503
+++


## Install sbt

```sh
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java
sdk install sbt
```

## Build Spark Jars

```sh
sbt clean package
aws s3api put-object --bucket $BUCKET_NAME --key emr/titanic/titanic-survivors-prediction_2.11-1.0.jar --body target/scala-2.11/titanic-survivors-prediction_2.11-1.0.jar
```

> Note: EMR has all spark libariries and this project doesn't reply on third-party library. We don't need to build fat jars.

## The dataset

Check Kaggle [Titanic: Machine Learning from Disaster](https://www.kaggle.com/c/titanic) for more details about this problem. 70% training dataset is used to train model and rest 30% for validation.

A copy of train.csv is included in this repository, it needs to be uploaded to S3.

```sh
aws s3api put-object --bucket $BUCKET_NAME --key emr/titanic/train.csv --body train.csv
```
