## <a name="motivation"></a><span data-ttu-id="cec52-101">目的</span><span class="sxs-lookup"><span data-stu-id="cec52-101">Motivation</span></span>
<span data-ttu-id="cec52-102">Azure CLI では、コマンドを記述し、すぐに実行できます。</span><span class="sxs-lookup"><span data-stu-id="cec52-102">The Azure CLI lets you write commands and execute them immediately.</span></span> <span data-ttu-id="cec52-103">ソフトウェア開発サンプルの全体的目標は、テストのために Web アプリの新しいビルドを展開することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="cec52-103">Recall that the overall goal in the software development example is to deploy new builds of a web app for testing.</span></span> <span data-ttu-id="cec52-104">最初の手順として、リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="cec52-104">The first step is to create a resource group.</span></span> <span data-ttu-id="cec52-105">ここでの目標は Azure CLI のローカル インストールを利用してこれらのリソースを作成することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="cec52-105">Remember that the goal here is to create these resources using a local installation of the Azure CLI.</span></span> 

<span data-ttu-id="cec52-106">この演習では、Azure CLI を使用して Azure サブスクリプションにサインインし、新しいリソースを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cec52-106">This unit shows you how to use the Azure CLI to sign in to your Azure subscription and create a new resource.</span></span>

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a><span data-ttu-id="cec52-107">Azure CLI を利用してどのような Azure リソースを管理できますか?</span><span class="sxs-lookup"><span data-stu-id="cec52-107">What Azure resources can be managed using the Azure CLI?</span></span>
<span data-ttu-id="cec52-108">Azure CLI を利用すると、あらゆる Azure リソースのほとんどすべての側面を制御できます。</span><span class="sxs-lookup"><span data-stu-id="cec52-108">The Azure CLI lets you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="cec52-109">リソース グループ、ストレージ、仮想マシン、Azure Active Directory (Azure AD)、コンテナー、機械学習などを操作することができます。</span><span class="sxs-lookup"><span data-stu-id="cec52-109">You can work with resource groups, storage, virtual machines, Azure Active Directory (Azure AD), containers, machine learning, and so on.</span></span>

<span data-ttu-id="cec52-110">CLI のコマンドはグループとサブグループで構成されています。</span><span class="sxs-lookup"><span data-stu-id="cec52-110">Commands in the CLI are structured in groups and subgroups.</span></span> <span data-ttu-id="cec52-111">各グループが、Azure によって提供されるサービスを表しており、サブグループによって、そのサービスのコマンドが論理グループに分かれています。</span><span class="sxs-lookup"><span data-stu-id="cec52-111">Each group represents a service provided by Azure, and the subgroups divide commands for these services into logical groupings.</span></span> <span data-ttu-id="cec52-112">たとえば、**ストレージ** グループには、**アカウント**、**BLOB**、**ストレージ**、**キュー**などのサブグループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cec52-112">For example, the **storage** group contains subgroups including **account**, **blob**, **storage**, and **queue**.</span></span>

<span data-ttu-id="cec52-113">それでは、必要な特定のコマンドはどのような方法で見つけますか?</span><span class="sxs-lookup"><span data-stu-id="cec52-113">So, how do you find the particular commands you need?</span></span> <span data-ttu-id="cec52-114">方法の 1 つに、**az find** を使用するというものがあります。</span><span class="sxs-lookup"><span data-stu-id="cec52-114">One way is to use **az find**.</span></span> <span data-ttu-id="cec52-115">たとえば、ストレージ **blob** の管理に役立つかもしれないコマンドを見つける場合、次の find コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="cec52-115">For example, if you want to find commands that might help you manage a storage **blob**, you'd use the following find command:</span></span>

```bash
az find -q blob
```

<span data-ttu-id="cec52-116">必要なコマンドの名前が既にわかっている場合、そのコマンドの **--help** 引数がさらに役に立つことがあります。</span><span class="sxs-lookup"><span data-stu-id="cec52-116">If you already know the name of the command you want, the **--help** argument for that command may be more useful.</span></span> <span data-ttu-id="cec52-117">コマンドに関する詳細が得られます。コマンド グループの場合、利用可能なサブコマンドの一覧が得られます。</span><span class="sxs-lookup"><span data-stu-id="cec52-117">You get detailed information on the command, and for a command group, a list of the available subcommands.</span></span> <span data-ttu-id="cec52-118">そこで、今回のストレージの例では、blob ストレージを管理するためのサブグループとコマンドの一覧を次のように取得できます。</span><span class="sxs-lookup"><span data-stu-id="cec52-118">So, with our storage example, here's how you can get a list of the subgroups and commands for managing blob storage:</span></span>

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a><span data-ttu-id="cec52-119">Azure リソースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="cec52-119">How to create an Azure resource</span></span>
<span data-ttu-id="cec52-120">新しい Azure リソースを作成するとき、通常、3 つの段階があります。Azure サブスクリプションに接続し、リソースを作成し、その作成が成功したことを確認します (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="cec52-120">When creating a new Azure resource, there are typically three steps: connect to your Azure subscription, create the resource, and verify that creation was successful (see below).</span></span>

![Azure CLI を利用してリソースを作成する手順](../media-drafts/4-create-resources-overview.png)

<span data-ttu-id="cec52-122">各ステップは、異なる Azure CLI コマンドに対応します。</span><span class="sxs-lookup"><span data-stu-id="cec52-122">Each step corresponds to a different Azure CLI command.</span></span>

### <a name="connect"></a><span data-ttu-id="cec52-123">接続</span><span class="sxs-lookup"><span data-stu-id="cec52-123">Connect</span></span>
<span data-ttu-id="cec52-124">Azure CLI のローカル インストールを使用している場合、Azure コマンドを実行する前に Azure CLI **login** コマンドを使用して認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cec52-124">Since you're working with a local install of the Azure CLI, you'll need to authenticate before you can execute Azure commands, by using the Azure CLI **login** command.</span></span> 

```bash
az login
```

<span data-ttu-id="cec52-125">Azure CLI は一般的に既定のブラウザーを起動して Azure のサインイン ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="cec52-125">The Azure CLI will typically launch your default browser to open the Azure sign-in page.</span></span> <span data-ttu-id="cec52-126">これでうまくいかない場合、コマンドラインの指示に従い、[https://aka.ms/devicelogin](https://aka.ms/devicelogin) で承認コードを入力します。</span><span class="sxs-lookup"><span data-stu-id="cec52-126">If this doesn't work, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

<span data-ttu-id="cec52-127">サインインに成功すると、Azure サブスクリプションに接続されます。</span><span class="sxs-lookup"><span data-stu-id="cec52-127">After a successful sign in, you'll be connected to your Azure subscription.</span></span> 

### <a name="create"></a><span data-ttu-id="cec52-128">Create</span><span class="sxs-lookup"><span data-stu-id="cec52-128">Create</span></span>
<span data-ttu-id="cec52-129">新しい Azure サービスを作成する前に新しいリソース グループを作成しなければならないことが頻繁にあります。そこで、例としてリソース グループを使用し、CLI から Azure リソースを作成する方法について示します。</span><span class="sxs-lookup"><span data-stu-id="cec52-129">You'll often need to create a new resource group before you create a new Azure service, so we'll use resource groups as an example to show how to create Azure resources from the CLI.</span></span>

<span data-ttu-id="cec52-130">Azure CLI **group create** コマンドによってリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="cec52-130">The Azure CLI **group create** command creates a resource group.</span></span> <span data-ttu-id="cec52-131">名前と場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cec52-131">You must specify a name and location.</span></span> <span data-ttu-id="cec52-132">この名前はサブスクリプション内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="cec52-132">The name must be unique within your subscription.</span></span> <span data-ttu-id="cec52-133">この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます。</span><span class="sxs-lookup"><span data-stu-id="cec52-133">The location determines where the metadata for your resource group will be stored.</span></span> <span data-ttu-id="cec52-134">"米国西部"、"北ヨーロッパ"、"インド西部" などの文字列を使用して場所を指定するか、westus、northeurope、westindia など、同じものを意味する 1 つの単語を使用できます。</span><span class="sxs-lookup"><span data-stu-id="cec52-134">You use strings like "West US", "North Europe", or "West India" to specify the location; alternatively, you can use single word equivalents, such as westus, northeurope, or westindia.</span></span> <span data-ttu-id="cec52-135">中心的な構文は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="cec52-135">The core syntax is:</span></span>

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a><span data-ttu-id="cec52-136">確認</span><span class="sxs-lookup"><span data-stu-id="cec52-136">Verify</span></span>
<span data-ttu-id="cec52-137">多くの Azure リソースの場合、Azure CLI によってリソース詳細を表示するための **list** サブコマンドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="cec52-137">For many Azure resources, the Azure CLI provides a **list** subcommand to view resource details.</span></span> <span data-ttu-id="cec52-138">たとえば、Azure CLI **group list** コマンドによって Azure リソース グループが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="cec52-138">For example, the Azure CLI **group list** command lists your Azure resource groups.</span></span> <span data-ttu-id="cec52-139">リソース グループが正常に作成されたことを確認するためにここで役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="cec52-139">This is useful here to verify whether creation of the resource group was successful:</span></span>

```bash
az group list
```

<span data-ttu-id="cec52-140">さらに簡潔なビューを得るために、出力をシンプルな表としてフォーマットできます。</span><span class="sxs-lookup"><span data-stu-id="cec52-140">To get a more concise view, you can format the output as a simple table:</span></span>

```bash
az group list --output table
```
