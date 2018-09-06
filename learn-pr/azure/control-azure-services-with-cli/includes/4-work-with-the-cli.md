<span data-ttu-id="2f495-101">Azure CLI では、コマンド ラインからコマンドを入力してすぐに実行できます。</span><span class="sxs-lookup"><span data-stu-id="2f495-101">The Azure CLI lets you type commands and execute them immediately from the command line.</span></span> <span data-ttu-id="2f495-102">ソフトウェア開発サンプルの全体的目標は、テストのために Web アプリの新しいビルドを展開することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="2f495-102">Recall that the overall goal in the software development example is to deploy new builds of a web app for testing.</span></span> <span data-ttu-id="2f495-103">Azure CLI を使用して実行できるタスクの種類について説明します。</span><span class="sxs-lookup"><span data-stu-id="2f495-103">Let's talk about the sorts of tasks you can do with the Azure CLI.</span></span>

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a><span data-ttu-id="2f495-104">Azure CLI を利用してどのような Azure リソースを管理できますか?</span><span class="sxs-lookup"><span data-stu-id="2f495-104">What Azure resources can be managed using the Azure CLI?</span></span>
<span data-ttu-id="2f495-105">Azure CLI を利用すると、あらゆる Azure リソースのほとんどすべての側面を制御できます。</span><span class="sxs-lookup"><span data-stu-id="2f495-105">The Azure CLI lets you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="2f495-106">リソース グループ、ストレージ、仮想マシン、Azure Active Directory (Azure AD)、コンテナー、機械学習などを操作することができます。</span><span class="sxs-lookup"><span data-stu-id="2f495-106">You can work with resource groups, storage, virtual machines, Azure Active Directory (Azure AD), containers, machine learning, and so on.</span></span>

<span data-ttu-id="2f495-107">CLI のコマンドは "_グループ_" と "_サブグループ_" で構成されています。</span><span class="sxs-lookup"><span data-stu-id="2f495-107">Commands in the CLI are structured in _groups_ and _subgroups_.</span></span> <span data-ttu-id="2f495-108">各グループが、Azure によって提供されるサービスを表しており、サブグループによって、そのサービスのコマンドが論理グループに分かれています。</span><span class="sxs-lookup"><span data-stu-id="2f495-108">Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.</span></span> <span data-ttu-id="2f495-109">たとえば、`storage` グループには、**account**、**blob**、**storage**、**queue** などのサブグループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2f495-109">For example, the `storage` group contains subgroups including **account**, **blob**, **storage**, and **queue**.</span></span>

<span data-ttu-id="2f495-110">それでは、必要な特定のコマンドはどのような方法で見つけますか?</span><span class="sxs-lookup"><span data-stu-id="2f495-110">So, how do you find the particular commands you need?</span></span> <span data-ttu-id="2f495-111">方法の 1 つに、`az find` を使用するというものがあります。</span><span class="sxs-lookup"><span data-stu-id="2f495-111">One way is to use `az find`.</span></span> <span data-ttu-id="2f495-112">たとえば、ストレージ BLOB の管理に役立つかもしれないコマンドを見つける場合、次の find コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2f495-112">For example, if you want to find commands that might help you manage a storage blob, you can use the following find command:</span></span>

```bash
az find -q blob
```

<span data-ttu-id="2f495-113">目的のコマンドの名前が既にわかっている場合、そのコマンドに対して `--help` 引数を指定すると、コマンド、コマンド グループ、使用可能なサブコマンドの一覧の詳細な情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2f495-113">If you already know the name of the command you want, the `--help` argument for that command will get you more detailed information on the command, and for a command group, a list of the available subcommands.</span></span> <span data-ttu-id="2f495-114">そこで、今回のストレージの例では、BLOB ストレージを管理するためのサブグループとコマンドの一覧を次のように取得できます。</span><span class="sxs-lookup"><span data-stu-id="2f495-114">So, with our storage example, here's how you can get a list of the subgroups and commands for managing blob storage:</span></span>

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a><span data-ttu-id="2f495-115">Azure リソースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="2f495-115">How to create an Azure resource</span></span>
<span data-ttu-id="2f495-116">新しい Azure リソースを作成するとき、通常、3 つの段階があります。Azure サブスクリプションに接続し、リソースを作成し、その作成が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2f495-116">When creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful.</span></span> <span data-ttu-id="2f495-117">次の図では、プロセスの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="2f495-117">The following illustration shows a high-level overview of the process.</span></span>

![コマンド ライン インターフェイスを使用して Azure リソースを作成する手順を示す図。](../media-drafts/4-create-resources-overview.png)

<span data-ttu-id="2f495-119">各ステップは、異なる Azure CLI コマンドに対応します。</span><span class="sxs-lookup"><span data-stu-id="2f495-119">Each step corresponds to a different Azure CLI command.</span></span>

### <a name="connect"></a><span data-ttu-id="2f495-120">接続</span><span class="sxs-lookup"><span data-stu-id="2f495-120">Connect</span></span>
<span data-ttu-id="2f495-121">Azure CLI のローカル インストールを使用している場合、Azure コマンドを実行する前に Azure CLI **login** コマンドを使用して認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2f495-121">Since you're working with a local install of the Azure CLI, you'll need to authenticate before you can execute Azure commands, by using the Azure CLI **login** command.</span></span> 

```bash
az login
```

<span data-ttu-id="2f495-122">Azure CLI は一般的に既定のブラウザーを起動して Azure のサインイン ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="2f495-122">The Azure CLI will typically launch your default browser to open the Azure sign-in page.</span></span> <span data-ttu-id="2f495-123">これでうまくいかない場合、コマンドラインの指示に従い、[https://aka.ms/devicelogin](https://aka.ms/devicelogin) で承認コードを入力します。</span><span class="sxs-lookup"><span data-stu-id="2f495-123">If this doesn't work, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

<span data-ttu-id="2f495-124">サインインに成功すると、Azure サブスクリプションに接続されます。</span><span class="sxs-lookup"><span data-stu-id="2f495-124">After a successful sign in, you'll be connected to your Azure subscription.</span></span> 

### <a name="create"></a><span data-ttu-id="2f495-125">Create</span><span class="sxs-lookup"><span data-stu-id="2f495-125">Create</span></span>
<span data-ttu-id="2f495-126">新しい Azure サービスを作成する前に新しいリソース グループを作成しなければならないことが頻繁にあります。そこで、例としてリソース グループを使用し、CLI から Azure リソースを作成する方法について示します。</span><span class="sxs-lookup"><span data-stu-id="2f495-126">You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.</span></span>

<span data-ttu-id="2f495-127">Azure CLI **group create** コマンドによってリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2f495-127">The Azure CLI **group create** command creates a resource group.</span></span> <span data-ttu-id="2f495-128">名前と場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2f495-128">You must specify a name and location.</span></span> <span data-ttu-id="2f495-129">この名前はサブスクリプション内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="2f495-129">The name must be unique within your subscription.</span></span> <span data-ttu-id="2f495-130">この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます。</span><span class="sxs-lookup"><span data-stu-id="2f495-130">The location determines where the metadata for your resource group will be stored.</span></span> <span data-ttu-id="2f495-131">"米国西部"、"北ヨーロッパ"、"インド西部" などの文字列を使用して場所を指定するか、westus、northeurope、westindia など、同じものを意味する 1 つの単語を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2f495-131">You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia.</span></span> <span data-ttu-id="2f495-132">中心的な構文は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2f495-132">The core syntax is:</span></span>

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a><span data-ttu-id="2f495-133">確認</span><span class="sxs-lookup"><span data-stu-id="2f495-133">Verify</span></span>
<span data-ttu-id="2f495-134">多くの Azure リソースの場合、Azure CLI によってリソース詳細を表示するための **list** サブコマンドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="2f495-134">For many Azure resources, the Azure CLI provides a **list** subcommand to view resource details.</span></span> <span data-ttu-id="2f495-135">たとえば、Azure CLI **group list** コマンドによって Azure リソース グループが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="2f495-135">For example, the Azure CLI **group list** command lists your Azure resource groups.</span></span> <span data-ttu-id="2f495-136">リソース グループが正常に作成されたことを確認するためにここで役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="2f495-136">This is useful here to verify whether creation of the resource group was successful:</span></span>

```bash
az group list
```

<span data-ttu-id="2f495-137">さらに簡潔なビューを得るために、出力をシンプルな表としてフォーマットできます。</span><span class="sxs-lookup"><span data-stu-id="2f495-137">To get a more concise view, you can format the output as a simple table:</span></span>

```bash
az group list --output table
```
