<span data-ttu-id="7899f-101">ビルドするアプリケーションはクロスプラットフォームのモバイル アプリであり、Azure 関数と通信を行って場所を共有します。</span><span class="sxs-lookup"><span data-stu-id="7899f-101">The application you're building is a cross-platform mobile app that talks to an Azure function to share your location.</span></span> <span data-ttu-id="7899f-102">このユニットでは、Visual Studio を使用して空のモバイル アプリを作成し、ユーザーの場所を取得するための API を含む NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7899f-102">In this unit, you create the blank mobile app using Visual Studio and install a NuGet package that has an API for getting the user's location.</span></span>

## <a name="create-the-xamarinforms-project"></a><span data-ttu-id="7899f-103">Xamarin.Forms プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="7899f-103">Create the Xamarin.Forms project</span></span>

1. <span data-ttu-id="7899f-104">Visual Studio で、*[ファイル]、[新規]、[プロジェクト]* の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="7899f-104">From Visual Studio, select *File->New->Project...*.</span></span>

1. <span data-ttu-id="7899f-105">左側にあるツリーから、*[Visual C#]、[クロス プラットフォーム]* の順に選択し、中央のパネルから *[モバイル アプリ (Xamarin.Forms)]* を選びます。</span><span class="sxs-lookup"><span data-stu-id="7899f-105">From the tree on the left-hand side, select *Visual C#->Cross-Platform* and then select *Mobile App (Xamarin.Forms)* from the panel in the center.</span></span>

1. <span data-ttu-id="7899f-106">ソリューションに "ImHere" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="7899f-106">Name the solution "ImHere".</span></span>

1. <span data-ttu-id="7899f-107">ソリューションに適した場所を選びます。</span><span class="sxs-lookup"><span data-stu-id="7899f-107">Choose an appropriate location for the solution.</span></span>

    > <span data-ttu-id="7899f-108">このモジュールを Windows でローカルに実行しており、Android 用のビルドを計画している場合は、パスをできるだけ短くしてください。</span><span class="sxs-lookup"><span data-stu-id="7899f-108">If you are running this module locally on Windows and are planning on building for Android, then make sure the path is as short as possible.</span></span> <span data-ttu-id="7899f-109">Android SDK にはパスの長さに関する制限があるため、ルート パスをできるだけ短くする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7899f-109">There are path length limitations with the Android SDK, so your root path should be as short as possible.</span></span>

1. <span data-ttu-id="7899f-110">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="7899f-110">Click **OK**.</span></span>

    ![[新しいソリューション] ダイアログ](../media-drafts/2-new-solution-dialog.png)

1. <span data-ttu-id="7899f-112">**[New Cross Platform App]\(新しいクロス プラットフォーム アプリ\)** ダイアログで、*[空のアプリ]* テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="7899f-112">From the **New Cross Platform App** dialog, select the *Blank App* template.</span></span>

1. <span data-ttu-id="7899f-113">このモジュールでは、UWP アプリをビルドするため、iOS と Android をオフにし、UWP はオンのままにします。</span><span class="sxs-lookup"><span data-stu-id="7899f-113">For this module you will build a UWP app, so uncheck iOS and Android and leave UWP checked.</span></span>

    > <span data-ttu-id="7899f-114">ローカルでこれを実行する場合は、Android をオンのままにしておいてかまいません。Android SDK は、Visual Studio 内で *[.NET によるモバイル開発]* ワークロードの一部としてインストールされるためです。</span><span class="sxs-lookup"><span data-stu-id="7899f-114">If you are running this locally, then you can leave Android checked because the Android SDK is installed as part of the *Mobile Development With .NET* workload inside Visual Studio.</span></span> <span data-ttu-id="7899f-115">また iOS 用にビルドする場合は、macOS ビルド エージェントにペアリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7899f-115">If you also want to build for iOS, you will need to pair to a macOS build agent.</span></span> <span data-ttu-id="7899f-116">これに関する詳細については、[Xamarin iOS のドキュメント](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/connecting-to-mac/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7899f-116">You can read more on this in the [Xamarin iOS docs](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/connecting-to-mac/).</span></span>

1. <span data-ttu-id="7899f-117">*[コード共有方法]* では、**[.NET Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7899f-117">For the *Code Sharing Strategy*, select **.NET Standard**.</span></span>

1. <span data-ttu-id="7899f-118">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="7899f-118">Click **OK**.</span></span>

    ![新しいソリューションの構成ダイアログ](../media-drafts/2-configure-solution-dialog.png)

<span data-ttu-id="7899f-120">Visual Studio では 2 つのプロジェクト (`ImHere.UWP` という UWP アプリと `ImHere` という .NET 標準ライブラリ) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="7899f-120">Visual Studio will create two projects for you - a UWP app called `ImHere.UWP` and a .NET standard library, `ImHere`.</span></span> <span data-ttu-id="7899f-121">Xamarin.Forms アプリは 2 つの部分、つまり、1 つ以上のプラットフォーム固有のアプリ プロジェクトと 1 つ (またはそれ以上の) .NET 標準ライブラリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="7899f-121">Xamarin.Forms apps are made up of two parts - one or more platform-specific app projects and one (or more) .NET standard libraries.</span></span> <span data-ttu-id="7899f-122">プラットフォーム固有のアプリ プロジェクトには、関連するプラットフォームでアプリを実行するために必要なプラットフォーム固有のコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7899f-122">The platform-specific app projects contain the platform-specific code needed to run an app on the relevant platform.</span></span> <span data-ttu-id="7899f-123">その後、これらのプロジェクトでは、クロスプラットフォームの .NET 標準ライブラリで定義されている Xamarin.Forms アプリが起動されます。</span><span class="sxs-lookup"><span data-stu-id="7899f-123">These projects then launch a Xamarin.Forms app that is defined in a cross-platform .NET standard library.</span></span> <span data-ttu-id="7899f-124">クロスプラットフォーム コードでアプリをビルドすると、実行時に、作成するすべてのユーザー インターフェイスが、関連するプラットフォーム固有の UI コンポーネントに変換されます。</span><span class="sxs-lookup"><span data-stu-id="7899f-124">You build your app in cross-platform code and, at runtime, any user interfaces you create are translated into the relevant platform-specific UI components.</span></span>

## <a name="adding-xamarinessentials"></a><span data-ttu-id="7899f-125">Xamarin.Essentials の追加</span><span class="sxs-lookup"><span data-stu-id="7899f-125">Adding Xamarin.Essentials</span></span>

<span data-ttu-id="7899f-126">UWP、Android、iOS プラットフォームでは、オペレーティング システムとハードウェアを利用する数多くの同様の機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="7899f-126">The UWP, Android, and iOS platforms provide numerous similar capabilities that take advantage of the operating system and hardware.</span></span> <span data-ttu-id="7899f-127">これらは似ていますが、API は大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="7899f-127">Despite these similarities, the APIs are very different.</span></span> <span data-ttu-id="7899f-128">クロスプラットフォーム コードからこれらの API を使用するには、.NET 標準ライブラリに公開するアプリ プロジェクトでプラットフォーム固有のコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7899f-128">Using these APIs from cross-platform code requires writing platform-specific code in your app projects that you expose to your .NET standard libraries.</span></span> <span data-ttu-id="7899f-129">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/) は NuGet パッケージであり、これらの多くの API でのクロスプラットフォームの抽象化が提供されます。これらの API には、ユーザーの場所を取得するためにアプリで使用する地理位置情報 API が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7899f-129">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/) is a NuGet package that provides a cross-platform abstraction over a number of these APIs, including the geolocation APIs that you will use in your app to get the user's location.</span></span>

1. <span data-ttu-id="7899f-130">Visual Studio のソリューション エクスプローラーで `ImHere` ソリューションを右クリックし、*[ソリューションの NuGet パッケージの管理]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="7899f-130">Right-click on the `ImHere` solution in the Visual Studio Solution Explorer and select *Manage NuGet Packages for Solution...*.</span></span>

1. <span data-ttu-id="7899f-131">**[参照]** タブを選択し、"Xamarin.Essentials" を検索します。</span><span class="sxs-lookup"><span data-stu-id="7899f-131">Select the **Browse** tab and search for "Xamarin.Essentials".</span></span> <span data-ttu-id="7899f-132">このパッケージは現在、プレリリース版の NuGet パッケージとして利用可能であるため、*[include prelease]\(プレリリースを含める\)* ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="7899f-132">This package is currently available as a prerelease NuGet package, so check the *include prelease* box.</span></span>

1. <span data-ttu-id="7899f-133">**[Xamarin.Essentials]** NuGet パッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="7899f-133">Select the **Xamarin.Essentials** NuGet package.</span></span>

1. <span data-ttu-id="7899f-134">右側にあるプロジェクトの一覧で自分のプロジェクトをすべて確認します。</span><span class="sxs-lookup"><span data-stu-id="7899f-134">Check all your projects in the project list on the right-hand side.</span></span>

1. <span data-ttu-id="7899f-135">**[インストール]** ボタンをクリックして、NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7899f-135">Click the **Install** button to install the NuGet package.</span></span> <span data-ttu-id="7899f-136">続行するにはライセンスに同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7899f-136">You'll need to accept the license to continue.</span></span>

    ![ソリューション内のすべてのプロジェクトへの Xamarin.Essentials NuGet パッケージの追加](../media-drafts/2-add-essentials-nuget.png)

    > <span data-ttu-id="7899f-138">このモジュールをローカルで実行し、Android を対象とする場合は、追加のセットアップをいくつか行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7899f-138">If you are running this module locally and want to target Android, you'll need to do some additional setup.</span></span> <span data-ttu-id="7899f-139">詳細については、[Xamarin.Essentials の概要に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/get-started?context=xamarin%2Fios&tabs=windows%2Candroid)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7899f-139">For more information, see the [Xamarin.Essentials Getting Started docs](https://docs.microsoft.com/xamarin/essentials/get-started?context=xamarin%2Fios&tabs=windows%2Candroid).</span></span>

## <a name="building-and-running-the-app"></a><span data-ttu-id="7899f-140">アプリのビルドと実行</span><span class="sxs-lookup"><span data-stu-id="7899f-140">Building and running the app</span></span>

1. <span data-ttu-id="7899f-141">ソリューション エクスプローラーで、`ImHere.UWP` プロジェクトを右クリックして *[スタートアップ プロジェクトに設定]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="7899f-141">Right-click on the `ImHere.UWP` project in Solution Explorer and select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="7899f-142">ビルド構成を **[デバッグ]** に、プラットフォームを **[x86]** に、実行先のデバイスを **[ローカル コンピューター]** にそれぞれ設定します。</span><span class="sxs-lookup"><span data-stu-id="7899f-142">Set the build configuration to **Debug**, the platform to **x86**, and the device to run on to **Local Machine**.</span></span>

    ![ローカル デバイスで実行する場合のデバッグ x86 構成の設定](../media-drafts/2-debug-configuration.png)

1. <span data-ttu-id="7899f-144">アプリのデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="7899f-144">Start debugging the app.</span></span>

    ![実行中のアプリ](../media-drafts/2-debuging-app.png)

## <a name="summary"></a><span data-ttu-id="7899f-146">まとめ</span><span class="sxs-lookup"><span data-stu-id="7899f-146">Summary</span></span>

<span data-ttu-id="7899f-147">このユニットでは、新しい Xamarin.Forms クロスプラットフォームのモバイル アプリを作成し、Xamarin.Essentials NuGet パッケージを追加しました。</span><span class="sxs-lookup"><span data-stu-id="7899f-147">In this unit, you created a new Xamarin.Forms cross-platform mobile app and added the Xamarin.Essentials NuGet package.</span></span> <span data-ttu-id="7899f-148">次は、モバイル アプリの UI とロジックを構築する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="7899f-148">Next, you learn how to build up the mobile app UI and logic.</span></span>