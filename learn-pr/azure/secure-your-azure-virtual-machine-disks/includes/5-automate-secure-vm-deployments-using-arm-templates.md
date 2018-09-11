Suppose your company is deploying several servers as part of their cloud transition. VM disks must be encrypted during the deployment, so there's no time when the disks are vulnerable. You want to automate this process, and have to modify the Azure Resource Manager templates to automatically enable encryption.

Here, we'll look at how to use an Azure Resource Manager template to automatically enable encryption for new Windows VMs.

## What are Azure Resource Manager templates?

These templates are JSON files used to define a resource to deploy to Azure, such as a virtual machine. You can write them from scratch, and for some Azure resources, including VMs, you can use the Azure portal to generate them. You'll need to complete the required information for a manual VM deployment, but instead of deploying the VM to Azure, you save the template.

There are example templates available in GitHub.

## Using GitHub templates

GitHub has a template for enabling encryption called the **Enable encryption on a running Windows VM ARM**. You can find it in the [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates) repository. The readme page for the template provides a **Deploy to Azure** button that then opens the template in the Azure portal.

The template enables you to deploy a Windows Server VM, with encryption pre-enabled. Before using the template, you must make sure that all of the encryption prerequisites are in place. You'll also need the configuration information that is provided by the prerequisites script, such as Azure Active Directory Client ID and Azure Active Directory Client Secret.

You create a new VM by entering the required information on the template. You then initiate deployment by clicking **Purchase** (the cost is typically the normal Azure compute charge).

## Deploy an encrypted VM by using a template

The main steps involved in deploying an encrypted VM using a template are:

1. Access and run the **Enable encryption on a running Windows VM ARM** template from GitHub.

1. Add the required details in the script configuration page.

1. Deploy a new VM using the script by clicking **Purchase**.

1. Use the Azure portal to verify disk encryption status.
