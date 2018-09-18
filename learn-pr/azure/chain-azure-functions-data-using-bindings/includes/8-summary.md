<span data-ttu-id="9ec44-101">このモジュールでは、関数へのデータとサービスの統合について説明しました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-101">This module was all about integrating data and services into your functions.</span></span> <span data-ttu-id="9ec44-102">バインディングの種類と、どのような場合にそれらを関数に追加するのかを説明するクイック ツアーから始めました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-102">We started off with a quick tour of the binding types that show up when you add them to a function.</span></span> <span data-ttu-id="9ec44-103">次に、入力バインディングを使用して、Azure Cosmos DB からデータを読み取る方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-103">We then looked at reading data from an Azure Cosmos DB using an input binding.</span></span> <span data-ttu-id="9ec44-104">接続文字列の管理はプラットフォームによって行われるので、バインディングを使用してコード内でデータを読み取ることがいかに簡単であるかがわかりました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-104">The platform takes care of managing connection strings and we saw how easy it is to read data in our code using the binding.</span></span> <span data-ttu-id="9ec44-105">最後に、出力バインディングを使用して、さまざまなソースにデータを書き込む方法に注目しました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-105">Finally we focused our attention on writing data different sources with the help of output bindings.</span></span> <span data-ttu-id="9ec44-106">この学習内容を次の表にまとめます。</span><span class="sxs-lookup"><span data-stu-id="9ec44-106">This journey is summarized in the following table.</span></span>

[!INCLUDE [summary table](./summary-table.md)]

<span data-ttu-id="9ec44-107">ここで学習した方法を適用して、関数にバインディングを追加し、テストすることができます。</span><span class="sxs-lookup"><span data-stu-id="9ec44-107">You can apply the approaches you have learned here to add and test bindings in your functions.</span></span> <span data-ttu-id="9ec44-108">バインディングの使い方をさらに練習し、ここで学んだことを活かすための興味深いアイデアをいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-108">Here are a few interesting ideas to get more practice with bindings and to build on what you have learned here.</span></span>

* <span data-ttu-id="9ec44-109">Blob Storage から読み取る別の関数と、このモジュールで使用しなかった他の入力バインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-109">Create another function to read from Blob Storage and other input bindings that we haven't used in this module.</span></span>

* <span data-ttu-id="9ec44-110">サポートされている他の出力バインディングの種類を使用して、より多くの送信先に書き込む別の関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-110">Create another function to write to more destinations using other supported output binding types.</span></span>

* <span data-ttu-id="9ec44-111">最後のユニットでは、キューを導入し、出力バインディングを使用してキューにメッセージを送信しました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-111">In the last unit, we introduced a queue and posted messages to it with an output binding.</span></span> <span data-ttu-id="9ec44-112">次のステップとして、キュー内のメッセージを読み取り、`Console.Log()` を使用して**メッセージ テキスト**をコンソールに出力する別の関数を追加することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="9ec44-112">As a next step, consider adding another function that reads the messages in the queue and prints out the **MESSAGE TEXT** to the console with `Console.Log()`.</span></span>

<span data-ttu-id="9ec44-113">このモジュールで説明したように、Azure portal には、関数を作成し、それらをデータや他のサービスに接続する作業を開始するための使いやすい機能が用意されています。</span><span class="sxs-lookup"><span data-stu-id="9ec44-113">As we saw in this module, the Azure portal offers easy-to-use features to start building functions and connecting them to data and other services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="9ec44-114">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="9ec44-114">Clean up resources</span></span>

<span data-ttu-id="9ec44-115">Azure の "*リソース*" とは、関数アプリ、関数、ストレージ アカウントなどを指します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-115">*Resources* in Azure refers to function apps, functions, storage accounts, and so forth.</span></span> <span data-ttu-id="9ec44-116">これらは "*リソース グループ*" に分類されており、グループを削除することでグループ内のすべてのものを削除できます。</span><span class="sxs-lookup"><span data-stu-id="9ec44-116">They are grouped into *resource groups*, and you can delete everything in a group by deleting the group.</span></span>

<span data-ttu-id="9ec44-117">このモジュールを完了するためにリソースを作成しました。</span><span class="sxs-lookup"><span data-stu-id="9ec44-117">You created resources to complete this module.</span></span> <span data-ttu-id="9ec44-118">これらのリソースには、[アカウントの状態](https://azure.microsoft.com/account/)と[サービスの価格](https://azure.microsoft.com/pricing/)に応じて課金される場合があります。</span><span class="sxs-lookup"><span data-stu-id="9ec44-118">You may be billed for these resources, depending on your [account status](https://azure.microsoft.com/account/) and [service pricing](https://azure.microsoft.com/pricing/).</span></span> <span data-ttu-id="9ec44-119">リソースの必要がなくなった場合にそれらを削除する方法を、次に示します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-119">If you don't need the resources anymore, here's how to delete them:</span></span>

1. <span data-ttu-id="9ec44-120">Azure Portal で、**[リソース グループ]** ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-120">In the Azure portal, go to the **Resource group** page.</span></span>

   <span data-ttu-id="9ec44-121">Function App ページからこのページに移動するには、**[概要]** タブを選択し、**[リソース グループ]** の下のリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-121">To get to that page from the function app page, select the **Overview** tab and then select the link under **Resource group**.</span></span>

   <span data-ttu-id="9ec44-122">ダッシュボードからこのページに移動するには、**[リソース グループ]** を選択し、このモジュールで使用したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-122">To get to that page from the dashboard, select **Resource groups**, and then select the resource group that you used for this module.</span></span> 

> [!NOTE]
> <span data-ttu-id="9ec44-123">このモジュールで推奨されるリソース グループの既定の名前は [!INCLUDE [resource-group-name](./rg-name.md)] でしたが、別の名前を使用していた可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9ec44-123">The default name of the resource group we suggested for this module was [!INCLUDE [resource-group-name](./rg-name.md)] but it is possible that you used another name.</span></span>

2. <span data-ttu-id="9ec44-124">**[リソース グループ]** ページで、含まれているリソースの一覧を見直し、それらが削除の対象であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9ec44-124">In the **Resource group** page, review the list of included resources, and verify that they are the ones you want to delete.</span></span>

3. <span data-ttu-id="9ec44-125">**[リソース グループの削除]** を選択し、指示に従います。</span><span class="sxs-lookup"><span data-stu-id="9ec44-125">Select **Delete resource group**, and follow the instructions.</span></span>

   <span data-ttu-id="9ec44-126">削除には数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="9ec44-126">Deletion may take a couple of minutes.</span></span> <span data-ttu-id="9ec44-127">実行されると、通知が数秒間表示されます。</span><span class="sxs-lookup"><span data-stu-id="9ec44-127">When it's done, a notification appears for a few seconds.</span></span> <span data-ttu-id="9ec44-128">ページの上部にあるベルのアイコンを選択して、通知を表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ec44-128">You can also select the bell icon at the top of the page to view the notification.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9ec44-129">参考資料</span><span class="sxs-lookup"><span data-stu-id="9ec44-129">Further Reading</span></span>

<span data-ttu-id="9ec44-130">この一覧は包括的なものではありませんが、このモジュールで取り上げたトピックに関連し、興味を持たれるかもしれないリソースを紹介しています。</span><span class="sxs-lookup"><span data-stu-id="9ec44-130">While this is not intended to be an exhaustive list, the following are some resources related to the topics covered in this module that you might find interesting.</span></span>

 * [<span data-ttu-id="9ec44-131">Azure Functions のドキュメント</span><span class="sxs-lookup"><span data-stu-id="9ec44-131">Azure Functions documentation</span></span>](https://docs.microsoft.com/azure/azure-functions/)

* [<span data-ttu-id="9ec44-132">Azure Functions Challenge</span><span class="sxs-lookup"><span data-stu-id="9ec44-132">The Azure Functions Challenge</span></span>](https://aka.ms/afc)

* [<span data-ttu-id="9ec44-133">Azure サーバーレス コンピューティング クックブック</span><span class="sxs-lookup"><span data-stu-id="9ec44-133">Azure Serverless Computing Cookbook</span></span>](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [<span data-ttu-id="9ec44-134">Node.js から Queue ストレージを使用する方法</span><span class="sxs-lookup"><span data-stu-id="9ec44-134">How to use Queue storage from Node.js</span></span>](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [<span data-ttu-id="9ec44-135">Azure Cosmos DB の概要: SQL API</span><span class="sxs-lookup"><span data-stu-id="9ec44-135">Introduction to Azure Cosmos DB: SQL API</span></span>](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [<span data-ttu-id="9ec44-136">Azure Cosmos DB の技術概要</span><span class="sxs-lookup"><span data-stu-id="9ec44-136">A technical overview of Azure Cosmos DB</span></span>](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [<span data-ttu-id="9ec44-137">Azure Cosmos DB のドキュメント</span><span class="sxs-lookup"><span data-stu-id="9ec44-137">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/azure/cosmos-db/)
