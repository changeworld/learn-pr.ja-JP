In this module, you've seen what Azure Database for PostgreSQL offering looks like. You saw how to create an Azure Database for PostgreSQL using the Azure portal as well as Azure CLI.

You looked at the configuration options available to configure a PostgreSQL server-level firewall rule and enforce SSL connections. Finally, you saw how to connect to an Azure Database for PostgreSQL server using _psql_ in Azure Cloud Shell

## Clean up

<!---TODO: Update for sandbox?--->

All that is left now is for you to clean up the resources you created in the labs. Remember the server will generate a monthly charge even though you may not use it. If you create resources for testing purposes, an easy way to get rid of them all is to delete the Resource Group they are part of.

> [!NOTE]
> Recall, a resource group is a collection of resources that share the same lifecycle, permissions, and policies. These actions will delete all resources allocated within this resource group, including all data you may have added to the databases on the server you created.

- Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).

- In the left menu, select **Resource groups** to view all resource groups currently in use for your account.

- Click on the name of the resource group you created and click on the **Delete resource group** button in the toolbar near the top.

- You'll a dialog box titled **Are you sure you want to delete `<resource_group_name>`** prompting you to type in the name of the resource group to confirm deletion. Confirm the resource group name and click the Delete button at the bottom.
