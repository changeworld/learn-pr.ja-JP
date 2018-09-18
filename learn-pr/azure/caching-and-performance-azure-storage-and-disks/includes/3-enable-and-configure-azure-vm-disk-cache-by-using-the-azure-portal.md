<span data-ttu-id="2b94e-101">次のいずれかのツールを使用して、仮想マシンのディスク キャッシュ設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-101">You can configure virtual machine disk cache settings with any of the following tools:</span></span>

- <span data-ttu-id="2b94e-102">Azure portal</span><span class="sxs-lookup"><span data-stu-id="2b94e-102">Azure portal</span></span>
- <span data-ttu-id="2b94e-103">Resource Manager テンプレート</span><span class="sxs-lookup"><span data-stu-id="2b94e-103">Resource Manager templates</span></span>
- <span data-ttu-id="2b94e-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2b94e-104">Azure CLI</span></span>
- <span data-ttu-id="2b94e-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2b94e-105">Azure PowerShell</span></span>

<span data-ttu-id="2b94e-106">次の演習では、ポータルを使用して VM を作成し、そのディスク上にキャッシュを構成します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-106">In the next exercise, we're going to use the portal to create a VM and configure caching on its disks.</span></span> <span data-ttu-id="2b94e-107">注意が必要な情報がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="2b94e-107">Here's some information to keep in mind.</span></span> 

<span data-ttu-id="2b94e-108">Azure portal を使用して新しい VM をプロビジョニングする場合、VM がデプロイされるまで、OS ディスクの既定のキャッシュ構成を読み取り/書き込みから変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="2b94e-108">When you provision a new VM using the Azure portal, you can't change the default caching configuration for the OS disk from Read/write until the VM is deployed.</span></span>

<span data-ttu-id="2b94e-109">データ ディスクを既存の VM に追加する場合、ディスクを VM にデプロイする前にキャッシュ オプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-109">When you add a data disk to an existing VM, you can configure the cache option before the disk is deployed to the VM.</span></span>

<span data-ttu-id="2b94e-110">Azure ディスクのキャッシュ設定を変更すると、対象となるディスクをデタッチして再アタッチします。</span><span class="sxs-lookup"><span data-stu-id="2b94e-110">Changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="2b94e-111">オペレーティング システム ディスクの場合は、VM が再起動されます。</span><span class="sxs-lookup"><span data-stu-id="2b94e-111">If it's the operating system disk, the VM is restarted.</span></span> <span data-ttu-id="2b94e-112">ディスク キャッシュの設定を変更する前に、この中断の影響を受ける可能性があるすべてのアプリケーションまたはサービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="2b94e-112">Stop all applications/services that might be affected by this disruption before changing the disk cache setting.</span></span>

<span data-ttu-id="2b94e-113">Azure portal を使用して VM を作成し、キャッシュ設定を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="2b94e-113">Let's create a VM and change the cache settings using the Azure portal.</span></span>
