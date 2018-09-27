<span data-ttu-id="d9e30-101">ビルドするアプリケーションはクロスプラットフォームのモバイル アプリであり、Azure 関数と通信を行って場所を共有します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-101">The application you're building is a cross-platform mobile app that talks to an Azure function to share your location.</span></span> <span data-ttu-id="d9e30-102">このユニットでは、Visual Studio を使用して空のモバイル アプリを作成し、ユーザーの場所を取得するための API を含む NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d9e30-102">In this unit, you create the blank mobile app using Visual Studio and install a NuGet package that has an API for getting the user's location.</span></span>

## <a name="create-the-xamarinforms-project"></a><span data-ttu-id="d9e30-103">Xamarin.Forms プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="d9e30-103">Create the Xamarin.Forms project</span></span>

1. <span data-ttu-id="d9e30-104">Visual Studio で、*[ファイル]、[新規]、[プロジェクト]* の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-104">From Visual Studio, select *File->New->Project...*.</span></span>

1. <span data-ttu-id="d9e30-105">左側にあるツリーから、*[Visual C#]、[クロス プラットフォーム]* の順に選択し、中央のパネルから *[モバイル アプリ (Xamarin.Forms)]* を選びます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-105">From the tree on the left-hand side, select *Visual C#->Cross-Platform* and then select *Mobile App (Xamarin.Forms)* from the panel in the center.</span></span>

1. <span data-ttu-id="d9e30-106">ソリューションに "ImHere" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-106">Name the solution "ImHere".</span></span>

1. <span data-ttu-id="d9e30-107">ソリューションに適した場所を選びます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-107">Choose an appropriate location for the solution.</span></span>

1. <span data-ttu-id="d9e30-108">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d9e30-108">Click **OK**.</span></span>

    ![[新しいソリューション] ダイアログ](../media/2-new-solution-dialog.png)

1. <span data-ttu-id="d9e30-110">**[New Cross Platform App]\(新しいクロス プラットフォーム アプリ\)** ダイアログで、*[空のアプリ]* テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-110">From the **New Cross Platform App** dialog, select the *Blank App* template.</span></span>

1. <span data-ttu-id="d9e30-111">このモジュールでは、UWP アプリをビルドするため、iOS と Android をオフにし、UWP はオンのままにします。</span><span class="sxs-lookup"><span data-stu-id="d9e30-111">For this module you will build a UWP app, so uncheck iOS and Android and leave UWP checked.</span></span>

1. <span data-ttu-id="d9e30-112">*[コード共有方法]* では、**[.NET Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-112">For the *Code Sharing Strategy*, select **.NET Standard**.</span></span>

1. <span data-ttu-id="d9e30-113">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9e30-113">Click **OK**.</span></span>

    ![新しいソリューションの構成ダイアログ](../media/2-configure-solution-dialog.png)

<span data-ttu-id="d9e30-115">Visual Studio では 2 つのプロジェクト (`ImHere.UWP` という UWP アプリと `ImHere` という .NET Standard ライブラリ) が自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-115">Visual Studio will create two projects for you - a UWP app called `ImHere.UWP` and a .NET Standard library, `ImHere`.</span></span> <span data-ttu-id="d9e30-116">Xamarin.Forms アプリは 2 つの部分、つまり、1 つ以上のプラットフォーム固有のアプリ プロジェクトと 1 つ (またはそれ以上の) .NET Standard ライブラリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-116">Xamarin.Forms apps are made up of two parts - one or more platform-specific app projects and one (or more) .NET Standard libraries.</span></span> <span data-ttu-id="d9e30-117">プラットフォーム固有のアプリ プロジェクトには、関連するプラットフォーム上でアプリを実行するために必要なプラットフォーム固有のコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-117">The platform-specific app projects contain the platform-specific code needed to run an app on the relevant platform.</span></span> <span data-ttu-id="d9e30-118">その後、これらのプロジェクトでは、クロスプラットフォームの .NET Standard ライブラリで定義されている Xamarin.Forms アプリが起動されます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-118">These projects then launch a Xamarin.Forms app that is defined in a cross-platform .NET Standard library.</span></span> <span data-ttu-id="d9e30-119">クロスプラットフォーム コードでアプリをビルドすると、実行時に、作成するすべてのユーザー インターフェイスが、関連するプラットフォーム固有の UI コンポーネントに変換されます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-119">You build your app in cross-platform code and, at runtime, any user interfaces you create are translated into the relevant platform-specific UI components.</span></span>

## <a name="adding-xamarinessentials"></a><span data-ttu-id="d9e30-120">Xamarin.Essentials の追加</span><span class="sxs-lookup"><span data-stu-id="d9e30-120">Adding Xamarin.Essentials</span></span>

<span data-ttu-id="d9e30-121">UWP、Android、iOS プラットフォームでは、オペレーティング システムとハードウェアを利用する数多くの同様の機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-121">The UWP, Android, and iOS platforms provide numerous similar capabilities that take advantage of the operating system and hardware.</span></span> <span data-ttu-id="d9e30-122">これらは似ていますが、API は大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="d9e30-122">Despite these similarities, the APIs are very different.</span></span> <span data-ttu-id="d9e30-123">クロスプラットフォーム コードからこれらの API を使用するには、ご利用の .NET Standard ライブラリに公開するご利用のアプリ プロジェクト内でプラットフォーム固有のコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d9e30-123">Using these APIs from cross-platform code requires writing platform-specific code in your app projects that you expose to your .NET Standard libraries.</span></span> <span data-ttu-id="d9e30-124">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true)は、プラットフォーム固有のコードを記述しなくても済むように、複数のこのような API に対してクロス プラットフォームの抽象化を提供する NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="d9e30-124">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) is a NuGet package that provides a cross-platform abstraction over a number of these APIs so that you don't need to write platform-specific code.</span></span> <span data-ttu-id="d9e30-125">これには、ご利用のアプリ内でユーザーの場所を取得するために使用する位置情報 API が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d9e30-125">This includes the geolocation APIs that you will use in your app to get the user's location.</span></span>

1. <span data-ttu-id="d9e30-126">Visual Studio のソリューション エクスプローラーで `ImHere` ソリューション (`ImHere` .NET Standard プロジェクトではなく、上位レベルのソリューション) を右クリックし、*[ソリューションの NuGet パッケージの管理]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-126">Right-click on the `ImHere` solution (the top level solution, not the `ImHere` .NET Standard project) in the Visual Studio Solution Explorer and select *Manage NuGet Packages for Solution...*.</span></span>

1. <span data-ttu-id="d9e30-127">**[参照]** タブを選択し、"Xamarin.Essentials" を検索します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-127">Select the **Browse** tab and search for "Xamarin.Essentials".</span></span> <span data-ttu-id="d9e30-128">このパッケージは現在、プレリリース版の NuGet パッケージとして利用可能であるため、*[include prelease]\(プレリリースを含める\)* ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="d9e30-128">This package is currently available as a prerelease NuGet package, so check the *include prelease* box.</span></span>

    > [!TIP]
    > <span data-ttu-id="d9e30-129">Xamarin.Essentials NuGet パッケージが表示されない場合は、*[include prelease]\(プレリリースを含める\)* がオンになっているかもう一度確認してください。</span><span class="sxs-lookup"><span data-stu-id="d9e30-129">If you do not see the Xamarin.Essentials NuGet package, double check that *include prelease* is checked.</span></span> 

1. <span data-ttu-id="d9e30-130">**[Xamarin.Essentials]** NuGet パッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-130">Select the **Xamarin.Essentials** NuGet package.</span></span>

1. <span data-ttu-id="d9e30-131">右側にあるプロジェクトの一覧で自分のプロジェクトをすべて確認します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-131">Check all your projects in the project list on the right-hand side.</span></span>

1. <span data-ttu-id="d9e30-132">**[インストール]** ボタンをクリックして、NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d9e30-132">Click the **Install** button to install the NuGet package.</span></span> <span data-ttu-id="d9e30-133">続行するにはライセンスに同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d9e30-133">You'll need to accept the license to continue.</span></span>

    ![ソリューション内のすべてのプロジェクトへの Xamarin.Essentials NuGet パッケージの追加](../media/2-add-essentials-nuget.png)

## <a name="building-and-running-the-app"></a><span data-ttu-id="d9e30-135">アプリのビルドと実行</span><span class="sxs-lookup"><span data-stu-id="d9e30-135">Building and running the app</span></span>

1. <span data-ttu-id="d9e30-136">ソリューション エクスプローラーで、`ImHere.UWP` プロジェクトを右クリックして *[スタートアップ プロジェクトに設定]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-136">Right-click on the `ImHere.UWP` project in Solution Explorer and select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="d9e30-137">ビルド構成を **[デバッグ]** に、プラットフォームを **[x86]** に、実行先のデバイスを **[ローカル コンピューター]** にそれぞれ設定します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-137">Set the build configuration to **Debug**, the platform to **x86**, and the device to run on to **Local Machine**.</span></span>

    ![ローカル デバイスで実行する場合のデバッグ x86 構成の設定](../media/2-debug-configuration.png)

1. <span data-ttu-id="d9e30-139">アプリのデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-139">Start debugging the app.</span></span>

    ![実行中のアプリ](../media/2-debuging-app.png)

## <a name="summary"></a><span data-ttu-id="d9e30-141">まとめ</span><span class="sxs-lookup"><span data-stu-id="d9e30-141">Summary</span></span>

<span data-ttu-id="d9e30-142">このユニットでは、新しい Xamarin.Forms クロスプラットフォームのモバイル アプリを作成し、Xamarin.Essentials NuGet パッケージを追加しました。</span><span class="sxs-lookup"><span data-stu-id="d9e30-142">In this unit, you created a new Xamarin.Forms cross-platform mobile app and added the Xamarin.Essentials NuGet package.</span></span> <span data-ttu-id="d9e30-143">次は、モバイル アプリの UI とロジックを構築する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="d9e30-143">Next, you learn how to build up the mobile app UI and logic.</span></span>