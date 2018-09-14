<span data-ttu-id="84b35-101">Azure Event Hubs を使用すると、大量のデータを処理する機能を備えたビッグ データ アプリケーションを実現することができます。</span><span class="sxs-lookup"><span data-stu-id="84b35-101">Azure Event Hubs provides big data applications the capability to process large volume of data.</span></span> <span data-ttu-id="84b35-102">Azure Event Hubs には、需要が著しく高い期間中に必要に応じてスケール アウトする機能もあります。</span><span class="sxs-lookup"><span data-stu-id="84b35-102">It also has the ability to scale out during exceptionally high- demand periods as and when required.</span></span> <span data-ttu-id="84b35-103">Azure Event Hubs では、データ処理を管理するために送信メッセージと受信メッセージが分離されます。</span><span class="sxs-lookup"><span data-stu-id="84b35-103">Azure Event Hubs decouple the sending and receiving messages to manage the data processing.</span></span> <span data-ttu-id="84b35-104">これは、予期しない中断が原因でコンシューマー アプリケーションが過負荷状態になったりデータ損失が発生したりするリスクを排除するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="84b35-104">This helps eliminate the risk of overwhelming consumer application and data loss due to any unplanned interruptions.</span></span>

<span data-ttu-id="84b35-105">このモジュールでは、イベント処理ソリューションの一部として Azure Event Hubs をデプロイする方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="84b35-105">In this module, you've seen how to deploy Azure Event Hubs as part of an event processing solution.</span></span> <span data-ttu-id="84b35-106">以下の方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="84b35-106">You learned how to:</span></span>

- <span data-ttu-id="84b35-107">Azure CLI コマンドを使用して、Event Hubs 名前空間を作成し、さらにその名前空間内にイベント ハブを作成する。</span><span class="sxs-lookup"><span data-stu-id="84b35-107">Use the Azure CLI commands to create an Event Hubs namespace and an event hub in that namespace.</span></span> 
- <span data-ttu-id="84b35-108">そのイベント ハブを通してメッセージを送受信するように送信側アプリケーションと受信側アプリケーションを構成する。</span><span class="sxs-lookup"><span data-stu-id="84b35-108">Configure sender and receiver applications to send and receive messages through the event hub.</span></span>
- <span data-ttu-id="84b35-109">Azure portal を使用してご利用のイベント ハブの状態とパフォーマンスを表示する。</span><span class="sxs-lookup"><span data-stu-id="84b35-109">Use the Azure portal to view your event hub status and performance.</span></span>

## <a name="clean-up"></a><span data-ttu-id="84b35-110">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="84b35-110">Clean up</span></span> 

<span data-ttu-id="84b35-111">ご利用のイベント ハブのテストに使用したリソースには、サブスクリプションに対してコストが発生します。</span><span class="sxs-lookup"><span data-stu-id="84b35-111">The resources you've used for your event hub testing will incur costs against your subscription.</span></span> <span data-ttu-id="84b35-112">イベント ハブでの作業が完了したら、不要なコストがかからないようにそれらのリソースを必ず削除してください。</span><span class="sxs-lookup"><span data-stu-id="84b35-112">When you've have finished with the event hub, remember to remove these resources in order to avoid unnecessary charges.</span></span>

<span data-ttu-id="84b35-113">ハブ、名前空間、およびストレージを同じリソースグループ内で作成したので、Azure サブスクリプションをクリーンアップする最も簡単な方法は、リソース グループを削除することです。これにより、そのすべてのコンテンツが削除されます。</span><span class="sxs-lookup"><span data-stu-id="84b35-113">Because you create the hub, namespace, and storage in the same resource group, the easiest way to clean up your Azure subscription is to remove the resource group, which will remove all its contents.</span></span> 

<span data-ttu-id="84b35-114">次のコマンドを実行して、リソース グループ、名前空間、ストレージ アカウント、およびすべての関連リソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="84b35-114">Run the following command to remove the resource group, namespace, storage account, and all related resources.</span></span> <span data-ttu-id="84b35-115">`myResourceGroup` は、作成したリソース グループの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="84b35-115">Replace `myResourceGroup` with the name of the resource group you created:</span></span>

```azurecli
az group delete --resource-group myResourceGroup
```

<span data-ttu-id="84b35-116">削除の確認を求められたら、**[はい]** と答えます。</span><span class="sxs-lookup"><span data-stu-id="84b35-116">When you are asked to confirm the delete, answer **Yes**.</span></span>

<span data-ttu-id="84b35-117">リソースが削除済みとなってコマンドが完了するまで、数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="84b35-117">The command may take several minutes to complete as resources are deleted.</span></span>