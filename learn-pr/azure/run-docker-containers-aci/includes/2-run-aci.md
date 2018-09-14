<span data-ttu-id="4f16b-101">Azure Container Instances を使用すると、仮想マシンをプロビジョニングしたり、より高度なレベルのサービスを採用したりしなくても、Azure の Docker コンテナーを簡単に作成、管理できます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-101">Azure Container Instances makes it easy to create and manage Docker containers in Azure, without having to provision virtual machines or adopt a higher-level service.</span></span> <span data-ttu-id="4f16b-102">このユニットでは、Azure でコンテナーを作成し、完全修飾ドメイン名 (FQDN) を使用してインターネットに公開します。</span><span class="sxs-lookup"><span data-stu-id="4f16b-102">In this unit, you create a container in Azure and expose it to the internet with a fully qualified domain name (FQDN).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="4f16b-103">リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="4f16b-103">Create a resource group</span></span>

<span data-ttu-id="4f16b-104">Azure Container Instances は、すべての Azure リソースと同様に、リソース グループに配置する必要があります。リソース グループとは、Azure リソースのデプロイと管理に使用する論理コレクションです。</span><span class="sxs-lookup"><span data-stu-id="4f16b-104">Azure Container Instances, like all Azure resources, must be placed in a resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="4f16b-105">`az group create` コマンドでリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f16b-105">Create a resource group with the `az group create` command.</span></span>

<span data-ttu-id="4f16b-106">次の例では、*myResourceGroup* という名前のリソース グループを *eastus* という場所に作成します。</span><span class="sxs-lookup"><span data-stu-id="4f16b-106">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="4f16b-107">コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="4f16b-107">Create a container</span></span>

<span data-ttu-id="4f16b-108">名前、Docker イメージ、および Azure リソース グループを **az container create** コマンドに指定することで、コンテナーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-108">You can create a container by providing a name, a Docker image, and an Azure resource group to the **az container create** command.</span></span> <span data-ttu-id="4f16b-109">必要に応じて、DNS 名ラベルを指定してコンテナーをインターネットに公開できます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-109">You can optionally expose the container to the internet by specifying a DNS name label.</span></span> <span data-ttu-id="4f16b-110">この例では、小型の Web アプリをホストするコンテナーをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4f16b-110">In this example, you deploy a container that hosts a small web app.</span></span>

<span data-ttu-id="4f16b-111">次のコマンドを実行して、コンテナー インスタンスを開始します。</span><span class="sxs-lookup"><span data-stu-id="4f16b-111">Execute the following command to start a container instance.</span></span> <span data-ttu-id="4f16b-112">*--dns-name-label* の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。そのため、場合によっては一意性を確保するためにこの値を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4f16b-112">The *--dns-name-label* value must be unique within the Azure region you create the instance, so you might need to modify this value to ensure uniqueness:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

<span data-ttu-id="4f16b-113">数秒のうちに、要求に対する応答が得られます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-113">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="4f16b-114">最初に、コンテナーは**作成中**の状態ですが、数秒のうちに起動されます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-114">Initially, the container is in the **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="4f16b-115">`az container show` コマンドを使用して状態を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-115">You can check the status using the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

<span data-ttu-id="4f16b-116">コマンドを実行すると、コンテナーの完全修飾ドメイン名 (FQDN) とそのプロビジョニング状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4f16b-116">When you run the command, the container's fully qualified domain name (FQDN) and its provisioning state are displayed:</span></span>

```bash
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

<span data-ttu-id="4f16b-117">コンテナーが**成功**状態に推移したら、ブラウザーでその FQDN に移動します。</span><span class="sxs-lookup"><span data-stu-id="4f16b-117">Once the container moves to the **Succeeded** state, navigate to its FQDN in your browser:</span></span>

![Azure コンテナー インスタンスで実行されているアプリケーションを示すブラウザー スクリーンショット](../media-draft/aci-app-browser.png)

## <a name="summary"></a><span data-ttu-id="4f16b-119">まとめ</span><span class="sxs-lookup"><span data-stu-id="4f16b-119">Summary</span></span>

<span data-ttu-id="4f16b-120">このユニットでは、Web サーバーとアプリケーションを実行する Azure コンテナー インスタンスを作成しました。</span><span class="sxs-lookup"><span data-stu-id="4f16b-120">In this unit, you created an Azure container instance that is running a web server and application.</span></span> <span data-ttu-id="4f16b-121">また、このアプリケーションには、コンテナー インスタンスの FQDN を使用してアクセスしました。</span><span class="sxs-lookup"><span data-stu-id="4f16b-121">You also accessed this application using the FQDN of the container instance.</span></span>

<span data-ttu-id="4f16b-122">次のユニットでは、コンテナー インスタンスの再起動のポリシーを構成します。</span><span class="sxs-lookup"><span data-stu-id="4f16b-122">In the next unit, you will configure container instance restart policies.</span></span>