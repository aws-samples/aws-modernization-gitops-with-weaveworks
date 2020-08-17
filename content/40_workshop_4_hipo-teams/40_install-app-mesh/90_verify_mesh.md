+++
title = "Verify the Service Mesh is Active"
chapter = false
weight = 90
+++

When we installed the App Mesh Injector we asked it to create a new service mesh called **apps**.  A service mesh is a logical boundary for network traffic between the services that reside within it.

To check that the mesh has been created we can use the AWS CLI. Run the following command:

```bash
aws appmesh list-meshes
```

The output should be similar to below and you should see **apps** listed:

```json
{
    "meshes": [
        {
            "arn": "arn:aws:appmesh:us-west-2:1234567890:mesh/apps",
            "meshName": "apps",
            "meshOwner": "1234567890",
            "resourceOwner": "1234567890"
        }
    ]
}
```

You can also check by going to the AWS Console and choosing **AWS App Mesh** from the **Services** menu:

![aws_console_appmesh](/images/aws_console_appmesh.png)

And we need to check out out cluster by running:

```bash
kubectl get meshes -A
```

You should see output similar to:

```bash
NAME   AGE
apps   51m
```
