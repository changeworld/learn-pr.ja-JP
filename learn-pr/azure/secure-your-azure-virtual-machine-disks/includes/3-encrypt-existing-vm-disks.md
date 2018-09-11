Suppose your company has decided to implement Azure Disk Encryption (ADE) across all VMs. You need to evaluate how to roll out encryption to existing VM volumes.

Here, we'll look at the requirements for ADE, and the steps involved in encrypting disks on existing Windows VMs.

## Azure Disk Encryption prerequisites

Before you can encrypt your first VM disk, you need to:

1. Create a key vault.
1. Set up an Azure Active Directory (Azure AD) application and service principal.
1. Set the key vault access policy for the Azure AD app.
1. Set key vault advanced access policies.

### Azure Key Vault

The encryption keys used by ADE can be stored in Azure Key Vault. Azure Key Vault is a tool for securely storing and accessing secrets. A secret is anything that you want to tightly control access to, such as API keys, passwords, or certificates. This provides highly available and scalable secure storage, in Federal Information Processing Standards (FIPS) 140-2 Level 2 validated Hardware Security Modules (HSMs). Using Key Vault, you keep full control of the keys used to encrypt your data, and you can manage and audit your key usage. You can configure and manage your key vault using the Azure portal, Azure PowerShell, and the Azure CLI.

>[!NOTE]
> Azure Disk Encryption requires that your key vault and your VMs are in the same Azure region; this ensures that encryption secrets do not cross regional boundaries.

### Azure AD application and service principal

To access or modify resources, such as the encryption configuration for a VM with scripts or code, you must first set up an **Azure Active Directory (AD) application**. Azure AD is a multi-tenant, cloud-based directory, and identity management service. It combines core directory services, application access management, and identity protection into a single solution.

You'll also need an Azure **service principal**. Service principals are the service accounts you use to run the script or code. They allow you to assign the specific permissions and scope that are needed to run the task against a particular Azure resource.

There are two elements in Azure AD: the application object is the **_definition_** of the application (what it does), and the service principal is the **_specific instance_** of the application.

This approach aligns with the principle of **least privilege**, where the permissions assigned to the app are restricted to the bare minimum required to enable the app to perform its tasks.

You can configure and manage Azure AD applications and service principals using the Azure portal, Azure PowerShell, and the Azure CLI.

### Key vault access policies

Before you can store encryption keys in a key vault, ADE requires details on the **Client ID** and the **Client Secret** of the Azure AD application that is permitted to write to the key vault.

You'll also need to provide Azure access to the encryption keys in your key vault, so they are made available to the VM for booting and decrypting the volumes.

## Set key vault advanced access policies

**Advanced access policies** enable disk encryption on the key vault, and without them, encryption deployments will fail. 

There are three policies that need to be enabled:

- **Key Vault for disk encryption**. Required for Azure Disk Encryption.
- **Key Vault for deployment**. Enables the Microsoft.Compute resource provider to retrieve secrets from the key vault. This policy is needed when you are creating a VM.
- **Key Vault for template deployment, if needed**. Enables Azure Resource Manager to get secrets from the key vault. This policy is required when you are using Azure Resource Manager templates for VM deployment.

Key vault access policies can be configured and managed using the Azure portal, Azure PowerShell, or the Azure CLI.

### What is the Azure Disk Encryption prerequisites configuration script

The **Azure Disk Encryption prerequisites configuration script** sets up all (or as many as you want) of the encryption prerequisites. The script also ensures that your key vault is in the same region as the VM you are going to encrypt. It will create a resource group and key vault, and set the key vault access policy. The script also creates a resource lock on the key vault to help protect it from accidental deletion.

## Encrypting an existing VM disk

There are two steps for encrypting an existing VM disk, when you are using the Azure Disk Encryption prerequisites configuration script:

1. Run the Azure Disk Encryption prerequisites configuration script.
1. Encrypt the Azure virtual machine in PowerShell.
