<span data-ttu-id="5cc0f-101">このモジュールでは、信頼性と回復力の高い分散アプリケーションを作成するための 4 つの異なる Azure サービスについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-101">In this module, you have explored four different Azure services that allow you to create reliable and resilient distributed applications.</span></span> <span data-ttu-id="5cc0f-102">これらのサービス間で選択を行うことは、コンポーネント間で渡す必要のあるデータの種類 (メッセージまたはイベント) を判断し、そのデータを配信および処理するのに必要な機能を判断するということです。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-102">Choosing between them is a matter of deciding the type of data that needs to be passed between components (messages or events), and then what features you need to deliver and process the data.</span></span>

## <a name="clean-up"></a><span data-ttu-id="5cc0f-103">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="5cc0f-103">Clean up</span></span>

<span data-ttu-id="5cc0f-104">ごく少量のメッセージが含まれている小さなキューにかかるコストはわずかですが、それでもストレージ アカウントにデータが含まれていれば Azure サブスクリプションに対してコストが発生します。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-104">While a Storage Account contains data, it incurs a cost against your Azure subscription, although these are likely to be low for small queue with few messages.</span></span> <span data-ttu-id="5cc0f-105">キューが完了したら、不要なコストがかからないように必ず削除してください。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-105">When you have finished with the queue, remember to remove it in order to avoid unnecessary charges.</span></span> <span data-ttu-id="5cc0f-106">すべてのリソースを同じリソースグループ内で作成したので、Azure サブスクリプションをクリーンアップする最も簡単な方法は、リソース グループを削除することです。これにより、そのすべてのコンテンツが削除されます。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-106">Because you created all the resources in the same resource group, the easiest way to clean up your Azure subscription is to remove the resource group which will remove all its contents:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```

<span data-ttu-id="5cc0f-107">削除の確認を求められたら、**[はい]** と答えます。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-107">When you are asked to confirm the delete, answer **Yes**.</span></span>

<span data-ttu-id="5cc0f-108">リソースが削除済みとなってコマンドが完了するまで、数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5cc0f-108">The command may take several minutes to complete as resources are deleted.</span></span>