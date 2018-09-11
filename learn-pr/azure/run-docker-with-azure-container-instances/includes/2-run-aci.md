Azure Container Instances makes it easy to create and manage Docker containers in Azure, without having to provision virtual machines or adopt a higher-level service. In this unit, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).

## Create a resource group

Azure Container Instances, like all Azure resources, must be placed in a resource group, a logical collection into which Azure resources are deployed and managed.

Create a resource group with the `az group create` command.

The following example creates a resource group named *myResourceGroup* in the *eastus* location:

```azurecli
az group create --name myResourceGroup --location eastus
```

## Create a container

You can create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command. You can optionally expose the container to the internet by specifying a DNS name label. In this example, you deploy a container that hosts a small web app.

Execute the following command to start a container instance. The *--dns-name-label* value must be unique within the Azure region you create the instance, so you might need to modify this value to ensure uniqueness:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

Within a few seconds, you should get a response to your request. Initially, the container is in the **Creating** state, but it should start within a few seconds. You can check the status using the `az container show` command:

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:

```bash
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser:

![Browser screenshot showing an application running in an Azure container instance](../media-draft/aci-app-browser.png)

## Summary

In this unit, you created an Azure container instance that is running a web server and application. You also accessed this application using the FQDN of the container instance.

In the next unit, you will configure container instance restart policies.