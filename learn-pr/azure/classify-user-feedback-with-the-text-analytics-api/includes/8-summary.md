<span data-ttu-id="2c008-101">Microsoft Cognitive Services は、アプリの強化に利用できる充実したインテリジェントなサービス スイートです。</span><span class="sxs-lookup"><span data-stu-id="2c008-101">Microsoft Cognitive Services is a rich suite of intelligent services that we can use to enrich our apps.</span></span> <span data-ttu-id="2c008-102">Text Analytics API サービスのごく一部を利用して､テキストの高次の情報を調べてみました｡</span><span class="sxs-lookup"><span data-stu-id="2c008-102">We explored a small part of the Text Analytics API service to find out higher-level information about text.</span></span> <span data-ttu-id="2c008-103">お客様からのテキスト形式のフィードバックを感情面から分析しました。</span><span class="sxs-lookup"><span data-stu-id="2c008-103">We used the service to analyze text feedback from customers for sentiment.</span></span> <span data-ttu-id="2c008-104">それらテキスト メッセージをさらに処理するために､いくつかのバケット､あるいはキューに仕分けするため､Azure Functions でホストされるソリューションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="2c008-104">We created a solution hosted in Azure Functions to sort these text messages into different buckets, or queues, for further processing.</span></span>

<span data-ttu-id="2c008-105">REST API の呼び出し方法が分かると､これらのインテリジェント サービスをソリューションに簡単に組み込むことができます｡</span><span class="sxs-lookup"><span data-stu-id="2c008-105">Once you know how to call a REST API, then you can easily integrate these intelligent services into your solutions.</span></span> <span data-ttu-id="2c008-106">こうしたことのすべてが類似のパターンに従っています｡</span><span class="sxs-lookup"><span data-stu-id="2c008-106">They all follow a similar pattern:</span></span>

- <span data-ttu-id="2c008-107">サインアップしてアクセス キーを入手する。</span><span class="sxs-lookup"><span data-stu-id="2c008-107">Sign up for an access key</span></span>
- <span data-ttu-id="2c008-108">API のテスト コンソールに表示する。</span><span class="sxs-lookup"><span data-stu-id="2c008-108">Explore in the API testing console</span></span>
- <span data-ttu-id="2c008-109">アクセス キーと適切なリージョンを使用して要求を作成する。</span><span class="sxs-lookup"><span data-stu-id="2c008-109">Formulate requests using the access key and the correct region.</span></span>
- <span data-ttu-id="2c008-110">ソリューションからのリクエストを POST し、応答を解析して洞察を得る｡</span><span class="sxs-lookup"><span data-stu-id="2c008-110">POST requests from your solution and parse the responses for insights.</span></span>

<span data-ttu-id="2c008-111">私たちはこの情報を Azure Functions に作成したサーバーレス ロジックに追加しました。</span><span class="sxs-lookup"><span data-stu-id="2c008-111">We added this intelligence to serverless logic created in Azure Functions.</span></span> <span data-ttu-id="2c008-112">これらのサービスは､他の種類のアプリからも簡単に呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="2c008-112">You can easily call these services from other types of apps.</span></span> <span data-ttu-id="2c008-113">すぐに取り組めるよう､数多くのクライアント ライブラリやチュートリアル、クイックスタートがあります｡</span><span class="sxs-lookup"><span data-stu-id="2c008-113">There are many client libraries, tutorials and,  quickstarts to get you started.</span></span>

## <a name="suggestions-for-further-enhancement-of-our-solution"></a><span data-ttu-id="2c008-114">ソリューションをさらに充実させるために</span><span class="sxs-lookup"><span data-stu-id="2c008-114">Suggestions for further enhancement of our solution</span></span>

<span data-ttu-id="2c008-115">ここでは、これまでに行ったことをさらに深めたい場合に検討すべきいくつかのアイデアを紹介します｡</span><span class="sxs-lookup"><span data-stu-id="2c008-115">Here are some ideas for you to consider if you want to take what we did further.</span></span> 

- <span data-ttu-id="2c008-116">もっと多くのテキスト例でソリューションをテストし､感情点数を肯定的､否定的､中立的に分類するために設定したしきい値が適切かどうかを判断してください｡</span><span class="sxs-lookup"><span data-stu-id="2c008-116">Test the solution with more text examples and decide whether the thresholds we set to categorize sentiment scores into positive, negative, and neutral are appropriate.</span></span> 
- <span data-ttu-id="2c008-117">[!INCLUDE [negative-q](./q-name-negative.md)] キューからメッセージを読み取り､Text Analytics API の呼び出て､テキスト内の鍵となる語句を探す関数を関数アプリに追加することを検討してください｡</span><span class="sxs-lookup"><span data-stu-id="2c008-117">Consider adding another function into your function app that reads messages from the [!INCLUDE [negative-q](./q-name-negative.md)] queue and calls the Text Analytics API to find key phrases in the text.</span></span>
- <span data-ttu-id="2c008-118">この入力キューには、未加工のテキスト フィードバックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2c008-118">Our input queue contains raw text feedback.</span></span> <span data-ttu-id="2c008-119">現実世界では､フィードバックは電子メール アドレスやアカウント番号などの何らかの形式のユーザー識別情報に関連付けられます｡</span><span class="sxs-lookup"><span data-stu-id="2c008-119">In the real-world, we would associate feedback with some form of user ID such as email address, account number, and so on.</span></span> <span data-ttu-id="2c008-120">このため、入力キュー項目を強化して､ID フィールドとテキストを含む JSON ドキュメントにします｡</span><span class="sxs-lookup"><span data-stu-id="2c008-120">So, enhance the input queue items to be JSON documents containing and ID field and the text.</span></span> <span data-ttu-id="2c008-121">こうして､テキスト メッセージを操作するときはその ID を利用します。</span><span class="sxs-lookup"><span data-stu-id="2c008-121">Then use that ID when working with the text message.</span></span>
 - <span data-ttu-id="2c008-122">現在、私たちのソリューションは英語にハード コーディングされています｡</span><span class="sxs-lookup"><span data-stu-id="2c008-122">Currently our solution is "hard coded" to English.</span></span> <span data-ttu-id="2c008-123">Text Analytics API でサポートされているすべての言語のテキストを処理できるようにするためには､どのような変更を行えばよいのか考えてみてくだます。</span><span class="sxs-lookup"><span data-stu-id="2c008-123">Think about what changes you would do to make it capable or handling text in all languages supported by the Text Analytics API.</span></span>  

<span data-ttu-id="2c008-124">これらの Cognitive Services API の呼び出し方法が分かったわけですから、他のサービスもいくつか検討し､それらをソリューションで利用する方法を考えてください｡</span><span class="sxs-lookup"><span data-stu-id="2c008-124">Now that you know how to call one of these Cognitive Services APIs, take a look at some of the other services and think about how you might use them in your solutions.</span></span> 

## <a name="further-reading"></a><span data-ttu-id="2c008-125">参考資料</span><span class="sxs-lookup"><span data-stu-id="2c008-125">Further reading</span></span>

- [<span data-ttu-id="2c008-126">Text Analytics の概要</span><span class="sxs-lookup"><span data-stu-id="2c008-126">Text Analytics overview</span></span>](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [<span data-ttu-id="2c008-127">Text Analytics で感情を検出する方法</span><span class="sxs-lookup"><span data-stu-id="2c008-127">How to detect sentiment in Text Analytics</span></span>](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [<span data-ttu-id="2c008-128">Cognitive Services のドキュメント</span><span class="sxs-lookup"><span data-stu-id="2c008-128">Cognitive Services Documentation</span></span>](https://docs.microsoft.com/azure/cognitive-services/)

## <a name="clean-up-resources"></a><span data-ttu-id="2c008-129">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="2c008-129">Clean up resources</span></span>

<span data-ttu-id="2c008-130">Azure の *リソース*とは、Function App や関数、ストレージ アカウントなどのことを意味します。</span><span class="sxs-lookup"><span data-stu-id="2c008-130">*Resources* in Azure refer to function apps, functions, storage accounts, and so forth.</span></span> <span data-ttu-id="2c008-131">これらは*リソース グループ*に分類されており、グループを削除することでグループ内のすべてのものを削除できます。</span><span class="sxs-lookup"><span data-stu-id="2c008-131">They are grouped into *resource groups*, and you can delete everything in a group by deleting the group.</span></span>

<span data-ttu-id="2c008-132">あなたは､このモジュールを終了するためにリソースを作成しました｡</span><span class="sxs-lookup"><span data-stu-id="2c008-132">You created resources to complete this module.</span></span> <span data-ttu-id="2c008-133">[アカウントの状態](https://azure.microsoft.com/account/)と[サービスの価格](https://azure.microsoft.com/pricing/)によっては､それらリソースは課金されてることがあります｡</span><span class="sxs-lookup"><span data-stu-id="2c008-133">You may be billed for these resources, depending on your [account status](https://azure.microsoft.com/account/) and [service pricing](https://azure.microsoft.com/pricing/).</span></span> <span data-ttu-id="2c008-134">リソースの必要がなくなった場合にそれらを削除する方法を、次に示します。</span><span class="sxs-lookup"><span data-stu-id="2c008-134">If you don't need the resources anymore, here's how to delete them:</span></span>

1. <span data-ttu-id="2c008-135">Azure Portal で、**[リソース グループ]** ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="2c008-135">In the Azure portal, go to the **Resource group** page.</span></span>

   <span data-ttu-id="2c008-136">Function App ページからこのページに移動するには、**[概要]** タブを選択してから、**[リソース グループ]** の下にあるリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c008-136">To get to that page from the function app page, select the **Overview** tab and then select the link under **Resource group**.</span></span>

   <span data-ttu-id="2c008-137">ダッシュボードからこのページに移動するには、**[リソース グループ]** を選択してから、このモジュールで使用したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c008-137">To get to that page from the dashboard, select **Resource groups**, and then select the resource group that you used for this module.</span></span> 

> [!NOTE]
> <span data-ttu-id="2c008-138">このモジュールで推奨された既定のリソース グループ名は [!INCLUDE [resource-group-name](./rg-name.md)] でしたが､別の名前を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="2c008-138">The default name of the resource group we suggested for this module was [!INCLUDE [resource-group-name](./rg-name.md)] but it is possible that you used another name.</span></span>

2. <span data-ttu-id="2c008-139">**[リソース グループ]** ページで、含まれているリソースの一覧を確認し、削除してもよいリソースであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2c008-139">In the **Resource group** page, review the list of included resources, and verify that they are the ones you want to delete.</span></span>

3. <span data-ttu-id="2c008-140">**[リソース グループの削除]** を選択し、指示に従います。</span><span class="sxs-lookup"><span data-stu-id="2c008-140">Select **Delete resource group**, and follow the instructions.</span></span>

   <span data-ttu-id="2c008-141">削除には数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="2c008-141">Deletion may take a couple of minutes.</span></span> <span data-ttu-id="2c008-142">実行されると、通知が数秒間表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c008-142">When it's done, a notification appears for a few seconds.</span></span> <span data-ttu-id="2c008-143">ページの上部にあるベルのアイコンを選択することで、通知を表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="2c008-143">You can also select the bell icon at the top of the page to view the notification.</span></span>

## <a name="further-reading"></a><span data-ttu-id="2c008-144">参考資料</span><span class="sxs-lookup"><span data-stu-id="2c008-144">Further Reading</span></span>

<span data-ttu-id="2c008-145">このリストはすべての資料を網羅するものではありません｡以下はこのモジュールで取り上げられ､ユーザーに有用と思われるトピックに関係しているリソースです｡</span><span class="sxs-lookup"><span data-stu-id="2c008-145">While this is not intended to be an exhaustive list, the following are some resources related to the topics covered in this module that you might find interesting.</span></span>

 * [<span data-ttu-id="2c008-146">Azure Functions のドキュメント</span><span class="sxs-lookup"><span data-stu-id="2c008-146">Azure Functions documentation</span></span>](https://docs.microsoft.com/azure/azure-functions/)

* [<span data-ttu-id="2c008-147">Azure Functions の課題</span><span class="sxs-lookup"><span data-stu-id="2c008-147">The Azure Functions Challenge</span></span>](https://aka.ms/afc)

* [<span data-ttu-id="2c008-148">Azure のサーバーレス コンピューティング クックブック</span><span class="sxs-lookup"><span data-stu-id="2c008-148">Azure Serverless Computing Cookbook</span></span>](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [<span data-ttu-id="2c008-149">Node.js から Queue ストレージを使用する方法</span><span class="sxs-lookup"><span data-stu-id="2c008-149">How to use Queue storage from Node.js</span></span>](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [<span data-ttu-id="2c008-150">Azure Cosmos DB の概要: SQL API</span><span class="sxs-lookup"><span data-stu-id="2c008-150">Introduction to Azure Cosmos DB: SQL API</span></span>](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [<span data-ttu-id="2c008-151">Azure Cosmos DB の技術概要</span><span class="sxs-lookup"><span data-stu-id="2c008-151">A technical overview of Azure Cosmos DB</span></span>](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [<span data-ttu-id="2c008-152">Azure Cosmos DB のドキュメント</span><span class="sxs-lookup"><span data-stu-id="2c008-152">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/azure/cosmos-db/)
