<span data-ttu-id="cc9a0-101">このモジュールでは、Azure portal を使った Linux VM の作成方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-101">In this module, you learned how to create a Linux VM using the Azure portal.</span></span> <span data-ttu-id="cc9a0-102">その後、VM のパブリック IP アドレスに接続し、SSH 接続を使って VM の管理を行いました。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-102">You then connected to the public IP address of the VM and managed it with an SSH connection.</span></span> 

<span data-ttu-id="cc9a0-103">また、SSH では仮想マシン上のオペレーティング システムやソフトウェアと通信が可能になり、ポータルでは仮想ハードウェアや接続性の設定ができることを学びました。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-103">You learned that while SSH allows us to interact with the operating system and software of the virtual machine, the portal will enable us to configure the virtual hardware and connectivity.</span></span> <span data-ttu-id="cc9a0-104">さらに、コマンドラインもしくはスクリプト可能な環境が推奨される場合に、PowerShell または Azure CLI が使えることを確認しました。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-104">We also could have used PowerShell or the Azure CLI, if a command-line or scriptable environment were preferred.</span></span>

## <a name="clean-up-the-resources"></a><span data-ttu-id="cc9a0-105">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="cc9a0-105">Clean up the resources</span></span>

<span data-ttu-id="cc9a0-106">VM が実行している間、および使用されているストレージ容量に基づいた課金が行われます。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-106">You are charged for VMs while they run, and for the storage based on how much you use.</span></span> <span data-ttu-id="cc9a0-107">使用していないときは VM を停止した上で割当解除を行い、リソースが今後必要でない場合は、削除することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-107">Always stop and deallocate VMs when you aren't using them, and when you no longer need the resources, it's a good idea to delete them.</span></span> <span data-ttu-id="cc9a0-108">作成したすべてのリソースを削除するには、1 つずつの削除またはリソース グループ単位での削除が行えます。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-108">To remove all the resources that you created, you can delete them one-by-one, or delete the resource group.</span></span>

1. <span data-ttu-id="cc9a0-109">Azure portal にサインインします。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-109">Sign in to the Azure portal.</span></span>

1. <span data-ttu-id="cc9a0-110">左側のメニューから、**[すべてのサービス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-110">On the left menu, select **All Services**.</span></span>

1. <span data-ttu-id="cc9a0-111">**[リソース グループ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-111">Select **Resource Groups**.</span></span>

1. <span data-ttu-id="cc9a0-112">最初の演習で作成したリソース グループを探します。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-112">Find the resource group that you created in the first exercise.</span></span> <span data-ttu-id="cc9a0-113">リスト ビューの右側にある省略記号 (...) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-113">Click the ellipsis (...) on the right side of the list view.</span></span>

1. <span data-ttu-id="cc9a0-114">**[リソース グループの削除]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-114">Select **Delete resource group**.</span></span>

1. <span data-ttu-id="cc9a0-115">次の画面で、リソース グループ名を入力し、削除を確定します。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-115">On the next screen, enter the resource group name to confirm the deletion.</span></span>

1. <span data-ttu-id="cc9a0-116">**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc9a0-116">Click **Delete**.</span></span>
