<span data-ttu-id="9c960-101">キューを操作するクライアント アプリケーションを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="9c960-101">Let's create a client application to work with a queue.</span></span> <span data-ttu-id="9c960-102">次に、コードに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="9c960-102">Then we'll add our connection string to the code.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="9c960-103">アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="9c960-103">Create the application</span></span>

<span data-ttu-id="9c960-104">Linux、macOS、または Windows で実行できる .NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="9c960-104">We'll create a .NET Core application that you can run on Linux, macOS, or Windows.</span></span> <span data-ttu-id="9c960-105">これに **QueueApp** という名前を付けましょう。</span><span class="sxs-lookup"><span data-stu-id="9c960-105">Let's name it **QueueApp**.</span></span> <span data-ttu-id="9c960-106">わかりやすくするため、キューを介してメッセージの受信と送信の両方を行う単独のアプリを使用します。</span><span class="sxs-lookup"><span data-stu-id="9c960-106">For simplicity, we'll use a single app that will both send and receive messages through our queue.</span></span>

1. <span data-ttu-id="9c960-107">`dotnet new` コマンドを使用して、**QueueApp** という名前の新しいコンソール アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9c960-107">Use the `dotnet new` command to create a new console app with the name **QueueApp**.</span></span> <span data-ttu-id="9c960-108">コマンドの入力は右側の Cloud Shell か、ローカルで作業している場合はターミナル/コンソール ウィンドウで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="9c960-108">You can type commands into the Cloud Shell on the right, or if you are working locally, in a terminal/console window.</span></span> <span data-ttu-id="9c960-109">これにより、1 つのソース ファイルによる、簡単なアプリが作成されます: `Program.cs`。</span><span class="sxs-lookup"><span data-stu-id="9c960-109">This creates a simple app with a single source file: `Program.cs`.</span></span>

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. <span data-ttu-id="9c960-110">新しく作成された `QueueApp` フォルダーに切り替え、アプリをビルドして、すべて問題ないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="9c960-110">Switch to the newly created `QueueApp` folder and build the app to verify that all is well.</span></span>

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a><span data-ttu-id="9c960-111">接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="9c960-111">Get your connection string</span></span>

<span data-ttu-id="9c960-112">Azure Portal のご自分のストレージ アカウントの **[設定] > [アクセス キー]** セクションで、ご自分の接続文字列が使用できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="9c960-112">Recall that your connection string is available in the Azure portal **Settings > Access keys** section of your storage account.</span></span>

<span data-ttu-id="9c960-113">Azure CLI または PowerShell のツールから取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="9c960-113">You can also retrieve it through the Azure CLI or PowerShell tools.</span></span> <span data-ttu-id="9c960-114">Azure CLI のコマンドを使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="9c960-114">Let's use the Azure CLI command.</span></span> <span data-ttu-id="9c960-115">`<name>` はご自分で作成したストレージ アカウントの名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="9c960-115">Remember to replace the `<name>` with the specific name of the storage account you created.</span></span> <span data-ttu-id="9c960-116">確認が必要な場合は、`az storage account list` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="9c960-116">You can use `az storage account list` if you need a reminder.</span></span>

```azurecli
az storage account show-connection-string --name <name> --resource-group ExerciseResources
```

<span data-ttu-id="9c960-117">このコマンドは、ご自分の接続文字列が含まれる JSON ブロックを返します。</span><span class="sxs-lookup"><span data-stu-id="9c960-117">This command will return a JSON block with your connection string.</span></span> <span data-ttu-id="9c960-118">ストレージ アカウント名とアカウント キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9c960-118">It will include the storage account name and the account key:</span></span>

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a><span data-ttu-id="9c960-119">接続文字列をアプリケーションに追加する</span><span class="sxs-lookup"><span data-stu-id="9c960-119">Add the connection string to the application</span></span>

<span data-ttu-id="9c960-120">最後に、接続文字列がストレージ アカウントにアクセスできるように、アプリに追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="9c960-120">Finally, let's add the connection string into our app so it can access the storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="9c960-121">わかりやすくするため、接続文字列を **Program.cs** ファイルに配置します。</span><span class="sxs-lookup"><span data-stu-id="9c960-121">For simplicity, you will place the connection string in the **Program.cs** file.</span></span> <span data-ttu-id="9c960-122">実稼働アプリケーションでは、安全な場所に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9c960-122">In a production application, you should store it in a secure location.</span></span> <span data-ttu-id="9c960-123">サーバー側で使用するために、Azure Key Vault の使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9c960-123">For server side use, we recommend using Azure Key Vault.</span></span>

1. <span data-ttu-id="9c960-124">ターミナルに `code .` と入力して、オンライン コード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="9c960-124">Type `code .` in the terminal to open the online code editor.</span></span> <span data-ttu-id="9c960-125">または、自分で操作する場合は、任意の IDE を使用できます。</span><span class="sxs-lookup"><span data-stu-id="9c960-125">Alternatively, if you are working on your own you can use the IDE of your choice.</span></span> <span data-ttu-id="9c960-126">優れたクロスプラットフォーム IDE である Visual Studio Code をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9c960-126">We recommend Visual Studio Code, which is an excellent cross-platform IDE.</span></span>

1. <span data-ttu-id="9c960-127">プロジェクトの `Program.cs` ソース ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="9c960-127">Open the `Program.cs` source file in the project.</span></span>

1. <span data-ttu-id="9c960-128">接続文字列を保持するため、`Program` クラスに `const string` の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="9c960-128">In the `Program` class, add a `const string` value to hold the connection string.</span></span> <span data-ttu-id="9c960-129">必要なのはこの値のみです (**DefaultEndpointsProtocol** のテキストで始まります)。</span><span class="sxs-lookup"><span data-stu-id="9c960-129">You only need the value (it starts with the text **DefaultEndpointsProtocol**).</span></span>

1. <span data-ttu-id="9c960-130">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="9c960-130">Save the file.</span></span> <span data-ttu-id="9c960-131">クラウド エディターの右上隅にある省略記号 "..." をクリックすると、キーボード ショートカットも表示されます。</span><span class="sxs-lookup"><span data-stu-id="9c960-131">You can click the ellipse "..." in the right corner of the cloud editor, it will also show you the keyboard shortcut.</span></span>

<span data-ttu-id="9c960-132">次のようなコードが表示されるはずです (文字列値は、アカウントに対して一意になります)。</span><span class="sxs-lookup"><span data-stu-id="9c960-132">Your code should look something like this (the string value will be unique to your account).</span></span>

```csharp
...
namespace QueueApp
{
    class Program
    {
        private const string ConnectionString = "DefaultEndpointsProtocol=https; ...";
        
        ...
    }
}
```

<span data-ttu-id="9c960-133">このスタート プロジェクトがセットアップされたので、コードでキューを操作する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="9c960-133">Now that we have this starter project setup, let's look at how to work with a queue in code.</span></span> <span data-ttu-id="9c960-134">すべて_メッセージ_で始まります。</span><span class="sxs-lookup"><span data-stu-id="9c960-134">It all starts with _messages_.</span></span>