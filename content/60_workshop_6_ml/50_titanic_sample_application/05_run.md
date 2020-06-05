+++
title = "Run Pipeline"
chapter = false
weight = 505
+++

## Download the pipeline tar to workstation

Using the portforward access to the Kubeflow UI the upload from URL option does not work so it is necessary to download the file to your workstation.
A shell script is provided to generate the commands required.
```sh
./get-tar-cmds.sh
```

## Run experiment

Now use the kubeflow UI to upload the pipeline file and run an experiment.

## Check results

Open the Kubeflow pipelines UI. Create a new pipeline, and then upload the compiled specification (`.tar.gz` file) as a new pipeline template.

Once the pipeline done, you can go to the S3 path specified in `output` to check your prediction results. There're three columes, `PassengerId`, `prediction`, `Survived` (Ground True value)

```
...
4,1,1
5,0,0
6,0,0
7,0,0
...
```

Find the result file name:

```sh
aws s3api list-objects --bucket $BUCKET_NAME --prefix emr/titanic/output
```

Download it and analyse:

```sh
export RESULT_FILE=<result file>
aws s3api get-object --bucket $BUCKET_NAME --key emr/titanic/output/$RESULT_FILE $HOME/$RESULT_FILE
grep ",1,1\|,0,0" $HOME/$RESULT_FILE | wc -l # To count correct results
wc -l $RESULT_FILE # To count items in file
```
