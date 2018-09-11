You've successfully created and deployed a full-featured web application using the Web Apps feature of Azure App Service.

App Service simplifies managing and controlling your web app in comparison to traditional hosting options. Web Apps can help you reduce the time and effort spent running and managing your web app, and provide advanced cloud features such as auto scaling and Git integration.

## Clean up
<!---TODO: Update for sandbox?--->

When you are done working with this application, you can follow the steps below to delete all resources created during the tutorial.

As you know, a resource group associates all the other resources that are created and related to the web app. So, cleaning up after yourself on Azure is a matter of deleting the resource group, and hence, all the resources created under that group disappear.

Let's explore together how you can delete a resource group using the Azure portal:

1. Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).

1. Click the **Resource groups** menu item on the left-side of the Dashboard. The **Resource groups** blade lists all the resource groups that you have created in the Azure portal.

1. Select the resource group name that you created in Unit #2. The portal navigates you to the web app blade.

1. The Azure portal navigates you to the **Resource group** blade. There, you can see a list of all the resources that you've created during this module under this resource group.

1. Click the **Delete resource group** link at the top of the blade.

1. Azure verifies that you really want to delete this resource group by asking you to type the name of it. To proceed, type the name of the resource group.

1. Click the **Delete** button at the bottom of the confirmation window.

1. Azure takes a few seconds to delete all the resources of the resource group.
