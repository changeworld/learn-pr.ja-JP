<span data-ttu-id="80f9d-101">Azure Container Registry Build では、コンテナー イメージをクラウドでビルドできます。ローカルの Docker ツールは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="80f9d-101">With Azure Container Registry Build, container images can be built in the cloud, without the need for local Docker tooling.</span></span> <span data-ttu-id="80f9d-102">また、Container Registry Build では、ソース コードのコミット時に DevOps プロセスと自動化されたビルドを統合できます。</span><span class="sxs-lookup"><span data-stu-id="80f9d-102">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="80f9d-103">このユニットでは、Azure Container Registry Build を使用してコンテナー イメージの作成を自動化します。</span><span class="sxs-lookup"><span data-stu-id="80f9d-103">In this unit, you automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-build"></a><span data-ttu-id="80f9d-104">Build を使用してコンテナー イメージを作成する</span><span class="sxs-lookup"><span data-stu-id="80f9d-104">Create a container image with Build</span></span>

<span data-ttu-id="80f9d-105">Azure Container Registry Build の使用時には、標準の Dockerfile を使用してビルド手順が提供されます。</span><span class="sxs-lookup"><span data-stu-id="80f9d-105">When using Azure Container Registry Build, a standard Dockerfile is used to provide build instructions.</span></span> <span data-ttu-id="80f9d-106">環境で現在使用されている Dockerfile (複数の段階で構成されるビルドを含む) は Azure Container Registry Build と連携します。</span><span class="sxs-lookup"><span data-stu-id="80f9d-106">Any Dockerfile currently used in your environment, including multistaged builds, works with Azure Container Registry Build.</span></span>

<span data-ttu-id="80f9d-107">`Dockerfile` という名前のファイルを作成し、任意のテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="80f9d-107">Create a file named `Dockerfile` and open it with any text editor.</span></span>

```bash
code Dockerfile
```

<span data-ttu-id="80f9d-108">次の内容をコピーしてファイルを保存し、テキスト エディターを終了します。</span><span class="sxs-lookup"><span data-stu-id="80f9d-108">Copy in the following contents, save the file, and exit the text editor.</span></span> <span data-ttu-id="80f9d-109">この Dockerfile は、Node.js アプリケーションを `node:9-alpine` イメージに追加し、コンテナーをポート 80 上のアプリケーションのサーバーとして構成します。</span><span class="sxs-lookup"><span data-stu-id="80f9d-109">This Dockerfile adds a Node.js application to the `node:9-alpine` image and configures the container to server the application on port 80.</span></span>

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="80f9d-110">次のコマンドを実行して、Dockerfile からコンテナー イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="80f9d-110">Run the following command to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="80f9d-111">この操作を実行すると、イメージがビルドされ、コンテナー レジストリにプッシュされます。</span><span class="sxs-lookup"><span data-stu-id="80f9d-111">As this operation runs, you will see the image being built and pushed to your container registry.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="80f9d-112">イメージを確認する</span><span class="sxs-lookup"><span data-stu-id="80f9d-112">Verify the image</span></span>

<span data-ttu-id="80f9d-113">ビルド操作が完了したら、次のコマンドを実行して、イメージが作成されてレジストリに格納されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="80f9d-113">When the build operation has completed, run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="80f9d-114">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="80f9d-114">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="80f9d-115">これで、`helloacrbuild` イメージが使用できる状態になりました。</span><span class="sxs-lookup"><span data-stu-id="80f9d-115">The `helloacrbuild` image is now ready to be used.</span></span>

## <a name="summary"></a><span data-ttu-id="80f9d-116">まとめ</span><span class="sxs-lookup"><span data-stu-id="80f9d-116">Summary</span></span>

<span data-ttu-id="80f9d-117">このユニットでは、Azure Container Registry Build を使用してコンテナー イメージの作成を自動化しました。</span><span class="sxs-lookup"><span data-stu-id="80f9d-117">In this unit, Azure Container Registry Build was used to automate the creation of a container image.</span></span> <span data-ttu-id="80f9d-118">続いて、このイメージが Azure コンテナー レジストリに格納されました。</span><span class="sxs-lookup"><span data-stu-id="80f9d-118">This image was then stored in the Azure container registry.</span></span> <span data-ttu-id="80f9d-119">次のユニットでは、新しいコンテナー イメージのインスタンスを Azure Container Instances にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="80f9d-119">In the next unit, you will deploy an instance of the new container image to Azure Container Instances.</span></span>