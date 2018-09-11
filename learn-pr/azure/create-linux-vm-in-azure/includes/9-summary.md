<span data-ttu-id="3b6fa-101">このモジュールでは、Azure portal を利用して Linux VM を作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-101">In this module, you learned how to create a Linux VM using the Azure portal.</span></span> <span data-ttu-id="3b6fa-102">次にその VM のパブリック IP アドレスに接続し、SSH 接続を使用して管理しました。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-102">You then connected to the public IP address of the VM and managed it with an SSH connection.</span></span> 

<span data-ttu-id="3b6fa-103">SSH では仮想マシンのオペレーティング システムやソフトウェアを操作でき、ポータルでは仮想のハードウェアと接続性を構成できることを学習しました。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-103">You learned that while SSH allows us to interact with the operating system and software of the virtual machine, the portal will enable us to configure the virtual hardware and connectivity.</span></span> <span data-ttu-id="3b6fa-104">コマンドラインやスクリプト可能環境を望む場合、PowerShell や Azure CLI も使用できました。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-104">We also could have used PowerShell or the Azure CLI, if a command-line or scriptable environment were preferred.</span></span>

## <a name="clean-up-the-resources"></a><span data-ttu-id="3b6fa-105">リソースをクリーンアップする</span><span class="sxs-lookup"><span data-stu-id="3b6fa-105">Clean up the resources</span></span>

<span data-ttu-id="3b6fa-106">VM は実行時間に対して課金され、ストレージは使用量に基づいて課金されます。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-106">You are charged for VMs while they run, and for the storage based on how much you use.</span></span> <span data-ttu-id="3b6fa-107">VM を使用していないときには常に停止して割り当てを解除してください。リソースが今後必要でなくなったら、削除することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-107">Always stop and deallocate VMs when you aren't using them, and when you no longer need the resources, it's a good idea to delete them.</span></span> <span data-ttu-id="3b6fa-108">作成したすべてのリソースを削除する場合、リソースを 1 つずつ削除する方法と、リソース グループを削除する方法があります。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-108">To remove all the resources that you created, you can delete them one-by-one, or delete the resource group.</span></span>

1. <span data-ttu-id="3b6fa-109">Azure portal にサインインします。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-109">Sign in to the Azure portal.</span></span>

1. <span data-ttu-id="3b6fa-110">左側のメニューから、**[すべてのサービス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-110">On the left menu, select **All Services**.</span></span>

1. <span data-ttu-id="3b6fa-111">**[リソース グループ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-111">Select **Resource Groups**.</span></span>

1. <span data-ttu-id="3b6fa-112">最初の演習で作成したリソース グループを探します。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-112">Find the resource group that you created in the first exercise.</span></span> <span data-ttu-id="3b6fa-113">リスト ビューの右側にある省略記号 (...) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-113">Click the ellipsis (...) on the right side of the list view.</span></span>

1. <span data-ttu-id="3b6fa-114">**[リソース グループの削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-114">Select **Delete resource group**.</span></span>

1. <span data-ttu-id="3b6fa-115">次の画面で、リソース グループ名を入力し、削除を確定します。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-115">On the next screen, enter the resource group name to confirm the deletion.</span></span>

1. <span data-ttu-id="3b6fa-116">**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3b6fa-116">Click **Delete**.</span></span>
