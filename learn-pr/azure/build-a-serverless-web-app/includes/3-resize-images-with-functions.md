<span data-ttu-id="79bb7-101">前のユニットでは、サーバーレス関数を使用して、Web アプリケーションから Blob Storage に画像をアップロードするときにセキュリティで保護する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="79bb7-101">In the previous unit, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="79bb7-102">このモジュールでは、アップロードされた画像を監視し、そこからサムネイルを作成するために、別のサーバーレス関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-102">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="79bb7-103">サムネイル用の Blob Storage コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="79bb7-103">Create a Blob storage container for thumbnails</span></span>

<span data-ttu-id="79bb7-104">フル サイズの画像は、**images** という名前のコンテナーに格納されます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-104">The full-size images are stored in a container named **images**.</span></span> <span data-ttu-id="79bb7-105">これらの画像のサムネイルを格納するには、別のコンテナーが必要になります。</span><span class="sxs-lookup"><span data-stu-id="79bb7-105">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="79bb7-106">Cloud Shell (Bash) に引き続きサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-106">Ensure you're still signed in to Cloud Shell (Bash).</span></span> <span data-ttu-id="79bb7-107">そうでない場合は、**[Enter focus mode]\(フォーカス モードにする\)** を選択して、Cloud Shell ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-107">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="79bb7-108">すべての BLOB へのパブリック アクセスを持つ Storage アカウント内に、**thumbnails** という名前の新しいコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-108">Create a new container named **thumbnails** in your Storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="79bb7-109">BLOB によってトリガーされるサーバーレス関数を作成する</span><span class="sxs-lookup"><span data-stu-id="79bb7-109">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="79bb7-110">トリガーでは関数を呼び出す方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-110">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="79bb7-111">次に作成する関数では BLOB トリガーを使用します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-111">The function you create next uses a blob trigger.</span></span> <span data-ttu-id="79bb7-112">関数は、BLOB (画像ファイル) が **images** コンテナーにアップロードされるときに自動的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-112">The function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="79bb7-113">1 つの関数には 1 つのトリガーしか含められません。</span><span class="sxs-lookup"><span data-stu-id="79bb7-113">A function must have one trigger.</span></span> <span data-ttu-id="79bb7-114">トリガーにはデータが関連付けられていて、通常そのデータは、その関数をトリガーしたペイロードです。</span><span class="sxs-lookup"><span data-stu-id="79bb7-114">Triggers have associated data, which are usually the payload that triggered the function.</span></span>

<span data-ttu-id="79bb7-115">バインディングでは、関数が Azure またはサード パーティのサービスでデータを読み書きする方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-115">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="79bb7-116">この関数では、関数をトリガーする画像のサムネイル バージョンを作成し、そのサムネイルを *thumbnails* コンテナーに保存します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-116">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="79bb7-117">[Azure portal](https://portal.azure.com/?azure-portal=true) でお使いの Functions アプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-117">Open your Functions app in the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="79bb7-118">Functions アプリ ウィンドウの左側のナビゲーションで、**[関数]** をポイントし、プラス記号 (+) をクリックして新しいサーバーレス関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-118">In the Functions app window's left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span> <span data-ttu-id="79bb7-119">クイック スタート ページが表示されたら、**[カスタム関数]** をクリックして、関数テンプレートの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-119">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="79bb7-120">**BlobTrigger** テンプレートを検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-120">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="79bb7-121">以下の値を使用して、画像がアップロードされるときにサムネイルを作成する関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-121">Use these values to create a function that creates thumbnails as images are uploaded:</span></span>

    | <span data-ttu-id="79bb7-122">設定</span><span class="sxs-lookup"><span data-stu-id="79bb7-122">Setting</span></span>      |  <span data-ttu-id="79bb7-123">推奨値</span><span class="sxs-lookup"><span data-stu-id="79bb7-123">Suggested value</span></span>   | <span data-ttu-id="79bb7-124">説明</span><span class="sxs-lookup"><span data-stu-id="79bb7-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="79bb7-125">**言語**</span><span class="sxs-lookup"><span data-stu-id="79bb7-125">**Language**</span></span> | <span data-ttu-id="79bb7-126">C# または JavaScript</span><span class="sxs-lookup"><span data-stu-id="79bb7-126">C# or JavaScript</span></span> | <span data-ttu-id="79bb7-127">優先する言語を選択します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-127">Choose your preferred language.</span></span> |
    | <span data-ttu-id="79bb7-128">**関数名の指定**</span><span class="sxs-lookup"><span data-stu-id="79bb7-128">**Name your function**</span></span> | <span data-ttu-id="79bb7-129">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="79bb7-129">ResizeImage</span></span> | <span data-ttu-id="79bb7-130">アプリケーションから関数を検出できるように、この名前を、表示されているとおりに入力します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-130">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="79bb7-131">**パス**</span><span class="sxs-lookup"><span data-stu-id="79bb7-131">**Path**</span></span> | <span data-ttu-id="79bb7-132">images/{name}</span><span class="sxs-lookup"><span data-stu-id="79bb7-132">images/{name}</span></span> | <span data-ttu-id="79bb7-133">**images** コンテナーにファイルが示されたときに関数を実行します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-133">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="79bb7-134">**ストレージ アカウント情報**</span><span class="sxs-lookup"><span data-stu-id="79bb7-134">**Storage account information**</span></span> | <span data-ttu-id="79bb7-135">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="79bb7-135">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="79bb7-136">接続文字列を使用して前に作成した環境変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-136">Use the environment variable previously created with the connection string.</span></span> |

    ![新しい関数用の設定を入力する](../media/3-new-function.png)

1. <span data-ttu-id="79bb7-138">**[作成]** をクリックして、関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-138">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="79bb7-139">関数が作成されたら、**[統合]** をクリックして、そのトリガー、入力、および出力バインディングを表示します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-139">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="79bb7-140">**[新しい出力]** をクリックし、新しい出力バインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-140">Click **New output** to create a new output binding.</span></span>

    ![[統合] タブで [新しい出力] を選択する](../media/3-new-output.jpg)

1. <span data-ttu-id="79bb7-142">**[Azure Blob Storage]** を選び、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="79bb7-142">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="79bb7-143">**[選択]** ボタンを見つけるために、下へスクロールする必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="79bb7-143">You may have to scroll down to see the **Select** button.</span></span>

    ![[Azure Blob Storage] を選択する](../media/3-storage-output.jpg)

1. <span data-ttu-id="79bb7-145">次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-145">Enter the following values:</span></span>

    | <span data-ttu-id="79bb7-146">設定</span><span class="sxs-lookup"><span data-stu-id="79bb7-146">Setting</span></span>      |  <span data-ttu-id="79bb7-147">推奨値</span><span class="sxs-lookup"><span data-stu-id="79bb7-147">Suggested value</span></span>   | <span data-ttu-id="79bb7-148">説明</span><span class="sxs-lookup"><span data-stu-id="79bb7-148">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="79bb7-149">**BLOB パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="79bb7-149">**Blob parameter name**</span></span> | <span data-ttu-id="79bb7-150">thumbnail</span><span class="sxs-lookup"><span data-stu-id="79bb7-150">thumbnail</span></span> | <span data-ttu-id="79bb7-151">この関数では、この名前のパラメーターを使用してサムネイルを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-151">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="79bb7-152">**関数の戻り値を使用する**</span><span class="sxs-lookup"><span data-stu-id="79bb7-152">**Use function return value**</span></span> | <span data-ttu-id="79bb7-153">No</span><span class="sxs-lookup"><span data-stu-id="79bb7-153">No</span></span> |  |
    | <span data-ttu-id="79bb7-154">**パス**</span><span class="sxs-lookup"><span data-stu-id="79bb7-154">**Path**</span></span> | <span data-ttu-id="79bb7-155">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="79bb7-155">thumbnails/{name}</span></span> | <span data-ttu-id="79bb7-156">サムネイルは、**thumbnails** という名前のコンテナーに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-156">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="79bb7-157">**ストレージ アカウント接続**</span><span class="sxs-lookup"><span data-stu-id="79bb7-157">**Storage account connection**</span></span> | <span data-ttu-id="79bb7-158">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="79bb7-158">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="79bb7-159">接続文字列を使用して前に作成した環境変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-159">Use the environment variable previously created with the connection string.</span></span> |

    ![BLOB 出力バインディングの設定を入力する](../media/3-blob-output.png)

<span data-ttu-id="79bb7-161">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="79bb7-161">::: zone pivot="javascript"</span></span>
1. <span data-ttu-id="79bb7-162">(JavaScript) ウィンドウの右上隅にある **[詳細エディター]** をクリックして、バインディングを表す JSON を表示します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-162">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON that represents the bindings.</span></span>

1. <span data-ttu-id="79bb7-163">(JavaScript) `blobTrigger` バインディングで、`binary` の値を持つ `dataType` という名前のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-163">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="79bb7-164">これによって、BLOB コンテンツをバイナリ データとして関数に渡すように、バインディングが構成されます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-164">This configures the binding to pass the blob contents to the function as binary data.</span></span>

```json
{
    "name": "myBlob",
    "type": "blobTrigger",
    "direction": "in",
    "path": "images/{name}",
    "connection": "AZURE_STORAGE_CONNECTION_STRING",
    "dataType": "binary"
}
```

<span data-ttu-id="79bb7-165">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="79bb7-165">::: zone-end</span></span>

1. <span data-ttu-id="79bb7-166">**[保存]** をクリックして新しいバインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-166">Click **Save** to create the new binding.</span></span>

<span data-ttu-id="79bb7-167">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="79bb7-167">::: zone pivot="csharp"</span></span>
1. <span data-ttu-id="79bb7-168">(C#) 左側のナビゲーションで、関数名 **ResizeImage** を選択して、この関数のソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-168">(C#) Select the **ResizeImage** function name in the left navigation to open the function's source code.</span></span>

1. <span data-ttu-id="79bb7-169">(C#) この関数でサムネイルを生成するには、**ImageResizer** という NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="79bb7-169">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="79bb7-170">NuGet パッケージは、**project.json** ファイルを使用して C# 関数に追加します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="79bb7-171">このファイルを作成するには、右にある **[ファイルの表示]** をクリックして、関数を構成するファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>

1. <span data-ttu-id="79bb7-172">(C#) **[追加]** をクリックして、**project.json** という名前の新しいファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-172">(C#) Click **Add** to add a new file named **project.json**.</span></span>

1. <span data-ttu-id="79bb7-173">(C#) [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) ファイルの内容を、新しく作成したファイルにコピーします。</span><span class="sxs-lookup"><span data-stu-id="79bb7-173">(C#) Copy the contents of the [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) file into the newly created file.</span></span> <span data-ttu-id="79bb7-174">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-174">Save the file.</span></span> <span data-ttu-id="79bb7-175">ファイルが更新されると、パッケージが自動的に復元されます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-175">Packages are automatically restored when the file is updated.</span></span>

    ![project.json ファイルと ImageResizer](../media/3-project-json.png)

1. <span data-ttu-id="79bb7-177">(C#) **[ファイルの表示]** で、**[run.csx]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-177">(C#) Under **View Files**, select **run.csx**.</span></span> <span data-ttu-id="79bb7-178">その内容を [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-178">Replace its content with the content in the [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx) file.</span></span>

<span data-ttu-id="79bb7-179">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="79bb7-179">::: zone-end</span></span>

<span data-ttu-id="79bb7-180">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="79bb7-180">::: zone pivot="javascript"</span></span>
1. <span data-ttu-id="79bb7-181">(JavaScript) この関数で写真のサイズを変更するには、npm の `jimp` パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="79bb7-181">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="79bb7-182">npm パッケージをインストールするには、左側のナビゲーションで Functions アプリの名前をクリックし、**[プラットフォーム機能]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="79bb7-182">To install the npm package, click on the Functions app name on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="79bb7-183">(JavaScript) **[コンソール]** をクリックしてコンソール ウィンドウを表示します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-183">(JavaScript) Click **Console** to reveal a console window.</span></span>

1. <span data-ttu-id="79bb7-184">(JavaScript) コンソールで `npm install jimp` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-184">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="79bb7-185">操作が完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="79bb7-185">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="79bb7-186">(JavaScript) 左側のナビゲーションで関数名 **ResizeImage** をクリックして、関数を表示します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-186">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function.</span></span> <span data-ttu-id="79bb7-187">**index.js** ファイルのすべての内容を、[**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-187">Replace all the content in the **index.js** file with the content of the [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js) file.</span></span>

<span data-ttu-id="79bb7-188">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="79bb7-188">::: zone-end</span></span>

1. <span data-ttu-id="79bb7-189">ログ パネルを展開するには、コード ウィンドウの下の **[ログ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="79bb7-189">To expand the logs panel, click **Logs** below the code window.</span></span>

1. <span data-ttu-id="79bb7-190">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="79bb7-190">Click **Save**.</span></span> <span data-ttu-id="79bb7-191">関数が正常に保存され、エラーがないことをログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-191">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="79bb7-192">サーバーレス関数をテストする</span><span class="sxs-lookup"><span data-stu-id="79bb7-192">Test the serverless function</span></span>

1. <span data-ttu-id="79bb7-193">ブラウザーでアプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="79bb7-193">Open the application in a browser.</span></span> <span data-ttu-id="79bb7-194">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="79bb7-194">Select an image file and upload it.</span></span> <span data-ttu-id="79bb7-195">アップロードが完了しますが、画像を表示する機能はまだ追加していないため、アップロードした写真はアプリに表示されません。</span><span class="sxs-lookup"><span data-stu-id="79bb7-195">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="79bb7-196">Cloud Shell で、**images** コンテナーに画像がアップロードされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-196">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="79bb7-197">**thumbnails** という名前のコンテナー内に、サムネイルが作成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-197">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="79bb7-198">サムネイル用の URL を取得します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-198">Get the URL for the thumbnail.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="79bb7-199">ブラウザーで URL を開き、サムネイルが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-199">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="79bb7-200">次のチュートリアルに進む前に、**images** と **thumbnails** の各コンテナー内のすべてのファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-200">Before continuing to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```

    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```

## <a name="summary"></a><span data-ttu-id="79bb7-201">まとめ</span><span class="sxs-lookup"><span data-stu-id="79bb7-201">Summary</span></span>

<span data-ttu-id="79bb7-202">このユニットでは、画像が Blob Storage コンテナーにアップロードされるときにサムネイルを作成するサーバーレス関数を作成しました。</span><span class="sxs-lookup"><span data-stu-id="79bb7-202">In this unit, you created a serverless function to create a thumbnail when an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="79bb7-203">次は、Azure Cosmos DB を使用して、画像のメタデータの格納および一覧表示を行う方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="79bb7-203">Next, you will learn how to use Azure Cosmos DB to store and list image metadata.</span></span>