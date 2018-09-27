<span data-ttu-id="8dfc3-101">保管した写真やその他のデータをユーザーの代わりに Azure Storage で管理する写真共有アプリケーションについて学習していることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-101">Recall that we are working on a photo-sharing application that will use Azure Storage to manage pictures and other bits of data we store on behalf of our users.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="8dfc3-102">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="8dfc3-102">::: zone pivot="csharp"</span></span>

<span data-ttu-id="8dfc3-103">Storage API に集中できるようにシナリオを簡単にする目的で、新しい .NET Core Console アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-103">To simplify our scenario so that we can focus on the Storage APIs, we will create a new .NET Core Console application.</span></span> <span data-ttu-id="8dfc3-104">また、ネットワークに常時接続できるものと想定します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-104">We will also assume it always has network connectivity.</span></span> <span data-ttu-id="8dfc3-105">ただし、ネットワークの故障が利用者に影響を与えないように、アプリケーション自体の失敗につながらないように、常にアプリを強固にしてください。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-105">However, you should always harden your app to ensure network failures will not impact the user experience or result in a failure of the application itself.</span></span>

## <a name="create-a-net-core-application"></a><span data-ttu-id="8dfc3-106">.NET Core アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="8dfc3-106">Create a .NET Core application</span></span>

<span data-ttu-id="8dfc3-107">.NET Core は .NET のクロスプラットフォーム版であり、macOS、Windows、Linux で動作します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-107">.NET Core is a cross-platform version of .NET that runs on macOS, Windows, and Linux.</span></span> <span data-ttu-id="8dfc3-108">ツールをローカルでインストールしたり、ウィンドウの右側にある Cloud Shell を使用して下の手順を行ったりすることができます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-108">You can install the tools locally or use the Cloud Shell on the right side of the window to execute the below steps below.</span></span>

1. <span data-ttu-id="8dfc3-109">Cloud Shell にサインインするか、コマンド ライン セッションを開き、"PhotoSharingApp" という名前で新しい .NET Core Console アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-109">Sign into the Cloud Shell or open a command-line session and create a new .NET Core Console application with the name "PhotoSharingApp".</span></span> <span data-ttu-id="8dfc3-110">`-o` または `--output` フラグを追加し、特定のフォルダーでアプリを作成できます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-110">You can add the `-o` or `--output` flag to create the app in a specific folder.</span></span>

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. <span data-ttu-id="8dfc3-111">アプリを実行し、正しくビルドされ、実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-111">Run the app to make sure it builds and executes correctly.</span></span> <span data-ttu-id="8dfc3-112">"Hello, World!" と</span><span class="sxs-lookup"><span data-stu-id="8dfc3-112">It should display "Hello World!"</span></span> <span data-ttu-id="8dfc3-113">コンソールに表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-113">to the console.</span></span>

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
<span data-ttu-id="8dfc3-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="8dfc3-114">::: zone-end</span></span>

<span data-ttu-id="8dfc3-115">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="8dfc3-115">::: zone pivot="javascript"</span></span>

<span data-ttu-id="8dfc3-116">Storage API に集中できるようにシナリオを簡単にする目的で、コンソールから実行できる新しい Node.js アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-116">To simplify our scenario so that we can focus on the Storage APIs, we will create a new Node.js application that can run from the console.</span></span> <span data-ttu-id="8dfc3-117">また、ネットワークに常時接続できるものと想定します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-117">We will also assume it always has network connectivity.</span></span> <span data-ttu-id="8dfc3-118">ただし、ネットワークの故障が利用者に影響を与えないように、アプリケーション自体の失敗につながらないように、常にアプリを強固にしてください。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-118">However, you should always harden your app to ensure network failures will not impact the user experience, or result in a failure of the application itself.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="8dfc3-119">Node.js アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="8dfc3-119">Create a Node.js application</span></span>

<span data-ttu-id="8dfc3-120">Node.js は JavaScript アプリの実行に利用される人気のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-120">Node.js is a popular framework for running JavaScript apps.</span></span> <span data-ttu-id="8dfc3-121">一般的には Web アプリで使用されることが多いですが、コマンド ラインからロジックを実行するために使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-121">It is most commonly used for web apps, but you can use it to run logic from the command line as well.</span></span> <span data-ttu-id="8dfc3-122">ツールをローカルでインストールした場合、コマンド ラインから次の手順を実行できます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-122">If you have the tools installed locally, you can run the following steps from a command line.</span></span> <span data-ttu-id="8dfc3-123">あるいは、ウィンドウの右側にある Cloud Shell を使用して下の手順を行います。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-123">Alternatively, you can use the Cloud Shell on the right side of the window to execute the below steps.</span></span>

1. <span data-ttu-id="8dfc3-124">Cloud Shell にサインインするか、コマンド ライン セッションを開き、"PhotoSharingApp" という名前で新しいフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-124">Sign into the Cloud Shell or open a command-line session and create a new folder named "PhotoSharingApp".</span></span>

    ```bash
    mkdir PhotoSharingApp
    ```

1. <span data-ttu-id="8dfc3-125">新しいフォルダーに変更し、`npm` を使用して新しい Node.js アプリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-125">Change into the new folder and use `npm` to initialize a new Node.js app.</span></span> <span data-ttu-id="8dfc3-126">これにより、アプリを記述するメタデータを含む **package.json** ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-126">This will create a **package.json** file containing metadata that describes the app.</span></span>

    ```bash
    cd PhotoSharingApp
    npm init -y
    ```

1. <span data-ttu-id="8dfc3-127">新しいソース ファイルとして **index.js** を作成します。コードはこのファイルに置かれます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-127">Create a new source file **index.js**, which is where our code will go.</span></span>

    ```bash
    touch index.js
    ```

1. <span data-ttu-id="8dfc3-128">エディターで **index.js** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-128">Open the **index.js** file with an editor.</span></span> <span data-ttu-id="8dfc3-129">Cloud Shell を使用する場合、`code .` と入力してエディターを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-129">If you are using the Cloud Shell, you can type `code .` to open an editor.</span></span>

1. <span data-ttu-id="8dfc3-130">**index.js** ファイルに次のプログラムを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-130">Paste the following program into the **index.js** file.</span></span> <span data-ttu-id="8dfc3-131">貼り付けるには、**Ctrl + V** ショートカット キーを使用するか、または右クリックします。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-131">You can use the **Ctrl+V** keyboard shortcut or right-click to paste.</span></span>

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. <span data-ttu-id="8dfc3-132">ファイルを保存します &mdash; Cloud Shell エディターの右上隅にある [...] メニューを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-132">Save the file&mdash;you can use the "..." menu on the top right corner of the Cloud Shell editor.</span></span>

1. <span data-ttu-id="8dfc3-133">アプリを実行し、正しく実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-133">Run the app to make sure it executes correctly.</span></span> <span data-ttu-id="8dfc3-134">"Hello, World!" と</span><span class="sxs-lookup"><span data-stu-id="8dfc3-134">It should display "Hello, World!"</span></span> <span data-ttu-id="8dfc3-135">コンソールに表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="8dfc3-135">to the console.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="8dfc3-136">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="8dfc3-136">::: zone-end</span></span>