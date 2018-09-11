You decide to create an Azure Database for PostgreSQL to store routes captured from runners' fitness devices. Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB. To support your processing requirements, you need compute Gen 5 support with 1 vCore. You also know that you require a retention period of 15 days for data backups.

> [!TIP]
> All of the exercises you do in Microsoft Learn are free, but once you start exploring on your own, you will need an Azure subscription. If you don't have one yet, take a couple of minutes and create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Let's begin.

Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).

Recall that you'll need to start an Azure Cloud Shell session. Select the Cloud Shell icon at the top of the screen to start the session.

If you don't already have a storage account to use with Cloud Shell, you'll need to create one with first access. The portal interface will step you through the process of creating a storage account.

This lab uses `bash` as the command-line environment.

1. Select the subscription you'll use to create the server.

    If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    Recall, you can list all your subscriptions using the `az account list --output table` command. Pick the subscription identifier from this list that you'd like to use.

1. If you haven't already done so in a previous unit, create a resource group. You'll run the following command.

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > You can retrieve a list of all locations using this command `az account list-locations`. Select the `displayName` or `name` value and use it for the `<location>` parameter.

1. You're now ready to run the `az postgres server create` command.

    Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.

    There are several parameters that you'll specify:

    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

    See if you can write the command and complete the parameters without looking at the solution below. Replace the values in `<>` with your own values.

    > [!NOTE]
    > You can retrieve a list of all locations using this command `az account list-locations`. Select the `displayName` or `name` value and use it for the `<location>` parameter.

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

You'll see the system take a few moments to process the information when executed. A Java Script Object Notation (JSON) string that describes the server is returned if the server was created. An error message is displayed if the server isn't created. You'll use this error information to review and fix your command parameters.

You've successfully created a PostgreSQL server using the Azure CLI. In the next unit, you'll see how to configure your server's security settings.
