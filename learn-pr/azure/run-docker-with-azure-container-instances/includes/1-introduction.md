Containers are becoming the preferred way to package, deploy, and manage cloud applications. Azure Container Instances offers the fastest and simplest way to run a container in Azure, without having to manage any virtual machines and without having to adopt a higher-level service.

Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs. For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).

Azure Container Instances offers the following benefits:

- **Fast startup**: Azure Container Instances can start containers in Azure in seconds.
- **Per second billing**: You are only charged while the container is running.
- **Hypervisor-level security**: Azure Container Instances guarantees your application is as isolated in a container as it would be in a VM.
- **Custom sizes**: Azure Container Instances provides optimum utilization by allowing exact specifications of CPU cores and memory.
- **Persistent storage**: To retrieve and persist state with Azure Container Instances, we offer direct mounting of Azure Files shares.
- **Linux and Windows**: Azure Container Instances can schedule both Windows and Linux containers with the same API.

## Learning objectives  

In this module, you will:

- Run a container in Azure Container Instances.
- Control container restart behavior.
- Use environment variables in your containers.
- Attach data volumes to Azure Container Instances.
- Learn about troubleshooting Azure Container Instances.
