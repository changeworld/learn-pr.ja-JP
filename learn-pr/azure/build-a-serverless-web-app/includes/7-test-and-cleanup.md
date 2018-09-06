<span data-ttu-id="ff9fb-101">Azure サービスを使用して、多彩な機能を備えたサーバーレス アプリケーションを作成できました。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-101">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="ff9fb-102">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="ff9fb-102">Clean up resources</span></span>

<span data-ttu-id="ff9fb-103">このアプリケーションの作業が完了したら、次のコマンドを使用して、このチュートリアルで作成したすべてのリソースを削除できます。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-103">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="ff9fb-104">プロンプトが表示されたら 「`y`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-104">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="ff9fb-105">次の手順</span><span class="sxs-lookup"><span data-stu-id="ff9fb-105">Next steps</span></span>

<span data-ttu-id="ff9fb-106">このモジュールでは、次の方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-106">In this module, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ff9fb-107">静的な Web サイトとアップロードされた画像がホストされるように Azure Blob Storage を構成します。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-107">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="ff9fb-108">Azure Functions を使用して、画像を Azure Blob Storage にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-108">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="ff9fb-109">Azure Functions を使用して、画像のサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-109">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="ff9fb-110">画像のメタデータを Azure Cosmos DB に格納します。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-110">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="ff9fb-111">Cognitive Services Vision API を使用して、画像のキャプションを自動生成します。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-111">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="ff9fb-112">Azure Active Directory を使用してユーザーを認証することにより、Web アプリをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-112">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="ff9fb-113">さらに多くのサービスを Functions に接続する方法については、Logic Apps のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="ff9fb-113">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="ff9fb-114">Functions と Logic Apps</span><span class="sxs-lookup"><span data-stu-id="ff9fb-114">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)