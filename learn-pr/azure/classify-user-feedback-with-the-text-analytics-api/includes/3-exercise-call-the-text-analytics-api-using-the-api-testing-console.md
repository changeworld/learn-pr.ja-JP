To see the Text Analytics API in action, let's make some calls using  the built-in API testing console tool located in the online reference documentation. Before we do that, we'll need an access key to make those calls. 

## Create an access key

Every call to Text Analytics API requires a subscription key. Often called an access key, it is used to validate that you have access to make the call. We'll use the Azure portal to grab a key. 

1. Sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with your Azure account.

1.  Click **+ Create a resource**

1.  Under Azure Marketplace, select **AI + Machine Learning** to display a list of available APIs. Click on **See all** in the top right of the page to see the entire list of Cognitive Services APIs. 

1. Find **Text Analytics** in the list of Cognitive Services and select it. 
![Screenshot of AI and Machine Learning pane, showing the Text Analytics API selected in the list.](../media-draft/select-text-analytics.PNG)

1. In the create page that opens, enter the following values into each field.


|Property  | Value  | Description  |
|---------|---------|---------|
|Name     |    MyTextAnalyticsAPIAccount     |  The name of the Cognitive Services account. We recommend using a descriptive name. Valid characters are `a-z`, `0-9`, and `-`.    |
|Subscription     |  Your subscription       |   The subscription under which this new Cognitive Services API account with **Text Analytics API** is created.      |
|Location     |  West US       |  Choose a [region](https://azure.microsoft.com/regions/) near you.       |
|Pricing tier     | **F0 Free**     |   The cost of your Cognitive Services account depends on the actual usage and the options you choose. We recommend selecting the free tier for our purposes here.      |
|Resource group     |  Create a new resource group and call it [!INCLUDE [resource-group-note](./rg-name.md)]       |  Name for the new resource group in which to create your Cognitive Services Text Analytics API account.       |


[!INCLUDE [resource-group-note](./rg-notice.md)]

Here's a screenshot of what the **Create** page looks like when you've completed it.

![Screenshot of user interface for creating a Text Analytics account with all fields filled out with values suggested in preceding table.](../media-draft/create-text-analytics-account.PNG)

6. Select **Create** at the bottom of the page to start the account creation process.  Watch for a notification that the deployment is in progress. You'll then get a notification that the account has been deployed successfully to your resource group.

![Deployment succeeded notification with a Go to resource button and a Pin to dashboard button.](../media-draft/deploy-resource-group-success.PNG)

Now that we have our Cognitive Services account, let's find the access key so we can start calling the API. 

7. Click on the **Go to resource** button on the *Deployment succeeded* notification. This action opens the account Quickstart. 

1. Select the **Keys** menu item from the menu on the left, or in the *Grab your keys* section of the quickstart.  This action opens the **Manage keys** page.

1. Copy one of the keys using the copy button. 

![Manage keys user interface showing the name of the Cognitive Services account and the Key 1 and Key 2 entries.](../media-draft/manage-keys.PNG)

> [!IMPORTANT]
> Always keep your access keys save and never share them. 

10. Store this key for the rest of this module. We'll use it shortly to make API calls from the testing console and throughout the rest od the module.

Now that we have our key we can head over to the testing console and take the API for a spin.

## Call the API from the testing console

1. Navigate to the [Text Analytics API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7?azure-portal=true) reference documentation in your favorite browser.

The landing page displays a menu on the left and content to the right. The menu lists the POST methods you can call on the Text Analytics API. These endpoints are **Detect Language**, **Entities**, **Key Phrases**, and **Sentiment**.  To call one of these operations, we need to do a few things.
- Select the method we want to call.
- Select a testing console that matches the region or location we selected earlier in this lesson. 
- Add the access key that we saved earlier in the lesson to each call.

2. Front the left menu, select **Sentiment**. This selection opens the Sentiment documentation to the right. As the documentation shows, we'll be making a REST call in the following format:

`https://[location].api.cognitive.microsoft.com/text/analytics/v2.0/sentiment` 

We'll pass in our subscription key, or access key, in the **ocp-Apim-Subscription-Key** header.

## Make some API calls

1. Select a region from the list on this page that matches the location we picked when creating our Cognitive Services account earlier in this lesson.  For example, if we chose *West US* earlier when creating the account, then select it here as follows.
![Screenshot of Text Analytics API reference site with Sentiment menu item selected, followed by West US.](../media-draft/select-testing-console-region.png)

1.  The next page that opens is a live, interactive, API console.  Paste the access key you saved earlier into the field on the page labeled **Ocp-Apim-Subscription-Key**. Notice that the key is written automatically into the HTTP request window as a header value.

1. Scroll to the bottom of the page and click **Send**. Let's break down what happens by looking at this screen in detail.

In the Headers section of the user interface, we set the access, or subscription, key in the header of our request.

![Screenshot of headers section.](../media-draft/2-marker.PNG)

Next we have the request body section which  holds a **documents** array. Each document in the array as three properties. The properties are *"language"*, *"id"*, *"text"*. The *"id"* is a number in this example, but can be anything you want as long as it's unique in the documents array. In this example we're also passing in documents written in three different languages. Over 15 languages are supported in the Sentiment feature of the Text Analytics API. For more info, check out [Supported languages in the Text Analytics API](https://docs.microsoft.com//azure/cognitive-services/text-analytics/text-analytics-supported-languages). The maximum size of a single document is 5,000 characters and one request can have up to 1,000 documents. 

![Screenshot of Request Body section](../media-draft/3-marker.PNG)

The complete request, including the headers and the request URL are displayed in the next section. In this example, you can see that the requests are routed to a URL that begin with `westus`. The URL differs depending on the region you selected.  

![Section number four.](../media-draft/4-marker.PNG) 
![Section number five.](../media-draft/5-marker.PNG) 

Then we have info about the response. In the example, we see that the request was a success and code `200` was returned. We can also see that the round trip took 38 ms.

![Section number five.](../media-draft/6-marker.PNG)  

Finally, we see the response to our request. The response holds the insight the Text Analytics API had about our documents. An array of documents is returned to us, without the original text. We get back an *"id"* and *"score"* for each document. The API returns a numeric score between 0 and 1. Scores close to 1 indicate positive sentiment, while scores close to 0 indicate negative sentiment. A score of 0.5 indicates the lack of sentiment, a neutral statement. In this example,  we have two pretty positive documents and one negative document. 
![Section number five.](../media-draft/7-marker.PNG)  

Congratulations! You've made your first call to the Text Analytics API without writing a line of code. Feel free to stay in the console and try out more calls. Here are some suggestions:

- Change the documents in section number 2 and see what the API returns. 
- Try the other methods, **Detect Language**, **Entities** and **Key Phrases**, using the same  subscription key.
- Try to make a call from a different region with your subscription and observe what happens. 

The API testing console is a great way to explore the capabilities of this API. Now that you've explored for yourself, let's move on to putting this intelligence into a real-world scenario.