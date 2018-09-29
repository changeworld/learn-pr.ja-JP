<span data-ttu-id="9e819-101">ここでは、Azure でコンテナーを作成し、完全修飾ドメイン名 (FQDN) を使用してインターネットに公開します。</span><span class="sxs-lookup"><span data-stu-id="9e819-101">Here, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="why-use-azure-container-instances"></a><span data-ttu-id="9e819-102">Azure Container Instances を使用する理由</span><span class="sxs-lookup"><span data-stu-id="9e819-102">Why use Azure Container Instances?</span></span>

<span data-ttu-id="9e819-103">Azure Container Instances は、シンプルなアプリケーション、タスク自動化、ビルド ジョブなど、分離されたコンテナーで操作できるシナリオに便利です。</span><span class="sxs-lookup"><span data-stu-id="9e819-103">Azure Container Instances is useful for scenarios that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="9e819-104">Azure Container Instances には次の利点があります。</span><span class="sxs-lookup"><span data-stu-id="9e819-104">Azure Container Instances offers the following benefits:</span></span>

- <span data-ttu-id="9e819-105">**高速起動**: 数秒でコンテナーを起動します。</span><span class="sxs-lookup"><span data-stu-id="9e819-105">**Fast startup**: Launch containers in seconds.</span></span>
- <span data-ttu-id="9e819-106">**秒単位の課金**: コンテナーが実行されている間だけコストが発生します。</span><span class="sxs-lookup"><span data-stu-id="9e819-106">**Per second billing**: Incur costs only while the container is running.</span></span>
- <span data-ttu-id="9e819-107">**ハイパーバイザー レベルのセキュリティ**: アプリケーションを VM 内にあるのと同様に完全に分離します。</span><span class="sxs-lookup"><span data-stu-id="9e819-107">**Hypervisor-level security**: Isolate your application as completely as it would be in a VM.</span></span>
- <span data-ttu-id="9e819-108">**カスタム サイズ**: CPU コアとメモリの正確な値を指定します。</span><span class="sxs-lookup"><span data-stu-id="9e819-108">**Custom sizes**: Specify exact values for CPU cores and memory.</span></span>
- <span data-ttu-id="9e819-109">**永続的なストレージ**: Azure Files 共有をコンテナーに直接マウントして、状態を取得して保持します。</span><span class="sxs-lookup"><span data-stu-id="9e819-109">**Persistent storage**: Mount Azure Files shares directly to a container to retrieve and persist state.</span></span>
- <span data-ttu-id="9e819-110">**Linux と Windows**: 同じ API を使用して、Windows と Linux の両方のコンテナーでスケジュールを設定します。</span><span class="sxs-lookup"><span data-stu-id="9e819-110">**Linux and Windows**: Schedule both Windows and Linux containers using the same API.</span></span>

<span data-ttu-id="9e819-111">複数のコンテナー間でのサービスの検出、自動スケーリング、調整されたアプリケーション アップグレードなど、完全なコンテナー オーケストレーションが必要なシナリオには、Azure Kubernetes Service (AKS) をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9e819-111">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="9e819-112">コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="9e819-112">Create a container</span></span>

<span data-ttu-id="9e819-113">名前、Docker イメージ、および Azure リソース グループを **az container create** コマンドに指定することで、コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="9e819-113">You create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="9e819-114">必要に応じて、DNS 名ラベルを指定してコンテナーをインターネットに公開できます。</span><span class="sxs-lookup"><span data-stu-id="9e819-114">You can optionally expose the container to the Internet by specifying a DNS name label.</span></span> <span data-ttu-id="9e819-115">この例では、小さな Web アプリをホストするコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="9e819-115">In this example, you deploy a container that hosts a small web app.</span></span> <span data-ttu-id="9e819-116">イメージを配置する場所を選択することもできます。既定では以下のように "米国東部" に設定されますが、次のリストに示されている自分に近い場所に変更することができます。</span><span class="sxs-lookup"><span data-stu-id="9e819-116">You can also select the location to place the image - we're defaulting to "East US" below, but you can change it to a location close to you from the following list.</span></span>

<span data-ttu-id="9e819-117"><!-- TODO: fix region list so it's not hardcoded here --> 無料のサンドボックスを使用すると、Azure のグローバル リージョンのサブセットにリソースを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9e819-117"><!-- TODO: fix region list so it's not hardcoded here --> The free sandbox allows you to create resources in a subset of Azure's global regions.</span></span> <span data-ttu-id="9e819-118">リソースを作成するときは、次のリストからリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="9e819-118">Select a region from the following list when creating any resources:</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="9e819-119">- westus2 - centralus - eastus - westeurope - southeastasia</span><span class="sxs-lookup"><span data-stu-id="9e819-119">- westus2 - centralus - eastus - westeurope - southeastasia</span></span> :::column-end:::
:::row-end:::

<span data-ttu-id="9e819-120">Cloud Shell で次のコマンドを実行して、コンテナー インスタンスを開始します。</span><span class="sxs-lookup"><span data-stu-id="9e819-120">Execute the following command in the Cloud Shell to start a container instance.</span></span> <span data-ttu-id="9e819-121">`--dns-name-label` の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。そのため、`[dns-name]` を一意の値に置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="9e819-121">The `--dns-name-label` value must be unique within the Azure region you create the instance, so you will need to replace `[dns-name]` with something unique.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label [dns-name] \
    --location eastus
```

<span data-ttu-id="9e819-122">数秒で、要求に対する応答が得られます。</span><span class="sxs-lookup"><span data-stu-id="9e819-122">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="9e819-123">最初に、コンテナーは**作成中**の状態ですが、数秒のうちに起動されます。</span><span class="sxs-lookup"><span data-stu-id="9e819-123">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="9e819-124">`az container show` コマンドを使用して状態を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="9e819-124">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="9e819-125">コマンドを実行すると、コンテナーの完全修飾ドメイン名 (FQDN) とそのプロビジョニング状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e819-125">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="9e819-126">コンテナーが**成功**状態に推移したら、ブラウザーでその FQDN に移動して、成功を確認します。</span><span class="sxs-lookup"><span data-stu-id="9e819-126">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser to verify success.</span></span>

<span data-ttu-id="9e819-127">ここでは、Web サーバーとアプリケーションを実行する Azure コンテナー インスタンスを作成しました。</span><span class="sxs-lookup"><span data-stu-id="9e819-127">Here, you created an Azure container instance to run a web server and application.</span></span> <span data-ttu-id="9e819-128">また、このアプリケーションには、コンテナー インスタンスの FQDN を使用してアクセスしました。</span><span class="sxs-lookup"><span data-stu-id="9e819-128">You also accessed this application using the FQDN of the container instance.</span></span>