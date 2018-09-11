Microsoft Cognitive Services is a rich suite of intelligent services that we can use to enrich our apps. We explored a small part of the Text Analytics API service to find out higher-level information about text. We used the service to analyze text feedback from customers for sentiment. We created a solution hosted in Azure Functions to sort these text messages into different buckets, or queues, for further processing.

Once you know how to call a REST API, then you can easily integrate these intelligent services into your solutions. They all follow a similar pattern:

- Sign up for an access key
- Explore in the API testing console
- Formulate requests using the access key and the correct region.
- POST requests from your solution and parse the responses for insights.

We added this intelligence to serverless logic created in Azure Functions. You can easily call these services from other types of apps. There are many client libraries, tutorials and,  quickstarts to get you started.

## Suggestions for further enhancement of our solution

Here are some ideas for you to consider if you want to take what we did further. 

- Test the solution with more text examples and decide whether the thresholds we set to categorize sentiment scores into positive, negative, and neutral are appropriate. 
- Consider adding another function into your function app that reads messages from the [!INCLUDE [negative-q](./q-name-negative.md)] queue and calls the Text Analytics API to find key phrases in the text.
- Our input queue contains raw text feedback. In the real-world, we would associate feedback with some form of user ID such as email address, account number, and so on. So, enhance the input queue items to be JSON documents containing and ID field and the text. Then use that ID when working with the text message.
 - Currently our solution is "hard coded" to English. Think about what changes you would do to make it capable or handling text in all languages supported by the Text Analytics API.  

Now that you know how to call one of these Cognitive Services APIs, take a look at some of the other services and think about how you might use them in your solutions. 

## Further reading

- [Text Analytics overview](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [How to detect sentiment in Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Cognitive Services Documentation](https://docs.microsoft.com/azure/cognitive-services/)

## Clean up resources

*Resources* in Azure refer to function apps, functions, storage accounts, and so forth. They are grouped into *resource groups*, and you can delete everything in a group by deleting the group.

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
