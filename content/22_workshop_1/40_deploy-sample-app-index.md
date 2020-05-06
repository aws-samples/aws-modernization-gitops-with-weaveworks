+++
title = "Deploy Sock Shop Application (Optional)"
chapter = true
weight = 40
+++

### Shop Sock

For our labs, we picked up the Sock Shop application. [Sock Shop](https://github.com/microservices-demo/microservices-demo) is a microservice sample application that [Weaveworks](https://weave.works) initially developed for demoing and testing purposes. We made it open source so it can be used by  other organizations for learning and demonstration purposes. 

![sockshop](https://github.com/microservices-demo/microservices-demo.github.io/raw/master/assets/sockshop-frontend.png)

### Shop Sock Architecture

The architecture of the demo microserivces application was intentionally designed to provide as many microservices as possible. Please don't use the Sock Shop application as a model for a well architected micro-services application, it was built for demonstration purposes. If you are just beginning to architect your own micro-services based cloud native application, [Weaveworks](https://www.weave.works/contact/) would be happy to help recommend the correct architecture for your use case..

Furthermore, Sock Shop is intentionally uses a number of different languages and technologies in order to demonstrate how Kubernetes works for a number of different technologies. Again, we'd recommend that you only consider new technologies based upon a need.

As seen in the image above, the microservices are roughly defined by the functions of an e-commerce site. Networks are specified, but due to technology limitations may not be implemented in some deployments.

All services communicate using REST over HTTP. This was chosen due to the simplicity of development and testing. Their API specifications are under development.

![sockshop-topology](/images/sockshop-topology.png)

{{% children showhidden="false" %}}
