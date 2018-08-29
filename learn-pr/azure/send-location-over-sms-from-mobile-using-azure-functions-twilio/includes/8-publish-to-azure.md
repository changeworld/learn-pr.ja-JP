<span data-ttu-id="c1a8c-101">アプリと Azure 関数がこれで完全になり、ローカルで実行されています。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-101">The app and Azure function are now complete and running locally.</span></span> <span data-ttu-id="c1a8c-102">この演習では、Azure 関数を Azure に発行し、クラウドで実行します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-102">In this unit, you publish the Azure function to Azure to run in the cloud.</span></span>

> <span data-ttu-id="c1a8c-103">この演習では、Visual Studio から関数を発行します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-103">In this unit, you will publish your function from Visual Studio.</span></span> <span data-ttu-id="c1a8c-104">これは概念実証、プロトタイプ、学習を始める方法として優れていますが、運用品質のアプリにはこの手法を利用**しないで**ください。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-104">This is a great way to get started for proof-of-concepts, prototypes, and learning, but for a production-quality app you should **not** use this method.</span></span> <span data-ttu-id="c1a8c-105">何らかの形式の CI ベースのデプロイを利用してください。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-105">You should use some form of CI-based deployment.</span></span> <span data-ttu-id="c1a8c-106">この方法に関する詳細は、[Azure Functions のデプロイに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-106">You can read more about doing this in the [Azure Functions Deployment docs](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment).</span></span>
>
> <span data-ttu-id="c1a8c-107">[![真の友人は右クリック発行させない](../media/8-friends-dont-let-friends-publish.png)](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/)</span><span class="sxs-lookup"><span data-stu-id="c1a8c-107">[![Friends don't let friends right-click publish](../media/8-friends-dont-let-friends-publish.png)](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/)</span></span>

## <a name="publishing-your-app-to-azure"></a><span data-ttu-id="c1a8c-108">Azure にアプリを発行する</span><span class="sxs-lookup"><span data-stu-id="c1a8c-108">Publishing your app to Azure</span></span>

<span data-ttu-id="c1a8c-109">Azure 関数は Visual Studio 内から Azure に発行できます。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-109">Azure functions can be published to Azure from inside Visual Studio.</span></span>

1. <span data-ttu-id="c1a8c-110">ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

2. <span data-ttu-id="c1a8c-111">ソリューション エクスプローラーで `ImHere.Functions` アプリを右クリックし、*[発行]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-111">Right-click on the `ImHere.Functions` app in the solution explorer and select *Publish...*.</span></span>

    ![Functions アプリで右クリック発行する](../media/8-right-click-publish.png)

3. <span data-ttu-id="c1a8c-113">**[発行先の選択]** ダイアログから *[Azure 関数アプリ]* を選択し、**[Azure App Service]** には *[新規作成]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-113">From the **Pick a publish target** dialog, select *Azure Function App*, and for **Azure App Service**, select *Create New*.</span></span> <span data-ttu-id="c1a8c-114">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-114">Click **Publish**.</span></span>

    ![発行先の Azure App Service を新しく作成する](../media/8-pick-publish-target.png)

4. <span data-ttu-id="c1a8c-116">Azure アカウントを複数お持ちのとき、必要なアカウントが選択されていない場合、右上隅のドロップダウンから Azure アカウントを選択します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-116">Select your Azure account from the drop-down in the top-right corner if you have more than one Azure accounts and the right one isn't selected.</span></span>

5. <span data-ttu-id="c1a8c-117">Function App に名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-117">Give your Functions app a name.</span></span> <span data-ttu-id="c1a8c-118">この名前は、Azure 全体のすべての Functions アプリで一意にする必要があります。"ImHere-\<YourName\>" のような名前を使用してください。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-118">This name needs to be globally unique across all the Functions apps in the whole of Azure, so use something like "ImHere-\<YourName\>".</span></span>

6. <span data-ttu-id="c1a8c-119">この Functions アプリを作成するサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-119">Select the subscription you want to create this Functions app under.</span></span>

7. <span data-ttu-id="c1a8c-120">**[リソース グループ]** ドロップダウンの横にある **[新規]** ボタンをクリックし、"ImHere" のような名前を付けることで、この Functions アプリに新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-120">Create a new resource group for this Functions app by clicking the **New...** button next to the **Resource Group** drop-down and giving it a name such as "ImHere".</span></span> <span data-ttu-id="c1a8c-121">リソース グループ名は、Azure 全体で一意にするのではなく、サブスクリプションに対して一意にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-121">Resource group names need to be unique to your subscription, not globally unique across Azure.</span></span> <span data-ttu-id="c1a8c-122">次に、 **[OK]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="c1a8c-122">Then, click **OK**.</span></span>

    ![新しいリソース グループを作成する](../media/8-create-new-resource-group.png)

   <span data-ttu-id="c1a8c-124">新しいリソース グループを作成すると、後のクリーンアップが簡単になります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-124">Creating a new resource group makes it easier to clean up later.</span></span> <span data-ttu-id="c1a8c-125">リソース グループを削除すると、この Functions アプリに作成したすべてが同時に削除されます。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-125">You can delete the resource group and know that everything you've created for this Functions app will all be deleted at the same time.</span></span>

8. <span data-ttu-id="c1a8c-126">**[ホスティング プラン]** ドロップダウンの横にある **[新規]** ボタンをクリックし、新しいホスティング プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-126">Create a new hosting plan by clicking the **New...** button next to the **Hosting Plan** drop-down.</span></span> <span data-ttu-id="c1a8c-127">App Service プランの名前は、既定でアプリ名の末尾に "Plan" が付く形式になります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-127">The App Service plan name will default to your app name with "Plan" on the end.</span></span> <span data-ttu-id="c1a8c-128">**[場所]** をお住まいの地域に最も近い場所に設定し、**[サイズ]** を消費に設定します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-128">Set the **Location** to the closest location to you and make sure **Size** is set to consumption.</span></span> <span data-ttu-id="c1a8c-129">次に、 **[OK]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="c1a8c-129">Then, click **OK**.</span></span>

    ![ホスティング プランを構成する](../media/8-configure-hosting-plan.png)

9. <span data-ttu-id="c1a8c-131">**[ストレージ アカウント]** ドロップダウンの横にある **[新規]** ボタンをクリックし、新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-131">Create a new storage account by clicking the **New...** button next to the **Storage Account** drop-down.</span></span> <span data-ttu-id="c1a8c-132">既定の名前が与えられます。既定値をすべてそのまま選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-132">A default name will be provided, so keep all the default values and click **OK**.</span></span>

    ![ストレージ アカウントの作成](../media/8-create-storage-account.png)

10. <span data-ttu-id="c1a8c-134">**[作成]** をクリックし、Azure にすべてのリソースをプロビジョニングし、Azure Functions アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-134">Click **Create** to provision all the resources on Azure and publish your Azure Functions app.</span></span>

    ![App Service を作成する](../media/8-create-app-service.png)

<span data-ttu-id="c1a8c-136">プロビジョニングの実行には 2、3 分かかります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-136">Provisioning will take a couple of minutes or so to run.</span></span> <span data-ttu-id="c1a8c-137">次のリソースがプロビジョニングされます。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-137">The following resources will be provisioned:</span></span>

* <span data-ttu-id="c1a8c-138">Azure Functions アプリに必要なファイルを格納するためのストレージ アカウント</span><span class="sxs-lookup"><span data-stu-id="c1a8c-138">A storage account to store the files needed for the Azure Functions app</span></span>
* <span data-ttu-id="c1a8c-139">Azure Functions アプリに必要なコンピューティング リソースを管理する App Service プラン</span><span class="sxs-lookup"><span data-stu-id="c1a8c-139">An App Service plan to manage the compute resources needed by the Azure Functions app</span></span>
* <span data-ttu-id="c1a8c-140">Azure 関数を実行する App Service</span><span class="sxs-lookup"><span data-stu-id="c1a8c-140">The App Service that runs the Azure function</span></span>

<span data-ttu-id="c1a8c-141">関数が発行され、 https://<your-app-name>.azurewebsites.net/api/SendLocation で呼び出せるようになります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-141">The function will now be published and available to call at https://<your-app-name>.azurewebsites.net/api/SendLocation.</span></span>

## <a name="configuring-your-app"></a><span data-ttu-id="c1a8c-142">アプリの構成</span><span class="sxs-lookup"><span data-stu-id="c1a8c-142">Configuring your app</span></span>

<span data-ttu-id="c1a8c-143">Azure 関数がローカルで実行されていたとき、`local.settings.json` ファイルに格納されていた Twilio 資格情報が使用されていました。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-143">When the Azure function was running locally, it was using Twilio credentials that were stored in a `local.settings.json` file.</span></span> <span data-ttu-id="c1a8c-144">名前が示すように、このファイルは Azure 設定ではなく、ローカル設定用です。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-144">As the name suggests, this file is for local settings, not Azure settings.</span></span> <span data-ttu-id="c1a8c-145">Azure 内で Azure 関数を呼び出すには、`TwilioAccountSid` 設定と `TwilioAuthToken` 設定を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-145">Before the Azure function can be called inside Azure, the `TwilioAccountSid` and `TwilioAuthToken` settings need to be configured.</span></span>

1. <span data-ttu-id="c1a8c-146">[発行] タブから **[アプリケーション設定の管理]** オプションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-146">From the Publish tab, click the **Manage Application Settings** option.</span></span>

    ![[アプリケーション設定の管理] オプション](../media/8-application-settings-option.png)

2. <span data-ttu-id="c1a8c-148">**[追加]** ボタンをクリックして、新しい設定を追加します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-148">Click the **Add** button to add a new setting.</span></span> <span data-ttu-id="c1a8c-149">これに "TwilioAccountSid" という名前を付け、値を Twilio アカウント SID に設定します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-149">Name it "TwilioAccountSid" and set the value to your Twilio account SID.</span></span> <span data-ttu-id="c1a8c-150">"TwilioAuthToken" という名前を使用し、認証トークンに対してこの手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-150">Repeat this step for your Auth Token using the name "TwilioAuthToken".</span></span>

    ![アプリケーション設定で Twilio 資格情報を設定する](../media/8-set-creds-in-app-settings.png)

3. <span data-ttu-id="c1a8c-152">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1a8c-152">Click **OK**.</span></span>

4. <span data-ttu-id="c1a8c-153">**[発行]** をクリックし、新しいアプリケーション設定で Azure Functions アプリを再発行します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-153">Click **Publish** to republish the Azure Functions app with the new application settings.</span></span>

    ![[発行] ボタン](../media/8-publish-application-button.png)

## <a name="pointing-the-mobile-app-to-azure"></a><span data-ttu-id="c1a8c-155">モバイル アプリを Azure にポイントする</span><span class="sxs-lookup"><span data-stu-id="c1a8c-155">Pointing the mobile app to Azure</span></span>

1. <span data-ttu-id="c1a8c-156">[発行] タブから、値の横にあるコピー ボタンを使用して **[サイト URL]** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-156">From the Publish tab, copy the **Site URL** using the copy button next to the value.</span></span>

    ![[発行] タブからサイトの URL をコピーする](../media/8-copy-site-url.png)

2. <span data-ttu-id="c1a8c-158">`ImHere` プロジェクトから `MainViewModel` を開く</span><span class="sxs-lookup"><span data-stu-id="c1a8c-158">Open the `MainViewModel` from the `ImHere` project.</span></span>

3. <span data-ttu-id="c1a8c-159">[発行] タブからコピーしたサイト URL に `baseUrl` フィールドの値を変更します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-159">Update the value of the `baseUrl` field to be the site URL copied from the Publish tab.</span></span>

4. <span data-ttu-id="c1a8c-160">この値のプロトコルを `http` から `https` に変更します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-160">Change the protocol for this value from `http` to `https`.</span></span> <span data-ttu-id="c1a8c-161">サイト URL は常に HTTP の使用で与えられますが、Azure 関数を呼び出すには HTTPS を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-161">The site URL is always given using HTTP, but you have to use HTTPS to call an Azure function.</span></span>

## <a name="test-it-out"></a><span data-ttu-id="c1a8c-162">テストする</span><span class="sxs-lookup"><span data-stu-id="c1a8c-162">Test it out</span></span>

1. <span data-ttu-id="c1a8c-163">`ImHere.UWP` アプリをスタートアップ アプリとして設定し、実行します。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-163">Set the `ImHere.UWP` app as the startup app and run it.</span></span>

2. <span data-ttu-id="c1a8c-164">電話番号を入力し、**[場所の送信]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-164">Enter a phone number and click the **Send Location** button.</span></span>

3. <span data-ttu-id="c1a8c-165">場所は SMS メッセージとして受信してください。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-165">You should receive the location as an SMS message.</span></span>

## <a name="summary"></a><span data-ttu-id="c1a8c-166">まとめ</span><span class="sxs-lookup"><span data-stu-id="c1a8c-166">Summary</span></span>

<span data-ttu-id="c1a8c-167">この演習では、Azure Functions プロジェクトを Visual Studio 内から Azure に発行し、アプリケーション設定を構成する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="c1a8c-167">In this unit, you learned how to publish an Azure Functions project to Azure from inside Visual Studio and configure application settings.</span></span>