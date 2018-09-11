<span data-ttu-id="d32a9-101">Xamarin および Azure 関数と Twilio バインディングを使用してクロスプラットフォーム モバイル アプリを正しく作成できました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-101">You've successfully created a cross-platform mobile app using Xamarin and an Azure function with a Twilio binding.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="d32a9-102">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="d32a9-102">Clean up resources</span></span>

<span data-ttu-id="d32a9-103">この Azure Functions アプリケーションの作業が終わったら、チュートリアルの間に作成したすべてのリソースを、Azure portal を使用して削除できます。</span><span class="sxs-lookup"><span data-stu-id="d32a9-103">When you're done working with this Azure Functions application, you can delete all resources created during the tutorial from the Azure portal.</span></span>

1. <span data-ttu-id="d32a9-104">Visual Studio で、*[表示] > [Cloud Explorer]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="d32a9-104">From Visual Studio, select *View->Cloud Explorer*.</span></span>

2. <span data-ttu-id="d32a9-105">このパネルの上部にあるドロップダウン リストから、*[リソース グループ]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="d32a9-105">From the drop-down at the top of this panel, select *Resource Groups*.</span></span>

3. <span data-ttu-id="d32a9-106">リソース グループの作成に使用したサブスクリプションを展開します。</span><span class="sxs-lookup"><span data-stu-id="d32a9-106">Expand the subscription that you used to create the resource group.</span></span> <span data-ttu-id="d32a9-107">"ImHere" リソース グループを右クリックし、*[ポータルで開く]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="d32a9-107">Right-click on the "ImHere" resource group and select *Open in Portal*.</span></span>

    ![Cloud Explorer ウィンドウからポータルでリソース グループを開く](../media-drafts/9-open-resource-group-in-portal.png)

4. <span data-ttu-id="d32a9-109">必要な場合は、ブラウザーで Azure portal にログインします。</span><span class="sxs-lookup"><span data-stu-id="d32a9-109">Log into the Azure portal in your browser, if necessary.</span></span>

5. <span data-ttu-id="d32a9-110">ポータルで "ImHere" リソース グループが開きます。</span><span class="sxs-lookup"><span data-stu-id="d32a9-110">The portal will open on the "ImHere" resource group.</span></span> <span data-ttu-id="d32a9-111">**[リソース グループの削除]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d32a9-111">Click the **Delete Resource Group** button.</span></span>

    ![リソース グループを削除します](../media-drafts/9-delete-resource-group.png)

6. <span data-ttu-id="d32a9-113">リソース グループの名前を入力して削除を確認し、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d32a9-113">Enter the name of the resource group to confirm the deletion and click **Delete**.</span></span>

    ![リソース グループの名前を入力して、削除を確定します。](../media-drafts/9-confirm-delete-resource-group.png)

## <a name="summary"></a><span data-ttu-id="d32a9-115">まとめ</span><span class="sxs-lookup"><span data-stu-id="d32a9-115">Summary</span></span>

<span data-ttu-id="d32a9-116">このチュートリアルで学習した内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d32a9-116">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d32a9-117">Xamarin.Essentials を使用するクロスプラットフォームの Xamarin.Forms アプリを作成しました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-117">Create a cross-platform Xamarin.Forms app that uses Xamarin.Essentials.</span></span>
> * <span data-ttu-id="d32a9-118">ViewModel のアプリケーション ロジックを含む XAML を使用してクロスプラットフォームの UI を作成し、UI に ViewModel のプロパティをバインドしました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-118">Create a cross-platform UI using XAML with application logic in a ViewModel, as well as bind properties in a ViewModel to the UI.</span></span>
> * <span data-ttu-id="d32a9-119">ユーザーの場所を検出しました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-119">Detect the user's location.</span></span>
> * <span data-ttu-id="d32a9-120">HTTP トリガーを含む Azure 関数を作成し、ローカルで実行しました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-120">Create an Azure function with an HTTP trigger and run it locally.</span></span>
> * <span data-ttu-id="d32a9-121">モバイル アプリから Azure 関数を呼び出し、JSON としてデータを渡しました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-121">Call an Azure function from a mobile app, passing data as JSON.</span></span>
> * <span data-ttu-id="d32a9-122">Azure 関数を Twilio にバインドして SMS メッセージを送信しました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-122">Bind an Azure function to Twilio to send an SMS message.</span></span>
> * <span data-ttu-id="d32a9-123">Azure 関数を Azure に発行しました。</span><span class="sxs-lookup"><span data-stu-id="d32a9-123">Publish an Azure function to Azure.</span></span>
