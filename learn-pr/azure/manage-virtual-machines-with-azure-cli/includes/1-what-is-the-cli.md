Jim manages a set of Azure virtual machines running on our corporate web infrastructure, which includes several websites and database servers running on various platforms. 

While the Azure portal is easy to use, Jim has found that navigating through the various blades adds time to some of the tasks. 

While exploring alternatives, he discovers the Azure CLI tool.

Working with the Azure CLI, Jim can write scripts to check the status of his servers, deploy a new configuration, open a port, or connect to a virtual machine to change a setting.

Perhaps you are like Jim and are looking for a tool to help automate tasks in your cloud environment. We're going to show you how to use the Azure CLI to create and manage virtual machines hosted in Azure. 

## Azure CLI

The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources. It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> There are two versions of the Azure CLI tool available today: Azure CLI 1.0 and Azure CLI 2.0. We'll be using Azure CLI 2.0, which is the latest version and is recommended unless you're running legacy scripts. Azure CLI 1.0 is started with the `azure` command, and Azure CLI 2.0 is started with the `az` command. 

The Azure CLI can help you manage Azure resources such as virtual machines and disks from the command line or in scripts. Let's get started and see what it can do.

## Learning objectives

In this module, you will:

- Create a virtual machine with the Azure CLI
- Resize virtual machines with the Azure CLI.
- Perform basic management tasks using the Azure CLI.
- Connect to a running VM with SSH and the Azure CLI.
