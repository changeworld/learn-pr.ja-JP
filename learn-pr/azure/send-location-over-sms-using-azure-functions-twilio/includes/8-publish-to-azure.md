<span data-ttu-id="221e8-101">アプリと Azure 関数が完成し、ローカルで実行されています。</span><span class="sxs-lookup"><span data-stu-id="221e8-101">The app and Azure Function are now complete and running locally.</span></span> <span data-ttu-id="221e8-102">このユニットでは、関数を Azure に発行してクラウドで実行します。</span><span class="sxs-lookup"><span data-stu-id="221e8-102">In this unit, you publish the function to Azure to run in the cloud.</span></span>

> [!Note]
> <span data-ttu-id="221e8-103">関数を Visual Studio から発行します。</span><span class="sxs-lookup"><span data-stu-id="221e8-103">You will publish your function from Visual Studio.</span></span> <span data-ttu-id="221e8-104">これは概念実証、プロトタイプ、学習を始める方法として優れていますが、運用品質のアプリにはこの手法を利用**しないで**ください。</span><span class="sxs-lookup"><span data-stu-id="221e8-104">This is a great way to get started for proof-of-concepts, prototypes, and learning, but for a production-quality app you should **not** use this method.</span></span> <span data-ttu-id="221e8-105">何らかの形式の CI ベースのデプロイを利用してください。</span><span class="sxs-lookup"><span data-stu-id="221e8-105">You should use some form of CI-based deployment.</span></span> <span data-ttu-id="221e8-106">この方法の詳細については、[Azure Functions のデプロイに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="221e8-106">You can read more about doing this in the [Azure Functions Deployment docs](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment?azure-portal=true).</span></span>

## <a name="publishing-your-app-to-azure"></a><span data-ttu-id="221e8-107">アプリを Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="221e8-107">Publishing your app to Azure</span></span>

<span data-ttu-id="221e8-108">Azure 関数は Visual Studio 内から Azure に発行できます。</span><span class="sxs-lookup"><span data-stu-id="221e8-108">Azure functions can be published to Azure from inside Visual Studio.</span></span>

1. <span data-ttu-id="221e8-109">ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。</span><span class="sxs-lookup"><span data-stu-id="221e8-109">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="221e8-110">ソリューション エクスプローラーで `ImHere.Functions` アプリを右クリックし、*[発行]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="221e8-110">Right-click on the `ImHere.Functions` app in the solution explorer and select *Publish...*.</span></span>

    ![Functions アプリで右クリック発行する](../media/8-right-click-publish.png)

1. <span data-ttu-id="221e8-112">**[発行先の選択]** ダイアログから *[Azure 関数アプリ]* を選択し、**[Azure App Service]** には *[新規作成]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="221e8-112">From the **Pick a publish target** dialog, select *Azure Function App*, and for **Azure App Service**, select *Create New*.</span></span> <span data-ttu-id="221e8-113">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="221e8-113">Click **Publish**.</span></span>

    ![発行先の Azure App Service を新しく作成する](../media/8-pick-publish-target.png)

1. <span data-ttu-id="221e8-115">これらの手順の **[リソース]** タブで、ユーザー名とパスワードを使用して Azure にサインインします。</span><span class="sxs-lookup"><span data-stu-id="221e8-115">Sign in to Azure using the username and password in the **Resources** tab of these instructions.</span></span> <span data-ttu-id="221e8-116">VM を使用するのでなくローカルでこのアプリを構築する場合は、ご利用の Azure アカウントを使用してログインし、ダイアログ上のリンクを使用する必要がある場合は、新しい Azure アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="221e8-116">If you are building this app locally instead of using the VM, log in with your Azure account, creating a new one if necessary using the links on the dialog.</span></span>

1. <span data-ttu-id="221e8-117">値はすべて既定値のままとします。そうすることで、ご自分の Functions アプリを実行するのに必要なインフラストラクチャ全体が作成されるからです。</span><span class="sxs-lookup"><span data-stu-id="221e8-117">Leave all the values as the defaults, as this will create all the necessary infrastructure to run your Functions app.</span></span>

1. <span data-ttu-id="221e8-118">**[作成]** をクリックし、Azure にすべてのリソースをプロビジョニングし、Azure Functions アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="221e8-118">Click **Create** to provision all the resources on Azure and publish your Azure Functions app.</span></span>

    ![App Service を作成する](../media/8-create-app-service.png)

1. <span data-ttu-id="221e8-120">Azure 上で Functions のバージョンを更新するかどうかを質問される場合があります。</span><span class="sxs-lookup"><span data-stu-id="221e8-120">You may be asked if you want to update the functions version on Azure.</span></span> <span data-ttu-id="221e8-121">次のダイアログ ボックスが表示されたら、**[はい]** を選択して、ご利用の関数アプリが最新の Azure Functions ランタイム バージョンで確実に発行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="221e8-121">If this dialog appears, select **Yes** to ensure your function app is published with the latest Azure Functions runtime version.</span></span>
    <span data-ttu-id="221e8-122">![Azure Functions の更新ダイアログ](../media/8-update-functions-on-azure.png)</span><span class="sxs-lookup"><span data-stu-id="221e8-122">![The update Azure Functions dialog](../media/8-update-functions-on-azure.png)</span></span>

<span data-ttu-id="221e8-123">プロビジョニングが完了するまで数分かかります。</span><span class="sxs-lookup"><span data-stu-id="221e8-123">Provisioning will take a couple of minutes to complete.</span></span> <span data-ttu-id="221e8-124">次のリソースがプロビジョニングされます。</span><span class="sxs-lookup"><span data-stu-id="221e8-124">The following resources will be provisioned:</span></span>

- <span data-ttu-id="221e8-125">Azure Functions アプリに必要なファイルを格納するためのストレージ アカウント</span><span class="sxs-lookup"><span data-stu-id="221e8-125">A storage account to store the files needed for the Azure Functions app</span></span>
- <span data-ttu-id="221e8-126">Azure Functions アプリに必要なコンピューティング リソースを管理する App Service プラン</span><span class="sxs-lookup"><span data-stu-id="221e8-126">An App Service plan to manage the compute resources needed by the Azure Functions app</span></span>
- <span data-ttu-id="221e8-127">Azure 関数を実行する App Service</span><span class="sxs-lookup"><span data-stu-id="221e8-127">The App Service that runs the Azure function</span></span>

<span data-ttu-id="221e8-128">関数が発行され、**https://\<your-app-name\>.azurewebsites.net/api/SendLocation** で呼び出せるようになります。</span><span class="sxs-lookup"><span data-stu-id="221e8-128">The function will now be published and available to call at **https://\<your-app-name\>.azurewebsites.net/api/SendLocation**.</span></span>

## <a name="configuring-your-app"></a><span data-ttu-id="221e8-129">アプリの構成</span><span class="sxs-lookup"><span data-stu-id="221e8-129">Configuring your app</span></span>

<span data-ttu-id="221e8-130">Azure 関数がローカルで実行されていたとき、`local.settings.json` ファイルに格納されていた Twilio 資格情報が使用されていました。</span><span class="sxs-lookup"><span data-stu-id="221e8-130">When the Azure function was running locally, it was using Twilio credentials that were stored in a `local.settings.json` file.</span></span> <span data-ttu-id="221e8-131">名前が示すように、このファイルは Azure 設定ではなく、ローカル設定用です。</span><span class="sxs-lookup"><span data-stu-id="221e8-131">As the name suggests, this file is for local settings, not Azure settings.</span></span> <span data-ttu-id="221e8-132">Azure 内で Azure 関数を呼び出すには、`TwilioAccountSid` 設定と `TwilioAuthToken` 設定を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="221e8-132">Before the Azure function can be called inside Azure, the `TwilioAccountSid` and `TwilioAuthToken` settings need to be configured.</span></span>

1. <span data-ttu-id="221e8-133">[発行] タブから **[アプリケーション設定の管理]** オプションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="221e8-133">From the Publish tab, click the **Manage Application Settings** option.</span></span>

    ![[アプリケーション設定の管理] オプション](../media/8-application-settings-option.png)

1. <span data-ttu-id="221e8-135">**[アプリケーション設定]** ダイアログには、ローカル値とリモート値を両方とも含むアプリケーション設定が表示されます。ローカル値はご利用の `local.settings.json` ファイルからのものです。リモート値は Azure でホストされている場合にご利用の関数で使用される値です。</span><span class="sxs-lookup"><span data-stu-id="221e8-135">The **Application Settings** dialog will show application settings with both a local and remote value - the local coming from your `local.settings.json` file, and the remote value is the one your function will use when it is hosted in Azure.</span></span> <span data-ttu-id="221e8-136">**[TwilioAccountSid]** 値および **[TwilioAuthToken]** 値について、*[ローカル]* ボックスから *[リモート]* ボックスに値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="221e8-136">Copy the values from the *Local* to the *Remote* boxes for the **TwilioAccountSid** and **TwilioAuthToken** values.</span></span>

    ![アプリケーション設定で Twilio 資格情報を設定する](../media/8-set-creds-in-app-settings.png)

1. <span data-ttu-id="221e8-138">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="221e8-138">Click **OK**.</span></span> <span data-ttu-id="221e8-139">これにより値が Azure Functions アプリに発行されます。</span><span class="sxs-lookup"><span data-stu-id="221e8-139">This will publish the values to the Azure functions app.</span></span>

## <a name="pointing-the-mobile-app-to-azure"></a><span data-ttu-id="221e8-140">モバイル アプリを Azure にポイントする</span><span class="sxs-lookup"><span data-stu-id="221e8-140">Pointing the mobile app to Azure</span></span>

1. <span data-ttu-id="221e8-141">[発行] タブから、値の横にある **[クリップボードにコピー]** ボタンを使用して **[サイト URL]** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="221e8-141">From the Publish tab, copy the **Site URL** using the **Copy to clipboard** button next to the value.</span></span>

    ![[発行] タブからサイトの URL をコピーする](../media/8-copy-site-url.png)

1. <span data-ttu-id="221e8-143">`ImHere` プロジェクトから `MainViewModel` を開く</span><span class="sxs-lookup"><span data-stu-id="221e8-143">Open the `MainViewModel` from the `ImHere` project.</span></span>

1. <span data-ttu-id="221e8-144">`baseUrl` フィールドの値を、[発行] タブからコピーしたサイト URL に変更します。</span><span class="sxs-lookup"><span data-stu-id="221e8-144">Update the value of the `baseUrl` field to be the site URL copied from the Publish tab.</span></span>

## <a name="test-it-out"></a><span data-ttu-id="221e8-145">テストする</span><span class="sxs-lookup"><span data-stu-id="221e8-145">Test it out</span></span>

1. <span data-ttu-id="221e8-146">`ImHere.UWP` アプリをスタートアップ アプリとして設定し、実行します。</span><span class="sxs-lookup"><span data-stu-id="221e8-146">Set the `ImHere.UWP` app as the startup app and run it.</span></span>

1. <span data-ttu-id="221e8-147">電話番号を入力し、**[場所の送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="221e8-147">Enter a phone number and click the **Send Location** button.</span></span>

1. <span data-ttu-id="221e8-148">場所は SMS メッセージとして受信してください。</span><span class="sxs-lookup"><span data-stu-id="221e8-148">You should receive the location as an SMS message.</span></span>

1. <span data-ttu-id="221e8-149">"*サービス利用不可*" エラーが返される場合は、ご利用の Functions アプリで使用されている "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージのバージョンを確認してください。それは 3.0.0 ではなく 3.0.0-rc1 である必要があります。</span><span class="sxs-lookup"><span data-stu-id="221e8-149">If you get back a *Service Unavailable* error, check what version of the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package your functions app is using, it should be 3.0.0-rc1, NOT 3.0.0.</span></span>
1. <span data-ttu-id="221e8-150">"*サービス利用不可*" エラーが返される場合は、ご利用の Functions アプリで使用されている "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージのバージョンを確認してください。それは 3.0.0 **ではなく** 3.0.0-rc1 である必要があります。</span><span class="sxs-lookup"><span data-stu-id="221e8-150">If you get back a *Service Unavailable* error, check what version of the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package your functions app is using, it should be 3.0.0-rc1, **NOT** 3.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="221e8-151">まとめ</span><span class="sxs-lookup"><span data-stu-id="221e8-151">Summary</span></span>

<span data-ttu-id="221e8-152">この演習では、Azure Functions プロジェクトを Visual Studio 内から Azure に発行し、アプリケーション設定を構成する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="221e8-152">In this unit, you learned how to publish an Azure Functions project to Azure from inside Visual Studio and configure application settings.</span></span>