<span data-ttu-id="6f8d5-101">ここでは、Azure でコンテナーを作成して、完全修飾ドメイン名 (FQDN) を使用してインターネットに公開します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-101">Here, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

## <a name="why-use-azure-container-instances"></a><span data-ttu-id="6f8d5-102">Azure Container Instances を使用する理由</span><span class="sxs-lookup"><span data-stu-id="6f8d5-102">Why use Azure Container Instances?</span></span>

<span data-ttu-id="6f8d5-103">Azure Container Instances は、単純なアプリケーション、タスクの自動化などの分離されたコンテナーで処理でき、ジョブを作成する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-103">Azure Container Instances is useful for scenarios that can operate in isolated containers, including simple applications, task automation, and build jobs.</span></span> <span data-ttu-id="6f8d5-104">Azure Container Instances には次の利点があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-104">Azure Container Instances offers the following benefits:</span></span>

- <span data-ttu-id="6f8d5-105">**高速スタートアップ**: 数秒でコンテナーを起動します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-105">**Fast startup**: Launch containers in seconds.</span></span>
- <span data-ttu-id="6f8d5-106">**単位の 2 つ目の課金**: コンテナーの実行中にのみ、コストを発生します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-106">**Per second billing**: Incur costs only while the container is running.</span></span>
- <span data-ttu-id="6f8d5-107">**ハイパーバイザー レベルのセキュリティ**: アプリケーションを VM であるかのように完全に分離します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-107">**Hypervisor-level security**: Isolate your application as completely as it would be in a VM.</span></span>
- <span data-ttu-id="6f8d5-108">**カスタム サイズ**: CPU コアとメモリの正確な値を指定します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-108">**Custom sizes**: Specify exact values for CPU cores and memory.</span></span>
- <span data-ttu-id="6f8d5-109">**永続的なストレージ**: Azure Files のマウントを取得し、状態を維持するためのコンテナーに直接共有します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-109">**Persistent storage**: Mount Azure Files shares directly to a container to retrieve and persist state.</span></span>
- <span data-ttu-id="6f8d5-110">**Linux および Windows**: 同じ API を使用して Windows と Linux の両方のコンテナーのスケジュールを設定します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-110">**Linux and Windows**: Schedule both Windows and Linux containers using the same API.</span></span>

<span data-ttu-id="6f8d5-111">複数のコンテナー間でのサービスの検出、自動スケーリング、調整されたアプリケーション アップグレードなど、完全なコンテナー オーケストレーションが必要なシナリオには、Azure Kubernetes Service (AKS) をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-111">For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).</span></span>

## <a name="create-a-container"></a><span data-ttu-id="6f8d5-112">コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="6f8d5-112">Create a container</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="6f8d5-113">名前、Docker イメージ、およびする Azure リソース グループを提供することでコンテナーを作成する、 **az コンテナー作成**コマンド。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-113">You create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="6f8d5-114">必要に応じて、DNS 名ラベルを指定してコンテナーをインターネットに公開できます。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-114">You can optionally expose the container to the internet by specifying a DNS name label.</span></span> <span data-ttu-id="6f8d5-115">この例では、小型の Web アプリをホストするコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-115">In this example, you deploy a container that hosts a small web app.</span></span>

<span data-ttu-id="6f8d5-116">コンテナー インスタンスを起動する Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-116">Execute the following command in the Cloud Shell to start a container instance.</span></span> <span data-ttu-id="6f8d5-117">*--dns-name-label* の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。そのため、場合によっては一意性を確保するためにこの値を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-117">The *--dns-name-label* value must be unique within the Azure region you create the instance, so you might need to modify this value to ensure uniqueness:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

<span data-ttu-id="6f8d5-118">数秒のうちに、要求に対する応答が得られます。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-118">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="6f8d5-119">最初に、コンテナーは**作成中**の状態ですが、数秒のうちに起動されます。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-119">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="6f8d5-120">`az container show` コマンドを使用して状態を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-120">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="6f8d5-121">コマンドを実行すると、コンテナーの完全修飾ドメイン名 (FQDN) とそのプロビジョニング状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-121">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="6f8d5-122">コンテナーに移動すると、 **Succeeded**状態、成功を確認する、ブラウザーでその FQDN に移動します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-122">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser to verify success.</span></span>

<span data-ttu-id="6f8d5-123">ここでは、web サーバーとアプリケーションを実行する Azure コンテナー インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-123">Here, you created an Azure container instance to run a web server and application.</span></span> <span data-ttu-id="6f8d5-124">また、このアプリケーションには、コンテナー インスタンスの FQDN を使用してアクセスしました。</span><span class="sxs-lookup"><span data-stu-id="6f8d5-124">You also accessed this application using the FQDN of the container instance.</span></span>