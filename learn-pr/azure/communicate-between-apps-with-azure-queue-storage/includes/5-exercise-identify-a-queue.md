<span data-ttu-id="5a8b0-101">キューを操作するクライアント アプリケーションを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-101">Let's create a client application to work with a queue.</span></span> <span data-ttu-id="5a8b0-102">次に、コードに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-102">Then we'll add our connection string to the code.</span></span>

> [!NOTE]
> <span data-ttu-id="5a8b0-103">.NET Core をインストールしている場合、クライアント アプリケーションはローカル コンピューターで作成できます。あるいは、Cloud Shell 環境で直接作成できます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-103">You can create the client application on your local computer if you have .NET Core installed, or directly in the Cloud Shell environment.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="5a8b0-104">アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="5a8b0-104">Create the application</span></span>

<span data-ttu-id="5a8b0-105">Linux、macOS、または Windows で実行できる .NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-105">We'll create a .NET Core application that you can run on Linux, macOS, or Windows.</span></span> <span data-ttu-id="5a8b0-106">これに **QueueApp** という名前を付けましょう。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-106">Let's name it **QueueApp**.</span></span> <span data-ttu-id="5a8b0-107">わかりやすくするため、キューを介してメッセージの受信と送信の両方を行う単独のアプリを使用します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-107">For simplicity, we'll use a single app that will both send and receive messages through our queue.</span></span>

1. <span data-ttu-id="5a8b0-108">`dotnet new` コマンドを使用して、**QueueApp** という名前の新しいコンソール アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-108">Use the `dotnet new` command to create a new console app with the name **QueueApp**.</span></span> <span data-ttu-id="5a8b0-109">コマンドの入力は右側の Cloud Shell か、ローカルで作業している場合はターミナル/コンソール ウィンドウで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-109">You can type commands into the Cloud Shell on the right, or if you are working locally, in a terminal/console window.</span></span> <span data-ttu-id="5a8b0-110">このコマンドにより、1 つのソース ファイルによる、シンプルなアプリが作成されます: `Program.cs`。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-110">This command creates a simple app with a single source file: `Program.cs`.</span></span>

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. <span data-ttu-id="5a8b0-111">新しく作成された `QueueApp` フォルダーに切り替え、アプリをビルドして、すべて問題ないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-111">Switch to the newly created `QueueApp` folder and build the app to verify that all is well.</span></span>

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a><span data-ttu-id="5a8b0-112">接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="5a8b0-112">Get your connection string</span></span>

<span data-ttu-id="5a8b0-113">Azure Portal のご自分のストレージ アカウントの **[設定] > [アクセス キー]** セクションで、ご自分の接続文字列が使用できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-113">Recall that your connection string is available in the Azure portal **Settings > Access keys** section of your storage account.</span></span>

<span data-ttu-id="5a8b0-114">Azure CLI または PowerShell のツールから取得することもできます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-114">You can also retrieve it through the Azure CLI or PowerShell tools.</span></span> <span data-ttu-id="5a8b0-115">Azure CLI のコマンドを使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-115">Let's use the Azure CLI command.</span></span> <span data-ttu-id="5a8b0-116">`<name>` はご自分で作成したストレージ アカウントの名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-116">Remember to replace the `<name>` with the specific name of the storage account you created.</span></span> <span data-ttu-id="5a8b0-117">確認が必要な場合は、`az storage account list` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-117">You can use `az storage account list` if you need a reminder.</span></span>

```azurecli
az storage account show-connection-string --name <name> --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="5a8b0-118">このコマンドは、ご自分の接続文字列が含まれる JSON ブロックを返します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-118">This command will return a JSON block with your connection string.</span></span> <span data-ttu-id="5a8b0-119">ストレージ アカウント名とアカウント キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-119">It will include the storage account name and the account key:</span></span>

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a><span data-ttu-id="5a8b0-120">接続文字列をアプリケーションに追加する</span><span class="sxs-lookup"><span data-stu-id="5a8b0-120">Add the connection string to the application</span></span>

<span data-ttu-id="5a8b0-121">最後に、接続文字列がストレージ アカウントにアクセスできるように、アプリに追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-121">Finally, let's add the connection string into our app so it can access the storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="5a8b0-122">わかりやすくするため、接続文字列を **Program.cs** ファイルに配置します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-122">For simplicity, you will place the connection string in the **Program.cs** file.</span></span> <span data-ttu-id="5a8b0-123">実稼働アプリケーションでは、安全な場所に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-123">In a production application, you should store it in a secure location.</span></span> <span data-ttu-id="5a8b0-124">サーバー側で使用するために、Azure Key Vault の使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-124">For server side use, we recommend using Azure Key Vault.</span></span>

1. <span data-ttu-id="5a8b0-125">ターミナルに `code .` と入力して、オンライン コード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-125">Type `code .` in the terminal to open the online code editor.</span></span> <span data-ttu-id="5a8b0-126">または、自分で操作する場合は、任意の IDE を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-126">Alternatively, if you are working on your own you can use the IDE of your choice.</span></span> <span data-ttu-id="5a8b0-127">優れたクロスプラットフォーム IDE である Visual Studio Code をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-127">We recommend Visual Studio Code, which is an excellent cross-platform IDE.</span></span>

1. <span data-ttu-id="5a8b0-128">プロジェクトの `Program.cs` ソース ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-128">Open the `Program.cs` source file in the project.</span></span>

1. <span data-ttu-id="5a8b0-129">接続文字列を保持するため、`Program` クラスに `const string` の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-129">In the `Program` class, add a `const string` value to hold the connection string.</span></span> <span data-ttu-id="5a8b0-130">必要なのはこの値のみです (**DefaultEndpointsProtocol** のテキストで始まります)。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-130">You only need the value (it starts with the text **DefaultEndpointsProtocol**).</span></span>

1. <span data-ttu-id="5a8b0-131">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-131">Save the file.</span></span> <span data-ttu-id="5a8b0-132">クラウド エディターの右隅にある省略記号 "..." をクリックするか、アクセラレータ キーを使用できます (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Command + S</kbd>)。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-132">You can click the ellipse "..." in the right corner of the cloud editor, or use the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

<span data-ttu-id="5a8b0-133">次のようなコードが表示されるはずです (文字列値は、アカウントに対して一意になります)。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-133">Your code should look something like this (the string value will be unique to your account).</span></span>

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

<span data-ttu-id="5a8b0-134">このスタート プロジェクトがセットアップされたので、コードでキューを操作する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-134">Now that we have this starter project setup, let's look at how to work with a queue in code.</span></span> <span data-ttu-id="5a8b0-135">すべて_メッセージ_で始まります。</span><span class="sxs-lookup"><span data-stu-id="5a8b0-135">It all starts with _messages_.</span></span>