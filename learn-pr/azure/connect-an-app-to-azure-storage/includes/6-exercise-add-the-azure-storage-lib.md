<span data-ttu-id="2aba8-101">::: zone pivot="csharp" Azure Storage クライアント ライブラリを .NET Core コンソール アプリケーションに統合しましょう。</span><span class="sxs-lookup"><span data-stu-id="2aba8-101">::: zone pivot="csharp" Let's integrate the Azure Storage client library into your .NET Core console application.</span></span>

<span data-ttu-id="2aba8-102">.NET 用 Azure Storage クライアント ライブラリは NuGet で配布されます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-102">The Azure storage client library for .NET is distributed with NuGet.</span></span> <span data-ttu-id="2aba8-103">**WindowsAzure.Storage** パッケージをご利用の .NET または .NET Core アプリケーションに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2aba8-103">You will want to add the **WindowsAzure.Storage** package to your .NET or .NET Core applications.</span></span>

## <a name="add-the-azure-storage-nuget-package"></a><span data-ttu-id="2aba8-104">Azure Storage NuGet パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="2aba8-104">Add the Azure Storage NuGet package</span></span>

1. <span data-ttu-id="2aba8-105">Cloud Shell を使用している場合は、正しいフォルダーに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-105">Switch to the correct folder if you are using the Cloud Shell.</span></span>

1. <span data-ttu-id="2aba8-106">**WindowsAzure.Storage** パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="2aba8-106">Add the **WindowsAzure.Storage** package to the application.</span></span>

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. <span data-ttu-id="2aba8-107">これにより、クライアント ライブラリと必要なすべての依存関係がダウンロードされている間、一部のコンソール アクティビティが示されます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-107">This should result in some console activity while the client library and all the required dependencies are downloaded.</span></span> <span data-ttu-id="2aba8-108">完了したら、先に進み、もう一度アプリをビルドして実行し、すべて準備が整ったことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2aba8-108">Once it's done, go ahead and build and run the app again to make sure everything is ready to go.</span></span>

    ```bash
    dotnet run
    ```

1. <span data-ttu-id="2aba8-109">先ほどと同様に、"Hello, World!" と出力されます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-109">As before, it should output "Hello, World!".</span></span>

<span data-ttu-id="2aba8-110">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2aba8-110">::: zone-end</span></span>

<span data-ttu-id="2aba8-111">::: zone-pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="2aba8-111">::: zone-pivot="javascript"</span></span>

<span data-ttu-id="2aba8-112">**Node.js と JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**をアプリケーションに統合しましょう。</span><span class="sxs-lookup"><span data-stu-id="2aba8-112">Let's integrate the **Microsoft Azure Storage Client Library for Node.js and JavaScript** into your application.</span></span>

<span data-ttu-id="2aba8-113">Node.js 用のクライアント ライブラリは、ノード パッケージ マネージャー (NPM) を通じて利用できます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-113">The client library for Node.js is available through the Node Package manager (NPM).</span></span> <span data-ttu-id="2aba8-114">**azure-storage** パッケージを **packages.json** ファイルに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2aba8-114">You will want to add the **azure-storage** package to your **packages.json** file.</span></span>

> [!NOTE]
> <span data-ttu-id="2aba8-115">**Node.js と JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**はサーバー アプリケーション用です。</span><span class="sxs-lookup"><span data-stu-id="2aba8-115">The **Microsoft Azure Storage Client Library for Node.js and JavaScript** is intended for server applications.</span></span> <span data-ttu-id="2aba8-116">クライアント側の JavaScript を実行する場合は、**JavaScript 用の Azure Storage クライアント ライブラリ** (提供される機能は同じですが、ブラウザーでの実行用に調整されています) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2aba8-116">If you are doing client-side JavaScript, you will want to use the **Azure Storage Client Library for JavaScript** which provides the same functionality but is tailored to running in a browser.</span></span>

## <a name="add-the-azure-storage-package"></a><span data-ttu-id="2aba8-117">Azure Storage パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="2aba8-117">Add the Azure Storage package</span></span>

1. <span data-ttu-id="2aba8-118">Cloud Shell を使用している場合は、正しいフォルダーに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-118">Switch to the correct folder if you are using the Cloud Shell.</span></span>

1. <span data-ttu-id="2aba8-119">**azure-storage** パッケージをアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="2aba8-119">Add the **azure-storage** package to the application.</span></span> <span data-ttu-id="2aba8-120">必ず、`--save` オプションを指定して、**packages.json** に保持されるようにしてください。</span><span class="sxs-lookup"><span data-stu-id="2aba8-120">Make sure to supply the `--save` option so it persists to **packages.json**.</span></span>

    ```bash
    npm install azure-storage --save
    ```

1. <span data-ttu-id="2aba8-121">これにより、クライアント ライブラリと必要なすべての依存関係がダウンロードされている間、一部のコンソール アクティビティが示されます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-121">This should result in some console activity while the client library and all the required dependencies are downloaded.</span></span> <span data-ttu-id="2aba8-122">完了したら、先に進み、もう一度アプリをビルドして実行し、すべて準備が整ったことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2aba8-122">Once it's done, go ahead and build and run the app again to make sure everything is ready to go.</span></span>

    ```bash
    node index.js
    ```

1. <span data-ttu-id="2aba8-123">先ほどと同様に、"Hello, World!" と出力されます。</span><span class="sxs-lookup"><span data-stu-id="2aba8-123">As before, it should output "Hello, World!".</span></span>

<span data-ttu-id="2aba8-124">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2aba8-124">::: zone-end</span></span>

<span data-ttu-id="2aba8-125">これで必要なライブラリが準備できました。次は、Azure Storage を操作するためにコードで行う一般的なタスクを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="2aba8-125">Now that we have the necessary libraries in place, let's look at the common tasks you'll do in your code to work with Azure storage.</span></span>
