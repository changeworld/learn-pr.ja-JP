Here you'll add an eviction policy to our Azure Redis Cache.

## Set an eviction policy

To set an eviction policy in Azure, we simply use a drop-down menu in the portal.

1. Open your Redis Cache in the Azure portal.

1. Select the **Advanced settings** blade.

1. Use the **maxmemory-policy** drop-down menu and select **allkeys-random**.

1. Click **Save**. 

At this point, if you run out of memory, Redis will select a random key to delete to make room for your new data.