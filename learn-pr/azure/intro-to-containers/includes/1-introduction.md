Containers are a modern way of delivering applications and compute processes. When using containers, applications and all dependencies are packaged into what is known as a *container image*. Container images are super portable when using a container image registry. You can create a container image on your development system, then run an instance of that image in an Azure datacenter and have confidence that it will work without additional modification.

## Container efficiencies

Containers and container images are built in such a way that they efficiently use host resources, such as disk space, memory, and CPU. Due to these efficiencies, containers start quickly. In some cases, starting a new instance of a container is almost instantaneous. This not only allows for quick provisioning of applications, it also allows for a new model of on-demand processing and scale operations.

Envision this scenario: You run a batch processing service that occasionally sees a large spike in demand. Using containers, you can build a system that reacts to increased demand by quickly provisioning new container instances to meet the increased demand. That's powerful and not easy to achieve with traditional virtual machines.

In addition to their fast start, containers also allow you to achieve "hyper density". This effectively means that you can run more applications and processes with less virtual or physical resources.

## Use cases

While containers are a great platform for running traditional workload like webservers, they also help open opportunities, such as burstable batch processing, applications built with a modern and distributed architecture, and anything that requires on-demand scale.

## Learning objectives

In this module, you will:

- Prepare a local container development environment.
- Learn basic Docker operations.
- Run, list, and delete containers.
- Create a custom container images.
- Push container images to a public container registry and run containers from these images.
