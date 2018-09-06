---
zone_pivot_groups: dev-lang-csharp-javascript
ms.openlocfilehash: 69bc512c02a30bc74ae82a3a43a083af635d89ba
ms.sourcegitcommit: bf091e3a389138b59573865ca54775e38a4ffa1f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43154577"
---
<span data-ttu-id="3fdbe-101">あなたが作成しているアプリケーションは、フォト ギャラリーです。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-101">The application that you're building is a photo gallery.</span></span> <span data-ttu-id="3fdbe-102">このアプリケーションでは、クライアント側 JavaScript を使用して API を呼び出し、画像をアップロードおよび表示します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-102">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="3fdbe-103">このモジュールでは、時間制限付き URL を生成して画像をアップロードするサーバーレス関数を使用して、API を作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-103">In this module, you create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="3fdbe-104">この Web アプリケーションでは、[Blob Storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) を使って Blob Storage に画像をアップロードするために、生成済み URL が使用されます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-104">The web application uses the generated URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="3fdbe-105">画像用の Blob Storage コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="3fdbe-105">Create a Blob storage container for images</span></span>

<span data-ttu-id="3fdbe-106">アプリケーションには、画像をアップロードしてホストするために、別個のストレージ コンテナーが必要です。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-106">The application requires a separate storage container to upload and host images.</span></span>

1. <span data-ttu-id="3fdbe-107">Azure Cloud Shell (Bash) に引き続きサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-107">Ensure that you're still signed in to Azure Cloud Shell (Bash).</span></span> <span data-ttu-id="3fdbe-108">そうでない場合は、**[Enter focus mode]\(フォーカス モードにする\)** を選択して、Cloud Shell ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span>

1.  <span data-ttu-id="3fdbe-109">すべての BLOB へのパブリック アクセスを持つストレージ アカウント内に **images** という名前の新しいコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-109">Create a new container named **images** in your Storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-functions-app"></a><span data-ttu-id="3fdbe-110">Azure Functions アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="3fdbe-110">Create an Azure Functions app</span></span>

<span data-ttu-id="3fdbe-111">Azure Functions はサーバーレス関数を実行するためのサービスです。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-111">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="3fdbe-112">サーバーレス関数は、HTTP 要求などのイベントによって、または BLOB がストレージ コンテナーに作成されたときに、トリガーする (呼び出す) ことができます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-112">A serverless function can be triggered (called) by events, such as an HTTP request, or when a blob is created in a storage container.</span></span>

<span data-ttu-id="3fdbe-113">Azure Functions アプリは、1 つ以上のサーバーレス関数のためのコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-113">An Azure Functions app is a container for one or more serverless functions.</span></span>

- <span data-ttu-id="3fdbe-114">新しい Functions アプリを、先に作成した **first-serverless-app** リソース グループ内に作成し、一意の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-114">Create a new Functions app with a unique name in the **first-serverless-app** resource group that you created earlier.</span></span> <span data-ttu-id="3fdbe-115">Functions アプリには、ストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-115">Functions apps require a Storage account.</span></span> <span data-ttu-id="3fdbe-116">このチュートリアルでは、既存のストレージ アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-116">In this tutorial, you use the existing storage account.</span></span>

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```

## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="3fdbe-117">HTTP によってトリガーされるサーバーレス関数を作成する</span><span class="sxs-lookup"><span data-stu-id="3fdbe-117">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="3fdbe-118">画像を Blob Storage に安全にアップロードするため、フォト ギャラリー Web アプリでは、サーバーレス関数に HTTP 要求を行い、時間制限付き URL を生成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-118">To securely upload an image to Blob storage, the photo gallery web app makes an HTTP request to the serverless function to generate a time-limited URL.</span></span> <span data-ttu-id="3fdbe-119">関数は HTTP 要求によってトリガーされます。この関数では Azure Storage SDK が使用されます。これにより、セキュリティで保護された URL が生成され、返されます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-119">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="3fdbe-120">Functions アプリが作成されたら、**検索**ボックスを使用して Azure portal 内でそのアプリを検索します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-120">After the Functions app is created, search for it in the Azure portal using the **Search** box.</span></span> <span data-ttu-id="3fdbe-121">アプリをクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-121">Click on the app to open it.</span></span>

    ![Functions アプリを開く](../media/2-search-function-app.png)


1. <span data-ttu-id="3fdbe-123">左側のナビゲーションの Functions アプリ ウィンドウで、**[関数]** をポイントし、プラス記号 (+) をクリックして新しいサーバーレス関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-123">In the Functions app window, in the left navigation, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span>

    ![新しい関数を作成する](../media/2-new-function.png)

1. <span data-ttu-id="3fdbe-125">**[カスタム関数]** をクリックして、関数テンプレートの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-125">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="3fdbe-126">**HttpTrigger** テンプレートを見つけて、使用する言語 (C# または JavaScript) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-126">Find the **HttpTrigger** template and click the language to use (C# or JavaScript).</span></span>

1. <span data-ttu-id="3fdbe-127">次の値を使用して、BLOB のアップロード URL を生成する関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-127">Use the following values to create a function that generates a blob upload URL:</span></span>

    | <span data-ttu-id="3fdbe-128">設定</span><span class="sxs-lookup"><span data-stu-id="3fdbe-128">Setting</span></span>      |  <span data-ttu-id="3fdbe-129">推奨値</span><span class="sxs-lookup"><span data-stu-id="3fdbe-129">Suggested value</span></span>   | <span data-ttu-id="3fdbe-130">説明</span><span class="sxs-lookup"><span data-stu-id="3fdbe-130">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="3fdbe-131">**言語**</span><span class="sxs-lookup"><span data-stu-id="3fdbe-131">**Language**</span></span> | <span data-ttu-id="3fdbe-132">C# または JavaScript</span><span class="sxs-lookup"><span data-stu-id="3fdbe-132">C# or JavaScript</span></span> | <span data-ttu-id="3fdbe-133">使用する言語を選びます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-133">Select the language that you want to use.</span></span> |
    | <span data-ttu-id="3fdbe-134">**関数名の指定**</span><span class="sxs-lookup"><span data-stu-id="3fdbe-134">**Name your function**</span></span> | <span data-ttu-id="3fdbe-135">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="3fdbe-135">GetUploadUrl</span></span> | <span data-ttu-id="3fdbe-136">アプリケーションから関数を検出できるように、この名前を、表示されているとおりに入力します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-136">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="3fdbe-137">**承認レベル**</span><span class="sxs-lookup"><span data-stu-id="3fdbe-137">**Authorization level**</span></span> | <span data-ttu-id="3fdbe-138">Anonymous</span><span class="sxs-lookup"><span data-stu-id="3fdbe-138">Anonymous</span></span> | <span data-ttu-id="3fdbe-139">関数にパブリックにアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-139">Allow the function to be accessed publicly.</span></span> |

    ![HTTP によってトリガーされる新しい関数の設定を入力する](../media/2-new-function-httptrigger.png)

1. <span data-ttu-id="3fdbe-141">**[作成]** をクリックして、関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-141">Click **Create** to create the function.</span></span>

<span data-ttu-id="3fdbe-142">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="3fdbe-142">::: zone pivot="csharp"</span></span>
1. <span data-ttu-id="3fdbe-143">**C#**</span><span class="sxs-lookup"><span data-stu-id="3fdbe-143">**C#**</span></span> 

    <span data-ttu-id="3fdbe-144">関数のソース コードが表示されたら、**run.csx** ファイル内のすべての内容を [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-144">When the function's source code appears, replace all of the content in the **run.csx** file with the content in the [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) file.</span></span>

<span data-ttu-id="3fdbe-145">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="3fdbe-145">::: zone-end</span></span>

<span data-ttu-id="3fdbe-146">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="3fdbe-146">::: zone pivot="javascript"</span></span>
1. <span data-ttu-id="3fdbe-147">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="3fdbe-147">**JavaScript**</span></span> 

    1. <span data-ttu-id="3fdbe-148">(JavaScript) この関数には、npm の `azure-storage` パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-148">(JavaScript) This function requires the `azure-storage` package from npm.</span></span> <span data-ttu-id="3fdbe-149">このパッケージによって、セキュリティで保護された URL を構築するために必要な Shared Access Signature (SAS) トークンが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-149">The package generates the shared access signature (SAS) token that's required to build the secure URL.</span></span> <span data-ttu-id="3fdbe-150">npm パッケージをインストールするには、左側のナビゲーションで Functions アプリをクリックし、**[プラットフォーム機能]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-150">To install the npm package, click on the Functions app on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="3fdbe-151">(JavaScript) **[コンソール]** をクリックしてコンソール ウィンドウを表示します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-151">(JavaScript) Click **Console** to reveal a console window.</span></span>

        ![コンソール ウィンドウを開く](../media/2-open-console.jpg)

    1. <span data-ttu-id="3fdbe-153">(JavaScript) `cd d:\home\site\wwwroot` コマンドを実行して、現在のディレクトリが **d:\home\site\wwwroot** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-153">(JavaScript) Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

    1. <span data-ttu-id="3fdbe-154">(JavaScript) `npm init -y` コマンドを実行して、空の **package.json** ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-154">(JavaScript) Run the command `npm init -y` to create an empty **package.json** file.</span></span>

    1. <span data-ttu-id="3fdbe-155">(JavaScript) パッケージをインストールするには、コンソールでコマンド `npm install --save azure-storage` を実行します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-155">(JavaScript) To install the package, run the command `npm install --save azure-storage` in the console.</span></span> <span data-ttu-id="3fdbe-156">パッケージを **package.json** として保存します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-156">Save the package as **package.json**.</span></span> <span data-ttu-id="3fdbe-157">操作が完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-157">It may take a few minutes to complete the operation.</span></span>

    1. <span data-ttu-id="3fdbe-158">(JavaScript) 左側のナビゲーションで関数 (**GetUploadUrl**) をクリックして、関数を表示します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-158">(JavaScript) Click on the function (**GetUploadUrl**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="3fdbe-159">**index.js** ファイルのすべての内容を、[**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-159">Replace all of the content in the **index.js** file with the content in the [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) file.</span></span>
    
        ![更新後の index.js の内容](../media/2-paste-js.jpg)

<span data-ttu-id="3fdbe-161">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="3fdbe-161">::: zone-end</span></span>

1. <span data-ttu-id="3fdbe-162">コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-162">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="3fdbe-163">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-163">Click **Save**.</span></span> <span data-ttu-id="3fdbe-164">関数が正常にコンパイルされていることを、ログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-164">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="3fdbe-165">この関数により、Blob Storage にファイルをアップロードするために使用する Shared Access Signature (SAS) URL と呼ばれるものが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-165">The function generates what's called a shared access signature (SAS) URL that's used to upload a file to Blob storage.</span></span> <span data-ttu-id="3fdbe-166">SAS URL の有効期間は短く、この URL でアップロードが許可されるのは 1 つのファイルだけです。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-166">The SAS URL is valid for a short period of time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="3fdbe-167">[Shared Access Signature の使用方法](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)については、Blob Storage のドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-167">Consult the Blob storage documentation, for more information on [how to use shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="3fdbe-168">ストレージ接続文字列に環境変数を追加する</span><span class="sxs-lookup"><span data-stu-id="3fdbe-168">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="3fdbe-169">作成した関数で SAS URL を生成できるようにするには、ストレージ アカウントの接続文字列が必要です。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-169">The function that you created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="3fdbe-170">接続文字列は、関数本体でハードコーディングするのではなく、アプリケーション設定として格納できます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-170">Instead of hardcoding the connection string in the function body, it can be stored as an application setting.</span></span> <span data-ttu-id="3fdbe-171">アプリケーション設定には、Functions アプリ内のすべての関数が環境変数としてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-171">Application settings are accessible as environment variables by all functions in the Functions app.</span></span>

1. <span data-ttu-id="3fdbe-172">Cloud Shell で、ストレージ アカウント接続文字列のクエリを実行し、**STORAGE_CONNECTION_STRING** という名前の Bash 変数に保存します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-172">In Cloud Shell, query the Storage account connection string and save it to a Bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="3fdbe-173">変数が正常に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-173">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="3fdbe-174">前の手順で保存した値を使用して、**AZURE_STORAGE_CONNECTION_STRING** という名前の新しいアプリケーション設定を作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-174">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    <span data-ttu-id="3fdbe-175">コマンドの出力に、正しい値が指定された新しいアプリケーション設定が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-175">Confirm that the command's output contains the new application setting with the correct value.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="3fdbe-176">サーバーレス関数をテストする</span><span class="sxs-lookup"><span data-stu-id="3fdbe-176">Test the serverless function</span></span>

<span data-ttu-id="3fdbe-177">Azure portal には、関数の作成と編集だけでなく、関数をテストするツールも組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-177">In addition to creating and editing functions, the Azure portal also provides a built-in tool for testing functions.</span></span>

1. <span data-ttu-id="3fdbe-178">HTTP サーバーレス関数をテストするには、コード ウィンドウの右側にある **[テスト]** タブをクリックして、テスト パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-178">To test the HTTP serverless function, on the right of the code window, click on the **Test** tab to expand the test panel.</span></span>

1. <span data-ttu-id="3fdbe-179">**[HTTP メソッド]** を **[GET]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-179">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="3fdbe-180">**[クエリ]** の下の **[パラメーターの追加]** をクリックし、次のパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-180">Under **Query**, click **Add parameter** and add the following parameter:</span></span>

    | <span data-ttu-id="3fdbe-181">名前</span><span class="sxs-lookup"><span data-stu-id="3fdbe-181">Name</span></span>      |  <span data-ttu-id="3fdbe-182">値</span><span class="sxs-lookup"><span data-stu-id="3fdbe-182">Value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="3fdbe-183">**filename**</span><span class="sxs-lookup"><span data-stu-id="3fdbe-183">**filename**</span></span> | <span data-ttu-id="3fdbe-184">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="3fdbe-184">image1.jpg</span></span> |

1. <span data-ttu-id="3fdbe-185">テスト パネルで **[実行]** をクリックして、HTTP 要求を関数に送信します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-185">In the test panel, click **Run** to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="3fdbe-186">関数の出力にアップロード URL が返されます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-186">The function returns an upload URL in the output.</span></span> <span data-ttu-id="3fdbe-187">関数の実行は、ログ パネルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-187">The function execution appears in the Logs panel.</span></span>

    ![関数が正常に実行されたことを示すログ](../media/2-test-function.png)


## <a name="configure-cors-in-the-functions-app"></a><span data-ttu-id="3fdbe-189">Functions アプリで CORS を構成する</span><span class="sxs-lookup"><span data-stu-id="3fdbe-189">Configure CORS in the Functions app</span></span>

<span data-ttu-id="3fdbe-190">関数のフロントエンドは Blob Storage でホストされているため、そのドメイン名は、Azure Functions アプリとは異なります。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-190">Because the function front end is hosted in Blob storage, it has a different domain name than the Azure Functions app.</span></span> <span data-ttu-id="3fdbe-191">作成した関数がクライアント側の JavaScript によって正常に呼び出されるように、Functions アプリをクロス オリジン リソース共有 (CORS) 用に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-191">For the client-side JavaScript to successfully call the function that you created, the Functions app has to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="3fdbe-192">関数アプリ ウィンドウの左側のナビゲーション バーで、お使いの Functions アプリの名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-192">In the left navigation of the Functions app window, click on the name of your Functions app.</span></span>

1. <span data-ttu-id="3fdbe-193">**[プラットフォーム機能]** をクリックして、高度な機能の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-193">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="3fdbe-194">**[API]** で **[CORS]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-194">Under **API**, click **CORS**.</span></span>

    ![CORS を選択する](../media/2-open-cors.jpg)

1. <span data-ttu-id="3fdbe-196">前のモジュールのアプリケーション URL の許可されたオリジンを、末尾のスラッシュ (/) を省略して追加します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-196">Add an allow origin for the application URL from the previous module and omit the trailing slash (/).</span></span> <span data-ttu-id="3fdbe-197">例: `https://firstserverlessweb.z4.web.core.windows.net`。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-197">For example, `https://firstserverlessweb.z4.web.core.windows.net`.</span></span>

    ![追加されたサーバーレス Web アプリの URL を示す CORS 設定](../media/2-add-cors.png)

1. <span data-ttu-id="3fdbe-199">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-199">Click **Save**.</span></span>

1. <span data-ttu-id="3fdbe-200">**C#**:</span><span class="sxs-lookup"><span data-stu-id="3fdbe-200">**C#**:</span></span>

   1. <span data-ttu-id="3fdbe-201">(C#) `GetUploadUrl` 関数に戻り、**[統合]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-201">(C#) Navigate back to the `GetUploadUrl` function and select the **Integrate** tab.</span></span>

   1. <span data-ttu-id="3fdbe-202">(C#) **[選択した HTTP メソッド]** で、**[OPTIONS]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-202">(C#) Under **Selected HTTP methods**, select **OPTIONS**.</span></span>

      <span data-ttu-id="3fdbe-203">**[GET]**、**[POST]**、および **[OPTIONS]** がすべて選択されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-203">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="3fdbe-204">CORS では **OPTIONS** メソッドが使用されます。C# 関数の既定では、これは選択されていません。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-204">CORS uses the **OPTIONS** method, which isn't selected by default for C# functions.</span></span>  

   1. <span data-ttu-id="3fdbe-205">(C#) **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-205">(C#) Click **Save**.</span></span>

1. <span data-ttu-id="3fdbe-206">引き続き Azure portal で、Functions アプリに移動します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-206">Still in the Azure portal, navigate to the Functions app.</span></span> <span data-ttu-id="3fdbe-207">**[概要]** タブを選択します。**[再起動]** をクリックして、CORS への変更が有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-207">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="3fdbe-208">ストレージ アカウントで CORS を構成する</span><span class="sxs-lookup"><span data-stu-id="3fdbe-208">Configure CORS in the Storage account</span></span>

<span data-ttu-id="3fdbe-209">Functions アプリでは Blob Storage に対してクライアント側 JavaScript 呼び出しを実行して、ファイルをアップロードするため、ストレージ アカウントを CORS 用に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-209">Because the Functions app also makes client-side JavaScript calls to Blob storage to upload files, you have to configure the Storage account for CORS.</span></span>

- <span data-ttu-id="3fdbe-210">次のコマンドを実行して、すべてのオリジンが、ファイルをストレージ アカウントにアップロードできるようにします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-210">Run the following command to allow all origins to upload files to the Storage account:</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="3fdbe-211">Web アプリを変更して画像をアップロードする</span><span class="sxs-lookup"><span data-stu-id="3fdbe-211">Modify the web app to upload images</span></span>

<span data-ttu-id="3fdbe-212">Web アプリでは、**settings.js** という名前のファイルから設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-212">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="3fdbe-213">次の手順では、Cloud Shell を使用してファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-213">In the following steps, you create the file using Cloud Shell.</span></span> <span data-ttu-id="3fdbe-214">`window.apiBaseUrl` を Functions アプリの URL に設定し、`window.blobBaseUrl` を Azure Blob Storage エンドポイントの URL に設定します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-214">You set `window.apiBaseUrl` to the URL of the Functions app, and `window.blobBaseUrl` to the URL of the Azure Blob storage endpoint.</span></span>

1. <span data-ttu-id="3fdbe-215">Cloud Shell で、現在のディレクトリが **www/dist** フォルダーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-215">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="3fdbe-216">Functions アプリの URL のクエリを実行し、**FUNCTION_APP_URL** という名前の Bash 変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-216">Query the URL of the Functions app and store it in a Bash variable named **FUNCTION_APP_URL**.</span></span>

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    <span data-ttu-id="3fdbe-217">変数が正しく設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-217">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. <span data-ttu-id="3fdbe-218">Functions アプリに API 呼び出しのベース URI を設定するには、**settings.js** ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-218">To set the base URI of API calls to your Functions app, create the **settings.js** file.</span></span> <span data-ttu-id="3fdbe-219">次の例のように、Functions アプリの URL を追加します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-219">Add the URL of the Functions app like the following example:</span></span>

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    <span data-ttu-id="3fdbe-220">変更を行うには、次のコマンドを実行するか、VIM などのコマンド ライン エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-220">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    <span data-ttu-id="3fdbe-221">ファイルが正常に書き込まれたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-221">Confirm the file was successfully written.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="3fdbe-222">Blob Storage のベース URL のクエリを実行し、**BLOB_BASE_URL** という名前の Bash 変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-222">Query the base URL for the Blob storage and store it in a Bash variable named **BLOB_BASE_URL**.</span></span>

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    <span data-ttu-id="3fdbe-223">変数が正しく設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-223">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. <span data-ttu-id="3fdbe-224">API 呼び出しのベース URI をお使いの Functions アプリに設定するには、次の例のように、Blob Storage URL を **settings.js** ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-224">To set the base URI of API calls to your Functions app, append the Blob storage URL to the **settings.js** file like the following example:</span></span>

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    <span data-ttu-id="3fdbe-225">変更を行うには、次のコマンドを実行するか、VIM などのコマンド ライン エディターを使用します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-225">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    <span data-ttu-id="3fdbe-226">ファイルが正常に書き込まれたこと、および 2 行が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-226">Confirm the file was successfully written and now contains two lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="3fdbe-227">ファイルを Blob Storage にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-227">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a><span data-ttu-id="3fdbe-228">Web アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="3fdbe-228">Test the web application</span></span>

<span data-ttu-id="3fdbe-229">この時点で、ギャラリー アプリケーションでは Blob Storage に画像をアップロードできますが、まだ画像は表示できません。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-229">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="3fdbe-230">`GetImages` 関数の呼び出しが試行されますが、この関数はまだ存在しません。これは後のモジュールで作成します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-230">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="3fdbe-231">この呼び出しは失敗し、Web ページには "分析中…" と表示されたままになりますが、選択する画像は正常にアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-231">The call will fail and the web page will appear to be stuck on "Analyzing...," but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="3fdbe-232">画像が正常にアップロードされていることを確認するには、Azure portal で **images** コンテナーの内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-232">You can verify that an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="3fdbe-233">ブラウザー ウィンドウで、アプリケーションを参照します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-233">In a browser window, browse to the application.</span></span> <span data-ttu-id="3fdbe-234">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-234">Select an image file and upload it.</span></span> <span data-ttu-id="3fdbe-235">アップロードが完了しますが、画像を表示する機能はまだ追加していないため、アップロードした写真はアプリに表示されません </span><span class="sxs-lookup"><span data-stu-id="3fdbe-235">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="3fdbe-236">(Web ページには "画像分析中..." と表示されたままになります。これは後で修正します)。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-236">(The web page appears to be stuck on "Analyzing image..." You'll fix that later.)</span></span>

1. <span data-ttu-id="3fdbe-237">Cloud Shell で、**images** コンテナーに画像がアップロードされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-237">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="3fdbe-238">次のチュートリアルに進む前に、**images** コンテナー内のすべてのファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-238">Before moving on to the next tutorial, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="3fdbe-239">まとめ</span><span class="sxs-lookup"><span data-stu-id="3fdbe-239">Summary</span></span>

<span data-ttu-id="3fdbe-240">このユニットでは、Azure Functions アプリを作成しました。また、サーバーレス関数を使用して、Web アプリケーションで画像を Blob Storage にアップロードできるようにする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-240">In this unit, you created an Azure Functions app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="3fdbe-241">次は、BLOB によってトリガーされるサーバーレス関数を使用して、アップロードされた画像のサムネイルを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="3fdbe-241">Next, you learn how to create thumbnails for the uploaded images using a blob-triggered serverless function.</span></span>