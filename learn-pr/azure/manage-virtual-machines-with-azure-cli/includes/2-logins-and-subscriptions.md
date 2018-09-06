<span data-ttu-id="afb60-101">始める前に、Azure CLI ツールの構文を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="afb60-101">Before we start, let's review the syntax for the Azure CLI tool.</span></span> <span data-ttu-id="afb60-102">「**Control Azure services with the Azure CLI**」(Azure CLI を使用した Azure サービスの制御) モジュールをご覧になった場合は、Azure CLI コマンドが次のような形式であることはご存じのはずです。</span><span class="sxs-lookup"><span data-stu-id="afb60-102">If you've taken the **Control Azure services with the Azure CLI** module, then you know that Azure CLI commands take the form of:</span></span>

```azurecli
az [command] [subcommand] [--parameter --parameter]
```

<span data-ttu-id="afb60-103">`[command]` は、Azure の特定の制御対象領域を示します。</span><span class="sxs-lookup"><span data-stu-id="afb60-103">The `[command]` identifies the specific area of Azure you want to control.</span></span> <span data-ttu-id="afb60-104">たとえば、`account` コマンドを使うとサブスクリプション情報を管理でき、`sql` コマンドを使うと SQL データベースを管理できます。</span><span class="sxs-lookup"><span data-stu-id="afb60-104">For example, you can manage subscription information with the `account` command, or SQL databases with the `sql` command.</span></span> <span data-ttu-id="afb60-105">`[subcommand]` と `[--parameters]` は、使用しているコマンドに依存します。</span><span class="sxs-lookup"><span data-stu-id="afb60-105">The `[subcommand]` and `[--parameters]` are then dependent upon the command you're working with.</span></span> 

<span data-ttu-id="afb60-106">部分的なコマンドを入力することで、コマンド、サブコマンド、パラメーターの一覧を表示できます。</span><span class="sxs-lookup"><span data-stu-id="afb60-106">You can view a list of commands, subcommands, and parameters by typing in a partial command.</span></span> <span data-ttu-id="afb60-107">たとえば、コマンド ラインで「`az`」と入力すると最上位レベルのヘルプ画面が表示され、「`az vm`」と入力すると仮想マシンに対するすべてのサブコマンドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="afb60-107">For example, typing `az` at the command line will give you the top-level help screen, and typing `az vm` will give you all the subcommands for virtual machines.</span></span> <span data-ttu-id="afb60-108">この方法は、Azure CLI ツールを調べるのにとても便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="afb60-108">This approach can be a great way to explore the Azure CLI tool.</span></span>

> [!NOTE]
> <span data-ttu-id="afb60-109">ここでは、ブラウザーでホストされた Azure Cloud Shell から Azure CLI を使用します。</span><span class="sxs-lookup"><span data-stu-id="afb60-109">We will be using browser-hosted Azure Cloud Shell to work with the Azure CLI.</span></span> <span data-ttu-id="afb60-110">ローカル コンピューターを使用する方がよい場合は、[Azure CLI をインストールする](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ことにより、ここで説明するすべてのコマンドをコマンド ラインから実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="afb60-110">If you prefer to work from your local machine, all of the commands we cover can also be executed from the command line by [installing the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="afb60-111">Azure にログインする</span><span class="sxs-lookup"><span data-stu-id="afb60-111">Log in to Azure</span></span>

<span data-ttu-id="afb60-112">Azure CLI を使用するときは、最初に、お使いの Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="afb60-112">The first thing you'll do when working with the Azure CLI is to log in to your Azure account.</span></span> <span data-ttu-id="afb60-113">それには、`login` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="afb60-113">This is done with the `login` command.</span></span> <span data-ttu-id="afb60-114">Cloud Shell を使用している場合は、Azure にサインインするためのボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="afb60-114">If you're using Cloud Shell, there will be a button to sign in to Azure.</span></span>

```azurecli
az login
```

<span data-ttu-id="afb60-115">このコマンドにより、ブラウザー ウィンドウが起動されて、使用する Microsoft アカウントを選択できます。</span><span class="sxs-lookup"><span data-stu-id="afb60-115">This command will launch a browser window and allow you to select the Microsoft account you want to use.</span></span>

## <a name="working-with-subscriptions"></a><span data-ttu-id="afb60-116">サブスクリプションの使用</span><span class="sxs-lookup"><span data-stu-id="afb60-116">Working with subscriptions</span></span>

<span data-ttu-id="afb60-117">このモジュールではプレイグラウンドとして作成された一時サブスクリプションで作業しますが、通常は、自分のアカウントのサブスクリプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="afb60-117">In this module, we will work in a temporary subscription created as a playground, but you will normally use a subscription from your own account.</span></span> <span data-ttu-id="afb60-118">複数のサブスクリプションがある場合は、`az account list --output table` ステートメントを使用して、わかりやすく書式設定されたサブスクリプションの一覧を取得できます。</span><span class="sxs-lookup"><span data-stu-id="afb60-118">If you have more than one subscription, you can get a clearly formatted list of subscriptions using the `az account list --output table` statement.</span></span>

```
Name                                  CloudName    SubscriptionId                        State    IsDefault
------------------------------------  -----------  ------------------------------------  -------  -----------
Contoso Legacy Resources              AzureCloud   abc13b0c-d2c4-64b2-9ac5-2f4cb819b752  Enabled  True
Visual Studio Enterprise              AzureCloud   233aebce-23c2-4572-c056-c029449e93ed  Enabled  False
```

<span data-ttu-id="afb60-119">このコマンドでは、すべてのコマンドが適用される "_既定の_" サブスクリプションも示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="afb60-119">Notice that the command also identifies the _default_ subscription where all your commands will apply.</span></span> <span data-ttu-id="afb60-120">別のサブスクリプションで作業する場合は、`az account set --subscription "[name]"` コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="afb60-120">If you would prefer to work in a different subscription, you can use the `az account set --subscription "[name]"` command.</span></span> <span data-ttu-id="afb60-121">たとえば、現在のサブスクリプションを上記のリストの `Visual Studio Enterprise` に設定するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="afb60-121">For example, we could set our current subscription to be `Visual Studio Enterprise` from the above list through the following command:</span></span>

```azurecli
az account set --subscription "Visual Studio Enterprise"
```