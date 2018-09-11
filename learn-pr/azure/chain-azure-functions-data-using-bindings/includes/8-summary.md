This module was all about integrating data and services into your functions. We started off with a quick tour of the binding types that show up when you add them to a function. We then looked at reading data from an Azure Cosmos DB using an input binding. The platform takes care of managing connection strings and we saw how easy it is to read data in our code using the binding. Finally we focused our attention on writing data different sources with the help of output bindings. This journey is summarized in the following table.

[!INCLUDE [summary table](./summary-table.md)]

You can apply the approaches you have learned here to add and test bindings in your functions. Here are a few interesting ideas to get more practice with bindings and to build on what you have learned here.

* Create another function to read from Blob Storage and other input bindings that we haven't used in this module.

* Create another function to write to more destinations using other supported output binding types.

* In the last unit, we introduced a queue and posted messages to it with an output binding. As a next step, consider adding another function that reads the messages in the queue and prints out the **MESSAGE TEXT** to the console with `Console.Log()`.

As we saw in this module, the Azure portal offers easy-to-use features to start building functions and connecting them to data and other services.

## Clean up resources

*Resources* in Azure refers to function apps, functions, storage accounts, and so forth. They are grouped into *resource groups*, and you can delete everything in a group by deleting the group.

You created resources to complete this module. You may be billed for these resources, depending on your [account status](https://azure.microsoft.com/account/) and [service pricing](https://azure.microsoft.com/pricing/). If you don't need the resources anymore, here's how to delete them:

1. In the Azure portal, go to the **Resource group** page.

   To get to that page from the function app page, select the **Overview** tab and then select the link under **Resource group**.

   To get to that page from the dashboard, select **Resource groups**, and then select the resource group that you used for this module. 

> [!NOTE]
> The default name of the resource group we suggested for this module was [!INCLUDE [resource-group-name](./rg-name.md)] but it is possible that you used another name.

2. In the **Resource group** page, review the list of included resources, and verify that they are the ones you want to delete.

3. Select **Delete resource group**, and follow the instructions.

   Deletion may take a couple of minutes. When it's done, a notification appears for a few seconds. You can also select the bell icon at the top of the page to view the notification.

## Further Reading

While this is not intended to be an exhaustive list, the following are some resources related to the topics covered in this module that you might find interesting.

 * [Azure Functions documentation](https://docs.microsoft.com/azure/azure-functions/)

* [The Azure Functions Challenge](https://aka.ms/afc)

* [Azure Serverless Computing Cookbook](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [How to use Queue storage from Node.js](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Introduction to Azure Cosmos DB: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [A technical overview of Azure Cosmos DB](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/cosmos-db/)
