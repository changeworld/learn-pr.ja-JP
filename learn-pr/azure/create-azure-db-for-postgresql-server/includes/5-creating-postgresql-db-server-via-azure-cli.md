Let's assume you're using an on-premises PostgreSQL database. Your company is now looking at expanding device support, availability, data tracking, and processing features by moving your server into Azure. You'll investigate how much effort it takes to automate the creation of an Azure Database for PostgreSQL.

Creating a single Azure Database for PostgreSQL server using the Azure portal is easy. Creating more than one database and running ongoing maintenance using only the portal may become tedious. You'll use the Azure CLI to create scripts when you want to automate management tasks.

Creating almost any resource within Microsoft Azure can be automated using the Azure CLI. In this unit, you'll learn how to automate management of your Azure Database for PostgreSQL servers using the Azure CLI.

## What is Azure CLI?

[Azure CLI](https://docs.microsoft.com/cli/azure/) is Microsoft’s cross-platform command-line environment for managing Azure resources. You can use the Azure CLI from your browser with Azure Cloud Shell, or you can install Azure CLI locally on Mac OS X, Linux, or Windows. The Azure CLI is run from a local command line using bash or Powershell. Running Azure CLI locally however requires additional setup. We'll use the Azure Cloud Shell for executing Azure CLI commands.

## What is Azure Cloud Shell?

Azure Cloud Shell is a browser-based shell experience that is hosted in the cloud and allows you to connect to Azure using an authenticated session. You can execute Azure CLI commands to automate the management of an Azure Database for PostgreSQL. Common Azure CLI tools are pre-installed and configured in Cloud Shell for you to use with your account.

> [!NOTE]
> Cloud Shell requires an Azure storage resource to persist any files you create while working in the Cloud Shell. On first launch Cloud Shell prompts to create a resource group, storage account, and Azure Files share on your behalf. This is a one-time step and will be automatically attached for all future Cloud Shell sessions.

## Create an Azure Database for PostgreSQL server using Azure CLI

You'll use Azure Cloud Shell to create an Azure Database for PostgreSQL server using Azure CLI. Let's look at the steps you'll take.

First, sign into the Azure portal.

Open the Cloud Shell from the Azure portal. Open your browser and go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:

Cloud Shell allows you to run your commands either in `bash` or `PowerShell`. We'll use the `bash` command-line option for all examples.

If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

You'll run the following command to list all your subscriptions.

   ```bash
   az account list --output table
   ```

The next step is to create a resource group to manage the server and where the resource group will be located. Recall, you'll use a resource group to manage all the resources related to your server. The location option allows you to specify where the server is created physically. You'll run the next command and replace the `<resource_group_name>` and `<location>` respectively with appropriate values.

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

The last step is to create the Azure Database for PostgreSQL server.

   The Azure CLI server creation command usage help showing all available parameters looks like the following example:

   ```bash
   az postgres server create [-h] [--verbose] [--debug]
                             [--output {json,jsonc,table,tsv}]
                             [--query JMESPATH]
                              --resource-group RESOURCE_GROUP_NAME --name SERVER_NAME
                              --sku-name SKU_NAME [--location LOCATION]
                              --admin-user ADMINISTRATOR_LOGIN
                              [--admin-password ADMINISTRATOR_LOGIN_PASSWORD]
                              [--backup-retention BACKUP_RETENTION]
                              [--geo-redundant-backup GEO_REDUNDANT_BACKUP]
                              [--ssl-enforcement {Enabled,Disabled}]
                              [--storage-size STORAGE_MB]
                              [--tags [TAGS [TAGS ...]]]
                              [--version VERSION]
                              [--subscription _SUBSCRIPTION]

   ```

   The following command line shows the required set of parameters to create an Azure Database for PostgreSQL server. You'll notice some are optional parameters and aren't listed.

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### Parameter descriptions

The `--resource-group <resource_group_name>` parameter specifies the resource group within which to create the server.

The server `admin-user` and `admin-password` that you specify is required to sign in to the server and its databases. Remember or record this information for later when interacting with the new server.

You use the `--sku-name` parameter is used to specify part of the pricing tier, in this case compute resource. The value follows the convention `{pricing tier}_{compute generation}_{vCores}`.

Examples:

- `--sku-name B_Gen4_4` maps to Basic, Gen 4, and 4 vCores.
- `--sku-name GP_Gen5_32` maps to General Purpose, Gen 5, and 32 vCores.
- `--sku-name MO_Gen5_2` maps to Memory Optimized, Gen 5, and 2 vCores.

Recall, we discussed the three pricing tiers in the unit where we create the server using the portal.

Let's assume you want to use a Basic, Gen 5, and 1 vCore compute resource, you'll then specify the parameter as `--sku-name B_Gen5_1`.

You use the `--storage-size` parameter is also used the specify part of the pricing tier. If the value isn't specified, then it defaults to 5,120 MB. Valid storage sizes range from 5,120 MB and increases in additional increments of 1,024 MB up to 1,048,576 MB.

The `--backup-retention` parameter is used when you need to specify the retention period for backups specified in days. If the value isn't specified, then it defaults to seven days.

You use the `--version` parameter is used to specify the major version of PostgreSQL you would like to use.

You've now seen the steps to create an Azure Database for PostgreSQL using Azure CLI. In the next unit, you'll create an Azure Database for PostgreSQL server using Azure CLI.