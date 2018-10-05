<span data-ttu-id="208bc-101">会社ではコンテナー イメージを使用して、コンピューティング ワークロードを管理しています。</span><span class="sxs-lookup"><span data-stu-id="208bc-101">Suppose your company makes use of container images to manage compute workloads.</span></span> <span data-ttu-id="208bc-102">ローカルの Docker ツールを使用して、ご自分のコンテナー イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="208bc-102">You use the local Docker tooling to build your container images.</span></span>

<span data-ttu-id="208bc-103">Azure Container Registry Tasks を使用することで、これらのコンテナーをビルドできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="208bc-103">You can now use Azure Container Registry Tasks to build these containers.</span></span> <span data-ttu-id="208bc-104">また、Container Registry Tasks では、ソース コードのコミット時に DevOps プロセスと自動化されたビルドを統合できます。</span><span class="sxs-lookup"><span data-stu-id="208bc-104">Container Registry Tasks also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="208bc-105">Azure Container Registry Tasks を使用してコンテナー イメージの作成を自動化してみましょう。</span><span class="sxs-lookup"><span data-stu-id="208bc-105">Let's automate the creation of a container image using Azure Container Registry Tasks.</span></span>

## <a name="create-a-container-image-with-azure-container-registry-tasks"></a><span data-ttu-id="208bc-106">Azure Container Registry Tasks を使用してコンテナー イメージを作成する</span><span class="sxs-lookup"><span data-stu-id="208bc-106">Create a container image with Azure Container Registry Tasks</span></span>

<span data-ttu-id="208bc-107">標準の Dockerfile にはビルド手順が提供されています。</span><span class="sxs-lookup"><span data-stu-id="208bc-107">A standard Dockerfile to provides build instructions.</span></span> <span data-ttu-id="208bc-108">Azure Container Registry Tasks では、ご利用の環境に現在ある任意の Dockerfile を再利用することができます。</span><span class="sxs-lookup"><span data-stu-id="208bc-108">Azure Container Registry Tasks allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.</span></span>

<span data-ttu-id="208bc-109">この例では、新しい Dockerfile を使用します。</span><span class="sxs-lookup"><span data-stu-id="208bc-109">We'll use a new Dockerfile for our example.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="208bc-110">最初の手順として、`Dockerfile` という名前の新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="208bc-110">The first step is to create a new file named `Dockerfile`.</span></span> <span data-ttu-id="208bc-111">任意のテキスト エディターを使用して、ファイルを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="208bc-111">You can use any text editor to edit the file.</span></span> <span data-ttu-id="208bc-112">この例では、Cloud Shell エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="208bc-112">We'll use Cloud Shell Editor for this example.</span></span> <span data-ttu-id="208bc-113">次のコマンドを Cloud Shell ウィンドウに貼り付けてエディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="208bc-113">Enter the following command into the Cloud Shell window to open the editor.</span></span>

```bash
code
```

<span data-ttu-id="208bc-114">次のコンテンツを新しい Dockerfile にコピーします。</span><span class="sxs-lookup"><span data-stu-id="208bc-114">Copy the following contents to your new Dockerfile.</span></span> <span data-ttu-id="208bc-115">必ずファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="208bc-115">Make sure to save the file.</span></span>

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="208bc-116"><kbd>Ctrl+S</kbd> キーの組み合わせを使用して保存します。</span><span class="sxs-lookup"><span data-stu-id="208bc-116">Use the key combination <kbd>Ctrl+S</kbd> to save.</span></span> <span data-ttu-id="208bc-117">メッセージが表示されたら、ファイルに `Dockerfile` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="208bc-117">Name the file `Dockerfile` when prompted.</span></span>

<span data-ttu-id="208bc-118">この構成では、Node.js アプリケーションが `node:9-alpine` イメージに追加されます。</span><span class="sxs-lookup"><span data-stu-id="208bc-118">This configuration adds a Node.js application to the `node:9-alpine` image.</span></span> <span data-ttu-id="208bc-119">次に、ポート 80 上で *EXPOSE* 命令を介してアプリケーションを提供できるようにコンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="208bc-119">After that, it configures the container to serve the application on port 80 via the *EXPOSE* instruction.</span></span>

<span data-ttu-id="208bc-120">次に、Azure CLI コマンド `az acr build` を実行して、Dockerfile からコンテナー イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="208bc-120">Now run the Azure CLI command `az acr build` to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrtasks:v1 .
```

<span data-ttu-id="208bc-121">このコマンドを実行すると、ビルドされてご利用の Container Registry にプッシュされるイメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="208bc-121">You'll see the image being built and pushed to your Container Registry as you run the command.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="208bc-122">イメージを確認する</span><span class="sxs-lookup"><span data-stu-id="208bc-122">Verify the image</span></span>

<span data-ttu-id="208bc-123">次のコマンドを実行して、イメージが作成されてレジストリに格納されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="208bc-123">Run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="208bc-124">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="208bc-124">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrtasks
```

<span data-ttu-id="208bc-125">これで、`helloacrtasks` イメージが使用できる状態になりました。</span><span class="sxs-lookup"><span data-stu-id="208bc-125">The `helloacrtasks` image is now ready to be used.</span></span>
