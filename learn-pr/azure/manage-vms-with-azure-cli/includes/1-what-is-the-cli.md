<span data-ttu-id="1dad7-101">Jim は、会社の Web インフラストラクチャで実行されている Azure 仮想マシンのセットを管理しています。このインフラストラクチャには、複数の Web サイトとさまざまなプラットフォームで実行されているデータベース サーバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1dad7-101">Jim manages a set of Azure virtual machines running on our corporate web infrastructure, which includes several websites and database servers running on various platforms.</span></span> 

<span data-ttu-id="1dad7-102">Azure portal は使いやすいですが、さまざまなブレードを移動することにより、一部のタスクに余分な時間がかかっていることに Jim は気付いています。</span><span class="sxs-lookup"><span data-stu-id="1dad7-102">While the Azure portal is easy to use, Jim has found that navigating through the various blades adds time to some of the tasks.</span></span> 

<span data-ttu-id="1dad7-103">代替手段を検討しているときに、彼は Azure CLI ツールを見つけました。</span><span class="sxs-lookup"><span data-stu-id="1dad7-103">While exploring alternatives, he discovers the Azure CLI tool.</span></span>

<span data-ttu-id="1dad7-104">Azure CLI を使用することで、Jim はスクリプトを記述して、サーバーの状態の確認、新しい構成の展開、ポートのオープン、または仮想マシンに接続して設定を変更できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1dad7-104">Working with the Azure CLI, Jim can write scripts to check the status of his servers, deploy a new configuration, open a port, or connect to a virtual machine to change a setting.</span></span>

<span data-ttu-id="1dad7-105">もしかしたら、あなたも Jim のようにクラウド環境内のタスクを自動化するためのツールを探しているのではないでしょうか。</span><span class="sxs-lookup"><span data-stu-id="1dad7-105">Perhaps you are like Jim and are looking for a tool to help automate tasks in your cloud environment.</span></span> <span data-ttu-id="1dad7-106">Azure CLI を使用して、Azure でホストされている仮想マシンを作成および管理する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="1dad7-106">We're going to show you how to use the Azure CLI to create and manage virtual machines hosted in Azure.</span></span> 

## <a name="azure-cli"></a><span data-ttu-id="1dad7-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1dad7-107">Azure CLI</span></span>

<span data-ttu-id="1dad7-108">Azure CLI は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="1dad7-108">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="1dad7-109">このツールは、macOS、Linux、および Windows で使用できます。またはブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使用して使用できます。</span><span class="sxs-lookup"><span data-stu-id="1dad7-109">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1dad7-110">現在使用できる Azure CLI ツールには、Azure CLI 1.0 と Azure CLI 2.0 の 2 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="1dad7-110">There are two versions of the Azure CLI tool available today: Azure CLI 1.0 and Azure CLI 2.0.</span></span> <span data-ttu-id="1dad7-111">ここでは最新バージョンである Azure CLI 2.0 を使用します。レガシ スクリプトを実行する場合を除いて、こちらを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1dad7-111">We'll be using Azure CLI 2.0, which is the latest version and is recommended unless you're running legacy scripts.</span></span> <span data-ttu-id="1dad7-112">Azure CLI 1.0 は `azure` コマンドで始まり、Azure CLI 2.0 は `az` コマンドで始まります。</span><span class="sxs-lookup"><span data-stu-id="1dad7-112">Azure CLI 1.0 is started with the `azure` command, and Azure CLI 2.0 is started with the `az` command.</span></span> 

<span data-ttu-id="1dad7-113">Azure CLI では、コマンドラインからまたはスクリプトで仮想マシンやディスクなどの Azure リソースを管理するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1dad7-113">The Azure CLI can help you manage Azure resources such as virtual machines and disks from the command line or in scripts.</span></span> <span data-ttu-id="1dad7-114">では、どのようなことができるかを見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="1dad7-114">Let's get started and see what it can do.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="1dad7-115">学習の目的</span><span class="sxs-lookup"><span data-stu-id="1dad7-115">Learning objectives</span></span>
<span data-ttu-id="1dad7-116">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="1dad7-116">In this module, you will:</span></span>

- <span data-ttu-id="1dad7-117">CLI を使用して Azure 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="1dad7-117">Create an Azure Virtual Machine with the CLI.</span></span>
- <span data-ttu-id="1dad7-118">CLI を使用して VM のサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="1dad7-118">Resize VMs with the CLI.</span></span>
- <span data-ttu-id="1dad7-119">VM の管理とクエリをコマンドラインから行います。</span><span class="sxs-lookup"><span data-stu-id="1dad7-119">Manage and query a VM from the command line.</span></span>
- <span data-ttu-id="1dad7-120">VM にソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1dad7-120">Install software into a VM.</span></span>
