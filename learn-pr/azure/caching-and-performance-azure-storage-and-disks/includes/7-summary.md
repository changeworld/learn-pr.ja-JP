<span data-ttu-id="90bfd-101">このモジュールでは、Azure ディスク キャッシュと、それによりパフォーマンスを向上させる可能性について学習しました。</span><span class="sxs-lookup"><span data-stu-id="90bfd-101">In this module, you learned about Azure disk caching and how it potentially improves performance.</span></span> <span data-ttu-id="90bfd-102">VM のディスク キャッシュの管理に、Azure portal と Azure PowerShell を使用しました。</span><span class="sxs-lookup"><span data-stu-id="90bfd-102">We used the Azure portal and Azure PowerShell to manage disk caching for our VM.</span></span> 

<span data-ttu-id="90bfd-103">Azure VM ディスク キャッシュの方法を決めると、スクリプトとテンプレートを使用して、最適なディスク キャッシュ設定で新しい VM とディスクを迅速かつ簡単にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="90bfd-103">Once you have Azure VM disk caching strategy in place, you can then quickly and easily deploy new VMs and disk with the optimum disk cache settings, by using scripts and templates.</span></span>

## <a name="further-reading"></a><span data-ttu-id="90bfd-104">参考資料</span><span class="sxs-lookup"><span data-stu-id="90bfd-104">Further reading</span></span>

- [<span data-ttu-id="90bfd-105">Azure Premium Storage: 高パフォーマンスのための設計</span><span class="sxs-lookup"><span data-stu-id="90bfd-105">Azure Premium Storage: Design for High Performance</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance)
- [<span data-ttu-id="90bfd-106">Azure PowerShell の概要</span><span class="sxs-lookup"><span data-stu-id="90bfd-106">Get started with Azure PowerShell</span></span>](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.8.1)
- [<span data-ttu-id="90bfd-107">Azure Computer コマンドレット リファレンス</span><span class="sxs-lookup"><span data-stu-id="90bfd-107">Azure Computer Cmdlets Reference</span></span>](https://docs.microsoft.com/powershell/module/azurerm.compute/?view=azurermps-6.8.1#vm_disks)


## <a name="cleanup"></a><span data-ttu-id="90bfd-108">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="90bfd-108">Cleanup</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="90bfd-109">Azure VM を実行すると、サブスクリプションに対してコストが発生します。</span><span class="sxs-lookup"><span data-stu-id="90bfd-109">Running Azure VMs incurs costs against your subscription.</span></span> <span data-ttu-id="90bfd-110">不必要な課金を避けるために不要なリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="90bfd-110">Remove unneeded resources to avoid unnecessary charges.</span></span> <span data-ttu-id="90bfd-111">Azure サブスクリプションをクリーンアップする最も簡単な方法は、リソース グループを削除することです。</span><span class="sxs-lookup"><span data-stu-id="90bfd-111">The easiest way to clean up your Azure subscription is to remove the resource group.</span></span> <span data-ttu-id="90bfd-112">リソース グループを削除すると、そのグループ内にあるすべてのリソースが削除されます。</span><span class="sxs-lookup"><span data-stu-id="90bfd-112">Deleting a resource group deletes all the resources in that group.</span></span> <span data-ttu-id="90bfd-113">このモジュールが終了したら、次の Azure PowerShell コマンドレットを実行します。</span><span class="sxs-lookup"><span data-stu-id="90bfd-113">When you're finished with this module, run the following Azure PowerShell cmdlet:</span></span>

    ```powershell
    Remove-AzureRmResourceGroup -Name "fotoshare-rg"
    ```

<span data-ttu-id="90bfd-114">削除の確認を求められたら、**[はい]** と答えます。</span><span class="sxs-lookup"><span data-stu-id="90bfd-114">When asked to confirm the delete, answer **Yes**.</span></span> <span data-ttu-id="90bfd-115">リソースが削除済みとなってコマンドが完了するまで、数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="90bfd-115">The command may take several minutes to complete as resources are deleted.</span></span>
