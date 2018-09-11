The Azure portal allows you to manage, and scale PostgreSQL database servers. You decide to create an Azure Database for PostgreSQL server to store runner performance data. Based on historic captured data volumes, you know your server storage requirements should be set at 10 GB. To support your processing requirements, you need compute Gen 5 support with 1 vCore. You also know that you typically store backups for 25 days.

> [!TIP]
> All of the exercises you do in Microsoft Learn are free, but once you start exploring on your own, you will need an Azure subscription. If you don't have one yet, take a couple of minutes and create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true). You'll see Azure resource creation and management menu on your left and the dashboard filling the rest of the screen.

## Create an Azure Database for PostgreSQL server

Once signed in you'll see the default Dashboard displayed. You have a couple of options available to you to create an Azure Database for PostgreSQL server. From the Dashboard, you can either:

- Select the **All services** option and then search for the **Azure Database for PostgreSQL server** option. This screen will display any configured servers already in your account. From here, you select **Add**, which will take you to the new server creation blade.

or

- Select the **Create a resource** option, which will present you with Azure Marketplace resource options. From here, you select the Databases option and choose **Azure Database for PostgreSQL**.

### Configure the server

You'll now see the PostgreSQL server create blade, similar to the following illustration.

![Screenshot of the Azure portal showing the creation blade for a new PostgreSQL database.](../media-draft/4-create-blade.png)

> [!NOTE]
> You'll need to remember or write down some details as you create the PostgreSQL server. For example the username and password to access the server. You'll use this information to connect to your server later.

1. Choose a unique name for the server. Recall, that then name must be all lowercase and can have numbers and hyphens.

1. Select a subscription, check to be sure this field is set to the subscription you want to use.

1. You now have the option to create or reuse an existing resource. To create a new resource, select the **Create new** radio button and enter a name for the new resource group. You'll use this group for the rest of this module. Name the resource something descriptive so that it's easy to delete the resource later.

1. Select the source of your new server. For this lab, you'll leave the option at _Blank_. Recall, you can change the option to _Back up_ if you want to restore and existing server backup.

1. Choose a login name to use as an administrator login for the new server. Recall, the admin login name can't be azure_superuser, azure_pg_admin, admin, administrator, root, guest, or public. It can't start with pg_. Remember or write down the name for future use.

1. Choose a password to use with the above administrator login name. Recall, our password must include characters from three of the following categories: English uppercase letters, English lowercase letters, numbers (0 through 9), and non-alphanumeric characters (!, $, #, %, etc.). Remember or write down the password for future use.

1. Retype the password to confirm your password.

1. Choose a location for your server. You'll want to choose a location closest to you.

1. You'll now select the version of for your server. Select the latest version of PostgreSQL.

1. As the second last step, select the **Pricing tier** option.

    Recall that you need to configure your server with specific storage and compute options.

    - 10 GB of disc storage
    - Compute Generation 5 support
    - Retention period of 25 days

    Click **Pricing tier** to access the pricing tier blade and make the following changes.

    - Choose the **Basic** option tab.
    - Choose the **Gen 5 Computation Generation** option.
    - Choose 1 vCore from the **vCore** slider. Notice how the changes in the slider affect the **Price Summary**.
    - Choose 10 GB from the **Storage** slider. If you're having trouble sliding to exactly 10 GB, you can use your keyboard's left and right cursor keys to get a precise value.
    - Choose 25 Days from the **Backup Retention Period** slider.

        ![Screenshot of the Azure portal showing the database pricing tier for a new PostgreSQL database.](../media-draft/4-azure-db-pricing-tier.png)

    - Click **OK** once you're satisfied with your selection to commit your selections and close the pricing tier options.

1. All that is left now, is to review the values you entered and click **Create**. Creation can take several minutes. You can select the Notifications icon (a bell) at the top of the Azure portal screen to monitor progress.

You now have a PostgreSQL server available. In the next unit, you'll see how to create the same server using the Azure CLI.
