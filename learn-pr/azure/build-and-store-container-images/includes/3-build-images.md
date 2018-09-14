<span data-ttu-id="93d8e-101">会社ではコンテナー イメージを使用して、社内のコンピューティング ワークロードを管理しています。</span><span class="sxs-lookup"><span data-stu-id="93d8e-101">Your company makes use of container images to manage compute workloads in the company.</span></span> <span data-ttu-id="93d8e-102">ローカルの Docker ツールを使用して、ご自分のコンテナー イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="93d8e-102">You use local Docker tooling to build your container images.</span></span> <span data-ttu-id="93d8e-103">Azure Container Registry の使用を決断することにより、クラウド内でコンテナー イメージをビルドできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="93d8e-103">The decision to make use of an Azure Container Registry now allows you to build container images in the cloud.</span></span> 

<span data-ttu-id="93d8e-104">Azure Container Registry Build を使用することで、これらのコンテナーをビルドできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="93d8e-104">You can now use Azure Container Registry Build to build these containers.</span></span> <span data-ttu-id="93d8e-105">また、Container Registry Build では、ソース コードのコミット時に DevOps プロセスと自動化されたビルドを統合できます。</span><span class="sxs-lookup"><span data-stu-id="93d8e-105">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="93d8e-106">Azure Container Registry Build を使用してコンテナー イメージの作成を自動化してみましょう。</span><span class="sxs-lookup"><span data-stu-id="93d8e-106">Let's automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-azure-container-registry-build"></a><span data-ttu-id="93d8e-107">Azure Container Registry Build を使用してコンテナー イメージを作成する</span><span class="sxs-lookup"><span data-stu-id="93d8e-107">Create a container image with Azure Container Registry Build</span></span>

<span data-ttu-id="93d8e-108">ビルド手順が提供されている標準の Dockerfile を使用します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-108">You use a standard Dockerfile to provide build instructions.</span></span> <span data-ttu-id="93d8e-109">Azure Container Registry Build では、ご利用の環境に現在ある任意の Dockerfile を再利用することができます。</span><span class="sxs-lookup"><span data-stu-id="93d8e-109">Azure Container Registry Build allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.</span></span>

<span data-ttu-id="93d8e-110">この例では、新しい Dockerfile を使用します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-110">We'll use new Dockerfile for our example.</span></span> 

<span data-ttu-id="93d8e-111">最初の手順として、`Dockerfile` という名前の新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-111">The first step is to create a new file named `Dockerfile`.</span></span> <span data-ttu-id="93d8e-112">任意のテキスト エディターを使用して、ファイルを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="93d8e-112">You can use any text editor to edit the file.</span></span> <span data-ttu-id="93d8e-113">この例では、Visual Studio Code を使用します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-113">We'll use Visual Studio Code for this example.</span></span>

```bash
code Dockerfile
```

<span data-ttu-id="93d8e-114">次のコンテンツを新しい Dockerfile にコピーします。</span><span class="sxs-lookup"><span data-stu-id="93d8e-114">Copy the following contents to your new Dockerfile.</span></span> <span data-ttu-id="93d8e-115">必ずファイルをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-115">Make sure to safe the file.</span></span> 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="93d8e-116">この構成では、Node.js アプリケーションが `node:9-alpine` イメージに追加されます。</span><span class="sxs-lookup"><span data-stu-id="93d8e-116">This configuration adds a Node.js application to the `node:9-alpine` image.</span></span> <span data-ttu-id="93d8e-117">次に、ポート 80 上で *EXPOSE* 命令を介してアプリケーションを提供できるようにコンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-117">Then configures the container to serve the application on port 80 via the *EXPOSE* instruction.</span></span>

<span data-ttu-id="93d8e-118">次に、Azure CLI コマンド `az acr build` を実行して、Dockerfile からコンテナー イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="93d8e-118">Now run the Azure CLI command, `az acr build`, to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="93d8e-119">このコマンドを実行すると、ビルドされてご利用のコンテナー レジストリにプッシュされるイメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="93d8e-119">You'll see the image being built and pushed to your Container Registry as you run the command.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="93d8e-120">イメージを確認する</span><span class="sxs-lookup"><span data-stu-id="93d8e-120">Verify the image</span></span>

<span data-ttu-id="93d8e-121">次のコマンドを実行して、イメージが作成されてレジストリに格納されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="93d8e-121">Run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="93d8e-122">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="93d8e-122">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="93d8e-123">これで、`helloacrbuild` イメージが使用できる状態になりました。</span><span class="sxs-lookup"><span data-stu-id="93d8e-123">The `helloacrbuild` image is now ready to be used.</span></span>
