<span data-ttu-id="a1ec9-101">この演習では、よく使用される値を格納および返すために、Azure Redis Cache インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-101">In this exercise, you'll create an Azure Redis Cache instance to store and return commonly used values.</span></span>

## <a name="create-a-cache"></a><span data-ttu-id="a1ec9-102">キャッシュを作成する</span><span class="sxs-lookup"><span data-stu-id="a1ec9-102">Create a cache</span></span>

1. <span data-ttu-id="a1ec9-103">Azure portal サインインします。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-103">Sign in to the Azure portal.</span></span> <span data-ttu-id="a1ec9-104">**[Create a resource]** \(リソースの作成\)、**[データベース]**、**[Redis Cache]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-104">Then click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="a1ec9-105">次のスクリーン ショットでは、Azure portal のさまざまなデータベース リソース オプション内の Redis Cache の場所を示しています。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![[Create a resource]\(リソースの作成\)、[データベース]、[Redis Cache] オプションが強調表示された、Azure portal データベースのオプションを示すスクリーンショット。](../media/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a><span data-ttu-id="a1ec9-107">キャッシュを構成する</span><span class="sxs-lookup"><span data-stu-id="a1ec9-107">Configure your cache</span></span>

1. <span data-ttu-id="a1ec9-108">**DNS 名:** **ContosoSportsApp1028** など、グローバルに一意の名前を作成します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-108">**DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.</span></span>

1. <span data-ttu-id="a1ec9-109">**サブスクリプション**: ご使用のサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-109">**Subscription:** Select your subscription.</span></span>

1. <span data-ttu-id="a1ec9-110">**リソース グループ**: 新しいリソース グループを作成し、名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-110">**Resource group:** Create a new resource group and give it a name.</span></span>

1. <span data-ttu-id="a1ec9-111">**場所:** ほとんどのお客様の地域はニューヨークなので、**[米国東部]** を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-111">**Location:** Because most of your customers are in the New York area, you should select **East US**.</span></span>

1. <span data-ttu-id="a1ec9-112">**価格レベル**: **[Basic C0]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-112">**Pricing tier:** Select **Basic C0**.</span></span>

1. <span data-ttu-id="a1ec9-113">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-113">Click **Create**.</span></span>

    <span data-ttu-id="a1ec9-114">次のスクリーンショットは、新しい Redis Cache のリソースの作成に使用された、代表的な構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-114">The following screenshot shows a representative configuration used to create a new Redis Cache resource.</span></span>

    ![DNS 名、サブスクリプション、新しいリソース グループ、場所、価格レベルの構成例が入力された、新しい Redis Cache のリソースを作成するときの Azure portal のブレードのスクリーンショット。](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> <span data-ttu-id="a1ec9-116">続行する前に、キャッシュがデプロイされるまで待つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-116">You will have to wait until the cache is deployed before continuing.</span></span> <span data-ttu-id="a1ec9-117">このプロセスにはしばらく時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-117">This process might take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="a1ec9-118">アクセス キーおよびホスト名を取得する</span><span class="sxs-lookup"><span data-stu-id="a1ec9-118">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="a1ec9-119">Azure portal で、ご自分のキャッシュを参照し、**[設定]**  >  **[アクセス キー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-119">In the Azure portal, browse to your cache and select **Settings** > **Access keys**.</span></span> <span data-ttu-id="a1ec9-120">**[プライマリ接続文字列 (StackExchange.Redis)]** のメモをとります。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-120">Write down the **Primary connection string (StackExchange.Redis)**.</span></span>

    <span data-ttu-id="a1ec9-121">このキーには、使用している StackExchange.Redis パッケージのアプリケーション設定で使用する完全な接続文字列に、ご使用のプライマリ キーとホスト名を含みます。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-121">This key includes your primary key and host name in a complete connection string for use within your application settings for the StackExchange.Redis package we are using.</span></span>

1. <span data-ttu-id="a1ec9-122">お使いのコンピューターでメモ帳か別のテキスト エディターを起動します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-122">Start Notepad or another text editor on your computer.</span></span>

1. <span data-ttu-id="a1ec9-123">次の内容を追加します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-123">Add the following contents:</span></span>

    <span data-ttu-id="a1ec9-124">`<connection-string>` を、上記のご使用のキャッシュのプライマリ接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-124">Replace `<connection-string>` with your cache's primary connection string from above.</span></span>

    ```xml
    <appSettings>
        <add key="CacheConnection" value="<connection-string>"/>
    </appSettings>
    ```

1. <span data-ttu-id="a1ec9-125">ファイルを、ご使用のサンプル アプリケーションのソース コードには含まれない場所に保存します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-125">Save the file in a location where it won't be included within the source code of your sample application.</span></span> <span data-ttu-id="a1ec9-126">このチュートリアルでは、ファイルを `C:\AppSecrets\CacheSecrets.config` として保存します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-126">For this tutorial, we will save the file as `C:\AppSecrets\CacheSecrets.config`.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="a1ec9-127">コンソール アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="a1ec9-127">Create a console app</span></span>

1. <span data-ttu-id="a1ec9-128">Visual Studio を起動し、**[ファイル]**  >  **[新規]**  >  **[プロジェクト...]** メニュー項目の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-128">Start Visual Studio and click the **File** > **New** > **Project...** menu item.</span></span>

1. <span data-ttu-id="a1ec9-129">**[Visual C#]**  >  **[Windows デスクトップ]**  >  **[コンソール アプリ]** テンプレートの順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-129">Select the **Visual C#** > **Windows Desktop** > **Console App** template.</span></span>

1. <span data-ttu-id="a1ec9-130">ご自分のアプリに名前を付け、**[OK]** をクリックして、新しいコンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-130">Give your app a name and click **OK** to create a new console application.</span></span>

## <a name="configure-the-cache-client"></a><span data-ttu-id="a1ec9-131">キャッシュ クライアントを構成する</span><span class="sxs-lookup"><span data-stu-id="a1ec9-131">Configure the cache client</span></span>

<span data-ttu-id="a1ec9-132">このセクションでは、.NET に **StackExchange.Redis** クライアントを使用するようにコンソール アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-132">In this section, you will configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="a1ec9-133">Visual Studio で、**[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックし、[パッケージ マネージャー コンソール] ウィンドウから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-133">In Visual Studio, click **Tools**, point to **NuGet Package Manager**, click **Package Manager Console**, and run the following command from the Package Manager Console window:</span></span>

    ```powershell
    Install-Package StackExchange.Redis
    ```

<span data-ttu-id="a1ec9-134">インストールが完了すると、StackExchange.Redis キャッシュ クライアントをプロジェクトに使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-134">Once the installation is completed, the StackExchange.Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="a1ec9-135">キャッシュに接続する</span><span class="sxs-lookup"><span data-stu-id="a1ec9-135">Connect to the cache</span></span>

1. <span data-ttu-id="a1ec9-136">Visual Studio で **ソリューション エクスプローラー**内の **App.config** ファイルを開き、**CacheSecrets.config** ファイルを参照する **appSettings** ファイル属性を含むようにそれを更新します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-136">In Visual Studio, open the **App.config** file within the **Solution Explorer** and update it to include an **appSettings** file attribute that references the **CacheSecrets.config** file.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. <span data-ttu-id="a1ec9-137">ソリューション エクスプローラーで **[参照]** を右クリックし、**[参照の追加]** をクリックします。**[System.Configuration]** を **[アセンブリ]**  >  **[フレームワーク]** の一覧から選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-137">In Solution Explorer, right-click **References** and click **Add Reference...**. Select **System.Configuration** from the **Assemblies** > **Framework** list and click **OK**.</span></span>

1. <span data-ttu-id="a1ec9-138">次の `using` ステートメントを **Program.cs** に追加します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-138">Add the following `using` statements to **Program.cs**:</span></span>

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

<span data-ttu-id="a1ec9-139">Azure Redis Cache への接続は、ConnectionMultiplexer クラスによって管理されています。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-139">The connection to Azure Redis Cache is managed by the ConnectionMultiplexer class.</span></span> <span data-ttu-id="a1ec9-140">このクラスは、クライアント アプリ全体で共有して再利用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-140">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="a1ec9-141">操作ごとに新しい接続を作成しないでください。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-141">Do not create a new connection for each operation.</span></span>

<span data-ttu-id="a1ec9-142">ソース コード内に資格情報を保存することは絶対に避けてください。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-142">Never store credentials in source code.</span></span> <span data-ttu-id="a1ec9-143">このサンプルをシンプルにするために、外部のシークレット構成ファイルのみを使用しています。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-143">To keep this sample simple, we're only using an external secrets config file.</span></span> <span data-ttu-id="a1ec9-144">実際には Azure Key Vault と証明書を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-144">A better approach would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="a1ec9-145">**Program.cs** で、次のメンバーをコンソール アプリの Program クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-145">In **Program.cs**, add the following members to the Program class of your console application:</span></span>

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

<span data-ttu-id="a1ec9-146">アプリで ConnectionMultiplexer インスタンスを共有するこの方法では、接続されたインスタンスを返す静的プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-146">This approach to sharing a ConnectionMultiplexer instance in your application uses a static property that returns a connected instance.</span></span> <span data-ttu-id="a1ec9-147">このコードにより、接続された 1 つの ConnectionMultiplexer インスタンスだけがスレッドセーフな方法で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-147">The code provides a thread-safe way to initialize only a single connected ConnectionMultiplexer instance.</span></span> <span data-ttu-id="a1ec9-148">**abortConnect** は false に設定されており、Azure Redis Cache への接続が確立されていない場合でも呼び出しが成功します。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-148">**abortConnect** is set to false, which means that the call succeeds even if a connection to Azure Redis Cache is not established.</span></span> <span data-ttu-id="a1ec9-149">ConnectionMultiplexer の主な機能の 1 つは、ネットワーク問題などの原因が解決されると、キャッシュへの接続が自動的に復元されることです。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-149">One key feature of ConnectionMultiplexer is that it automatically restores connectivity to the cache once the network issue or other causes are resolved.</span></span>

<span data-ttu-id="a1ec9-150">**CacheConnection** appSetting の値は、Azure portal からパスワード パラメーターとしてキャッシュ接続文字列を参照するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a1ec9-150">The value of the **CacheConnection** appSetting value is used to reference the cache connection string from the Azure portal as the password parameter.</span></span>
