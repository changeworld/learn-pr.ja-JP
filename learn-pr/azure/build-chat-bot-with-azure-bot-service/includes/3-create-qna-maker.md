
[QnA Maker](https://www.qnamaker.ai/) is part of [Azure Cognitive Services](https://www.microsoft.com/cognitive-services/), which is a suite of services and APIs for building intelligent apps backed by artificial intelligence (AI) and machine learning. Rather than code a bot to anticipate every question a user might ask and provide a response, you can connect it to a knowledge base of questions and answers created with QnA Maker. A common usage scenario is to create a knowledge base from an FAQ so the bot can answer domain-specific questions such as "How do I find my Windows product key" or "Where can I download Visual Studio Code?"

In this unit, you will use QnA Maker to create a knowledge base containing questions such as "What NFL teams have won the most Super Bowls" and "What is the largest city in the world?" Then, you will deploy the knowledge base in an Azure web app so it can be accessed via an HTTPS endpoint.

1. Open the [QnA Maker portal](https://www.qnamaker.ai/) in your browser and sign in with your Microsoft account if you aren't already signed in. Then, click **Create a knowledge base** in the menu bar at the top of the page.

1. Click **Create a QnA service**.

1. Enter a name, such as "qa-factbot-kb", into the **Name** box. This name must be unique within Azure, so make sure a green check mark appears next to it *and* in the **App name** box further down the blade. Select **Use existing** under **Resource group**, and then select the "factbot-rg" resource group that you created when you deployed the Azure web app bot in the prior exercise. Select **F0** and **F** as the pricing tiers. (Both are free tiers that are ideal for experimenting with bots.) Select the location nearest you in both location drop-downs, and then click the **Create** button at the bottom of the blade.

    ![Screenshot of the Azure portal showing the QnA Maker Create blade with configuration values as described.](../media/3-new-qna-maker-service.png)

1. Click **Resource groups** in the ribbon on the left side of the portal and open the "factbot-rg" resource group. Wait until "Deploying" changes to "Succeeded" at the top of the blade, indicating that the QnA service and the resources associated with it were successfully deployed. Once more, you can click **Refresh** at the top of the blade to refresh the deployment status.

1. Return to the [Create a knowledge base](https://www.qnamaker.ai/Create) page in the QnA Maker portal. Under **Azure QnA service**, select the QnA service name you specified in Step 3. Then, assign the knowledge base a name, such as "Factbot Knowledge Base."

1. You can enter questions and answers into a QnA Maker knowledge base manually, or you can import them from online FAQs or local files. Supported formats include tab-delimited text files, Microsoft Word documents, Excel spreadsheets, and PDF files.

    To demonstrate, [click here](https://topcs.blob.core.windows.net/public/bots-resources.zip) to download a zip file containing a text file named **Factbot.tsv**, and then copy the file to your computer. Then, scroll down in the QnA Maker portal, click **+ Add file**, and select **Factbot.tsv**. This file contains 20 questions and answers in tab-delimited format.

1. Click **Create your KB** at the bottom of the page and wait for the knowledge base to be created. It should take less than a minute.

1. Confirm that the questions and answers imported from **Factbot.tsv** appear in the knowledge base. Then, click **Save and train** and wait for training to complete.

    ![Screenshot of the Factbot Knowledge Base site showing the loaded knowledge base data.](../media/3-save-and-train.png)

1. Click the **Test** button to the right of the **Save and train** button. Type "Hi" into the message box and press **Enter**. Confirm that the response is "Welcome to the QnA Factbot," as shown below.

    ![Screenshot of a test interaction with the created chat bot.](../media/3-test-kb.png)

1. Type "What book has sold the most copies?" into the message box and press **Enter**. What is the response?

1. Click the **Test** button again to collapse the Test panel. Then, click **Publish** in the menu at the top of the page and click the **Publish** button at the bottom of the page to publish the knowledge base. *Publishing* makes the knowledge base available at an HTTPS endpoint.

Wait for the publication process to complete and confirm that you are told the QnA service has been deployed. With the knowledge base now hosted in an Azure web app of its own, the next step is to deploy a bot that can use it.
