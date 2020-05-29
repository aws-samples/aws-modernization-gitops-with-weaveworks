+++
title = "Verify App Mesh Resources"
chapter = false
weight = 50
+++

When we created the **VirtualNode** and **VirtualService** resources in EKS the controller acted on these and updated **AWS App Mesh**.

To check that the mesh has been created we can use the AWS CLI. Run the following command:

```bash
aws appmesh list-virtual-routers --mesh-name=apps
```

The output should be similar to below and you should see **apps** listed:

```json
{
    "virtualRouters": [
        {
            "arn": "arn:aws:appmesh:us-west-2:1234567890:mesh/apps/virtualRouter/backend-podinfo-router-apps",
            "meshName": "apps",
            "meshOwner": "1234567890",
            "resourceOwner": "1234567890",
            "virtualRouterName": "backend-podinfo-router-apps"
        }
    ]
}
```

Lets check that the traffic to the backend service has been configured to be routed 50:50 between v1 and v2:

```bash
aws appmesh describe-route --mesh-name=apps --virtual-router-name=backend-podinfo-router-apps --route-name=podinfo-route
```

Hopefully you will see something similar to the following:

```bash
{
    "route": {
        "meshName": "apps",
        "metadata": {
            "arn": "arn:aws:appmesh:us-west-2:1234567890:mesh/apps/virtualRouter/backend-podinfo-router-apps/route/podinfo-route",
            "createdAt": 1590071971.866,
            "lastUpdatedAt": 1590071971.866,
            "meshOwner": "1234567890",
            "resourceOwner": "1234567890",
            "uid": "aaa9b597-ede4-4d7a-8ca0-57682865003f",
            "version": 1
        },
        "routeName": "podinfo-route",
        "spec": {
            "httpRoute": {
                "action": {
                    "weightedTargets": [
                        {
                            "virtualNode": "backend-podinfo-v1-apps",
                            "weight": 50
                        },
                        {
                            "virtualNode": "backend-podinfo-v2-apps",
                            "weight": 50
                        }
                    ]
                },
                "match": {
                    "prefix": "/"
                }
            }
        },
        "status": {
            "status": "ACTIVE"
        },
        "virtualRouterName": "backend-podinfo-router-apps"
    }
}
```

You can also check by going to the AWS Console and choosing **AWS App Mesh** from the **Services** menu.
