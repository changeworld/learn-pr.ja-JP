Let's create an Azure Redis Cache instance to store and return commonly used values.

<!-- TODO: do we need to activate the sandbox here? -->

## Create a Redis cache in Azure

1. Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Click **Create a resource**, click **Databases**, and click **Redis Cache**.

    The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.

    ![Screenshot showing the Azure portal database options, with the Create a resource, Database, and Redis Cache options highlighted.](../media/4-create-a-cache-1.png)

### Identify the location for the cache

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### Configure your cache

Apply the following settings on the cache.

1. **DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.

1. **Subscription:** Select the Azure Sandbox subscription.

1. **Resource group:** Select <rgn>[Sandbox resource group name]</rgn> for the Resource Group.

1. **Location:** Normally, you would select a location near your customers - in this case, the East Coast. However, the Azure Sandbox only allows specific regions to be selected for resources as noted above. Please select one of those locations.

1. **Pricing tier:** Select **Basic C0**. This is the lowest tier you can use. Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.

1. Click **Create**.

    The following screenshot shows a representative configuration used to create a new Redis Cache resource. Note that yours will be slightly different due to the Azure Sandbox.

    ![Screenshot showing the Azure portal blade when creating a new Redis Cache resource, populated with an example configuration DNS name, subscription, new resource group, location, and pricing tier.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> You will have to wait until the cache is deployed before continuing. This process might take some time.

## Verify your data

You can use the **Console** feature in the Azure portal to issue commands to your Redis cache instance after it has been created.

1. Locate your Redis cache by selecting **All Resources** in the left-hand sidebar and using the filter box on the left to select Redis Cache instances. Alternatively, you can use the search box at the top and type the name of the cache.

1. Select your Redis cache instance.

1. In the **Overview** blade for your Redis Cache, select **Console**. This will open a Redis console, which allows you to enter low-level Redis commands.

1. Type **ping**. Verify that the value returned is **PONG**.

1. Type **set test one**. Verify that the value returned is **OK**.

1. Type **get test**. Verify that the value returned is **"one"**.

1. Switch back to the **Overview** panel either through the breadcrumb bar on the top, or use the scrollbar to slide the view back to the left.

## Retrieve the access keys and host name

1. Select **Settings** > **Access keys**. 

1. Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.

    This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.

Next, let's learn about some of the commands we can use to interrogate the cache.