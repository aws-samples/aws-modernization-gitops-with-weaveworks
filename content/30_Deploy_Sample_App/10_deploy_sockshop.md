+++
title = "Deploy Sock Shop App"
chapter = false
weight = 10
+++

## Script overview

Change directory to `gitops-k8s`. You can take a look at the deployment script that does the following tasks:

* Create a dev namespace
* Create a production namespace
* Deploy the backend services (databases and message queuing)
* Deploy the application services
* Expose frontend and carts services to public internet
* Launch continuous load testing on the dev carts service

```sh
$ cd gitops-k8s
$ more deploy-sockshop.sh
```

## Deploy Sock Shop

Now just execute the deployment script:

```sh
$ ./deploy-sockshop.sh
```

## Validate

Get the list of Sock Shop pods by running this command: 

```sh
$ eksctl get pods --all-namespaces -l product=sockshop
```

Notice the pods status and ready state. They should all be `Running` and `Ready`. Just rerun the command they are still starting up.

![validation](/images/validate.png)

## Access the Sock Shop web app

The application deployment created a <b>Service</b> resource of type <b>Load Balancer</b> to expose the <b>front-end</b> and <b>carts</b> services to the public internet. It might take a few minutes before the public IPs become available.
You can obtain the app URLs by running this script:

```sh
$ ./get-sockshop-urls.sh
```
The script will wait until the IPs are available and will then print those. 

![app urls](/images/app_urls.png)

Click on the Production or Dev frontend URL to load the Sock Shop home page.

The URLs are stored in the `configs.txt` file. You can always get those by running this command (from the current directory):

```sh
$ cat configs.txt
```

## Create Sock Shop user accounts

This following script will create a few user accounts that will be used by the synthetic monitors to generate traffic on the Production environment:

```sh
$ ./create-sockshop-accounts.sh
```

## Explore the app

Load the Sock Shop app page in your browser.

![sockshop](/images/sockshop.png)

Play around! 

Run some transactions from the browser (Register, Logout, Login, Catalogue, Add to Cart, etc).

You can manually register a new account or log in with one that was created during the previous step:

`username : perform`

`password : 1234`

{{% notice warning %}}
The checkout service in the application is not currently implemented. You can add items to the shopping cart but you will not be able to checkout.
{{% /notice %}}

{{% children showhidden="false" %}}
