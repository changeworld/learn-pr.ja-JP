<span data-ttu-id="868ac-101">会社がクラウド移行の一環としていくつかのサーバーをデプロイしているとします。</span><span class="sxs-lookup"><span data-stu-id="868ac-101">Suppose your company is deploying several servers as part of their cloud transition.</span></span> <span data-ttu-id="868ac-102">デプロイ時には VM ディスクを暗号化する必要があるため、ディスクが攻撃を受けやすくなることはありません。</span><span class="sxs-lookup"><span data-stu-id="868ac-102">VM disks must be encrypted during the deployment, so there's no time when the disks are vulnerable.</span></span> <span data-ttu-id="868ac-103">このプロセスを自動化して、暗号化を自動的に有効化するように Azure Resource Manager テンプレートを変更することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="868ac-103">You want to automate this process, and have to modify the Azure Resource Manager templates to automatically enable encryption.</span></span>

<span data-ttu-id="868ac-104">ここでは、Azure Resource Manager テンプレートを使用して、新しい Windows VM の暗号化を自動的に有効にする方法を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="868ac-104">Here, we'll look at how to use an Azure Resource Manager template to automatically enable encryption for new Windows VMs.</span></span>

## <a name="what-are-azure-resource-manager-templates"></a><span data-ttu-id="868ac-105">Azure Resource Manager テンプレートとは</span><span class="sxs-lookup"><span data-stu-id="868ac-105">What are Azure Resource Manager templates?</span></span>

<span data-ttu-id="868ac-106">Resource Manager テンプレートは、Azure にデプロイするリソース セットを定義するための JSON ファイルです。</span><span class="sxs-lookup"><span data-stu-id="868ac-106">Resource Manager templates are JSON files used to define a set of resources to deploy to Azure.</span></span> <span data-ttu-id="868ac-107">新規作成することも、VM など一部の Azure リソースについては、Azure portal を使用して生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="868ac-107">You can write them from scratch, and for some Azure resources, including VMs, you can use the Azure portal to generate them.</span></span> <span data-ttu-id="868ac-108">手動の VM デプロイでは必須情報を入力する必要がありますが、VM を Azure にデプロイする代わりに、テンプレートを保存します。</span><span class="sxs-lookup"><span data-stu-id="868ac-108">You'll need to complete the required information for a manual VM deployment, but instead of deploying the VM to Azure, you save the template.</span></span> <span data-ttu-id="868ac-109">テンプレートを_再利用_し、その特定の VM 構成を作成できます。</span><span class="sxs-lookup"><span data-stu-id="868ac-109">You can then _reuse_ the template to create that specific VM configuration.</span></span>

<span data-ttu-id="868ac-110">あらゆる種類の管理タスクを自動化するサンプル テンプレートを[こちら](https://azure.microsoft.com/resources/templates)のドキュメントで紹介しています。</span><span class="sxs-lookup"><span data-stu-id="868ac-110">There are [example templates available in docs](https://azure.microsoft.com/resources/templates) to automate all sorts of administrative tasks.</span></span> <span data-ttu-id="868ac-111">今回は手動で VM を暗号化しましたが、実はここにあるテンプレートを使用することもできました。</span><span class="sxs-lookup"><span data-stu-id="868ac-111">In fact, we could have used one of these templates to encrypt our VM that we just did manually!</span></span>

![Azure テンプレートのスクリーンショット。](../media/5-browse-templates.png)

## <a name="using-github-templates"></a><span data-ttu-id="868ac-113">GitHub テンプレートの使用</span><span class="sxs-lookup"><span data-stu-id="868ac-113">Using GitHub templates</span></span>

<span data-ttu-id="868ac-114">実際のテンプレート ソースは GitHub に保存されています。</span><span class="sxs-lookup"><span data-stu-id="868ac-114">The actual template source is stored in GitHub.</span></span> <span data-ttu-id="868ac-115">GitHub でテンプレートを参照し、そのページから Azure に直接デプロイできます。</span><span class="sxs-lookup"><span data-stu-id="868ac-115">You can browse to a template in GitHub and deploy right to Azure from the page.</span></span>

![GitHub テンプレートのスクリーンショット。[Azure にデプロイ] ボタンが強調表示されています。](../media/5-deploy-from-github.png)

<span data-ttu-id="868ac-117">テンプレートがデプロイされると、Azure では、必須入力フィールドの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="868ac-117">When the template is deployed, Azure will display a list of required input fields.</span></span>

![Azure portal のテンプレートのスクリーンショット。](../media/5-fill-in-template.png)

<span data-ttu-id="868ac-119">次に、テンプレートを実行してリソースを作成、修正、削除できます。</span><span class="sxs-lookup"><span data-stu-id="868ac-119">You can then execute the template to create, modify, or remove resources.</span></span>

### <a name="running-templates-in-the-azure-portal"></a><span data-ttu-id="868ac-120">Azure portal でテンプレートを実行する</span><span class="sxs-lookup"><span data-stu-id="868ac-120">Running templates in the Azure portal</span></span>

<span data-ttu-id="868ac-121">使用するテンプレートが既にわかっている場合、あるいは自分の Azure アカウントでテンプレートを保存している場合、**[リソースの作成]** の **[テンプレートのデプロイ]** リソースを使用し、ポータルで定義済みのテンプレートを検索し、実行できます。</span><span class="sxs-lookup"><span data-stu-id="868ac-121">If you already know the template you want to use, or you have saved templates in your Azure account, you can use the **Create a resource** > **Template Deployment** resource to locate and run defined templates in the portal.</span></span> <span data-ttu-id="868ac-122">名前でテンプレートを検索したり、テンプレートを編集してパラメーターや動作を変更したり、GUI から直接テンプレートを実行したりできます。</span><span class="sxs-lookup"><span data-stu-id="868ac-122">You can search through templates by name, edit a template to change the parameters or behavior, and execute the template right from the GUI.</span></span>

### <a name="running-templates-from-the-command-line"></a><span data-ttu-id="868ac-123">コマンドラインからテンプレートを実行する</span><span class="sxs-lookup"><span data-stu-id="868ac-123">Running templates from the command line</span></span>

<span data-ttu-id="868ac-124">テンプレートに URL を指定することで、Azure PowerShell でテンプレートを実行できます。</span><span class="sxs-lookup"><span data-stu-id="868ac-124">Given a URL to a template, you can execute it with Azure PowerShell.</span></span> <span data-ttu-id="868ac-125">たとえば、次の PowerShell コマンドでディスク暗号化テンプレートを実行できます。</span><span class="sxs-lookup"><span data-stu-id="868ac-125">For example, we could run the disk encryption template with the following PowerShell command:</span></span>

```powershell
New-AzureRmResourceGroupDeployment `
    -Name encrypt-disk `
    -ResourceGroupName <resource-group-name> `
    -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

<span data-ttu-id="868ac-126">あるいは、Azure CLI を使用する場合、`group deployment create` コマンドで実行できます。</span><span class="sxs-lookup"><span data-stu-id="868ac-126">Or, if you prefer the Azure CLI, with the `group deployment create` command.</span></span>

```azurecli
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> \ 
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

