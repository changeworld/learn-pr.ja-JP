<span data-ttu-id="b82f4-101">歯車駆動の例を続け、温度サービスのロジックを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="b82f4-101">Let's continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="b82f4-102">具体的には、HTTP 要求からデータを受信します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="b82f4-103">関数の要件</span><span class="sxs-lookup"><span data-stu-id="b82f4-103">Function requirements</span></span>

<span data-ttu-id="b82f4-104">まず、ロジックにいくつかの要件を定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b82f4-104">First, we need to define some requirements for our logic:</span></span>

- <span data-ttu-id="b82f4-105">0 - 25 の温度には、**[OK]** のフラグを設定します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-105">Temperatures between 0-25 should be flagged as **OK**.</span></span>
- <span data-ttu-id="b82f4-106">26 - 50 の温度には、**[注意]** のフラグを設定します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-106">Temperatures between 26-50 should be flagged as **CAUTION**.</span></span>
- <span data-ttu-id="b82f4-107">50 以上の温度には、**[危険]** のフラグを設定します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-107">Temperatures above 50 should be flagged as **DANGER**.</span></span>

## <a name="add-a-function-to-our-function-app"></a><span data-ttu-id="b82f4-108">関数アプリに関数を追加する</span><span class="sxs-lookup"><span data-stu-id="b82f4-108">Add a function to our function app</span></span>

<span data-ttu-id="b82f4-109">前のユニットで説明したように、Azure には関数の作成を始めるのに役立つテンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="b82f4-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="b82f4-110">このユニットでは、`HttpTrigger` テンプレートを使用して、温度サービスを実装します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-110">In this unit, we'll use the `HttpTrigger` template to implement the temperature service.</span></span>

1. <span data-ttu-id="b82f4-111">[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="b82f4-111">Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).</span></span>

1. <span data-ttu-id="b82f4-112">左側のメニューで **[すべてのリソース]** を選択し、"**<rgn>[サンドボックス リソース グループ名]</rgn>**" を選択し、最初の演習のリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-112">Select the resource group from the first exercise by choosing **All resources** in the left-hand menu, and then selecting "**<rgn>[sandbox resource group name]</rgn>**".</span></span>

1. <span data-ttu-id="b82f4-113">グループのリソースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="b82f4-114">**escalator-functions-xxxxxxx** 項目 (稲妻の Function アイコンでも示されます) を選択することで、前の演習で作成した関数アプリの名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b82f4-114">Click the name of the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt Function icon).</span></span>

    ![強調表示されている [すべてのリソース] ブレードと作成したエスカレーター関数アプリが示されている Azure portal のスクリーンショット。](../media/5-access-function-app.png)

<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> <span data-ttu-id="b82f4-116">現時点では、このモジュールの演習は Azure Functions V1 を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="b82f4-116">The exercises in this module currently work with Azure Functions V1.</span></span> <span data-ttu-id="b82f4-117">以下の手順に慎重に従って、関数アプリで V1 ランタイム バージョンが使用されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="b82f4-117">Please follow these steps carefully to make sure your function app uses the V1 runtime version.</span></span> 

1. <span data-ttu-id="b82f4-118">**[Function Apps]** リストで、関数アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-118">Select your function app in the **Function Apps** list.</span></span>
1. <span data-ttu-id="b82f4-119">**[プラットフォーム機能]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-119">Select **Platform features**.</span></span>
1. <span data-ttu-id="b82f4-120">**[プラットフォーム機能]** 画面の **[全般設定]** で、**[Function App の設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-120">In the **Platform features** screen, select **Function app settings** under **General Settings**.</span></span>
1. <span data-ttu-id="b82f4-121">**[ランタイム バージョン]** で *[~1]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-121">Select *~1* in the **Runtime version** .</span></span>
1. <span data-ttu-id="b82f4-122">**[Function App の設定]** を閉じます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-122">Close **Function app settings**.</span></span>

<span data-ttu-id="b82f4-123">これで、Azure Functions V1 ランタイムを使用するように関数アプリが構成されました。</span><span class="sxs-lookup"><span data-stu-id="b82f4-123">Our function app is now configured to use the Azure Functions V1 runtime.</span></span> <span data-ttu-id="b82f4-124">最初の関数の作成を続行できます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-124">We can now continue to create our first function.</span></span>
<!-- End temporary fix for issue #2498. --> 

1. <span data-ttu-id="b82f4-125">左側のメニューには、関数アプリ名と 3 つの項目 (*[関数]*、*[プロキシ]*、*[スロット]*) を含むサブメニューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-125">The left-side menu displays your function app name and a submenu with three items: *Functions*, *Proxies*, and *Slots*.</span></span>  

1. <span data-ttu-id="b82f4-126">初めての関数の作成を始めるには、**[Functions]** をクリックし、結果のページの上部にある **[新しい関数]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b82f4-126">To start creating our first function, select **Functions** and click  the **New function** button at the top of the resulting page.</span></span>

    ![関数アプリの Functions リストと強調表示された [Functions] メニュー項目および [新しい関数] ボタンが示されている Azure portal のスクリーンショット。](../media/5-function-add-button.png)

1. <span data-ttu-id="b82f4-128">クイック スタート画面で、次のスクリーンショットで示されているように、**[関数を独自に作成する]** セクションの **[カスタム関数]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-128">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span> <span data-ttu-id="b82f4-129">[クイック スタート] 画面が表示されない場合は、ページの上部にある **[クイックスタートに移動します]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b82f4-129">If you don't see the Quickstart screen, click on the **go to the quickstart** link at the top of the page.</span></span>

    ![[関数を独自に作成する] セクションの [カスタム関数] ボタンが強調表示されている [クイック スタート] ブレードが示されている Azure portal のスクリーンショット。](../media/5-custom-function.png)

1. <span data-ttu-id="b82f4-131">次のスクリーンショットに示すように、画面に表示されたテンプレートのリストから、**[HTTP トリガー]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-131">From the list of templates displayed on the screen, select the **HTTP trigger** template as shown in the following screenshot.</span></span>

1. <span data-ttu-id="b82f4-132">表示された **[新しい関数]** ダイアログの名前フィールドに「**DriveGearTemperatureService**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-132">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="b82f4-133">承認レベルを "関数" のままにして **[作成]** ボタンを押し、関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-133">Leave the Authorization level as "Function" and press the **Create** button to create the function.</span></span>

1. <span data-ttu-id="b82f4-134">関数の作成が完了すると、コード エディターが開き、*index.js* コード ファイルの内容が示されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-134">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="b82f4-135">テンプレートが生成された既定のコードは、次のスニペット内に一覧されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-135">The default code that the template generated for us is listed in the following snippet.</span></span>

    ```javascript
    module.exports = function (context, req) {
        context.log('JavaScript HTTP trigger function processed a request.');
    
        if (req.query.name || (req.body && req.body.name)) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + (req.query.name || req.body.name)
            };
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

    <span data-ttu-id="b82f4-136">関数では、HTTP 要求のクエリ文字列から、または要求本文の一部として渡される名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="b82f4-136">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="b82f4-137">この関数では、メッセージ "**Hello, {name}**" を返し、要求で送信された名前をエコー バックすることによって応答が行われます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-137">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

    <span data-ttu-id="b82f4-138">ソース ビューの右側にタブが 2 つ表示されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-138">On the right-hand side of the source view, you'll find two tabs.</span></span> <span data-ttu-id="b82f4-139">**[ファイルの表示]** タブでは、ご自分の関数におけるコードと構成ファイルが一覧されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-139">The **View files** tab lists the code and config file for your function.</span></span>  <span data-ttu-id="b82f4-140">**function.json** を選択すると、次のような外観の関数の構成が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-140">Select **function.json** to view the configuration of the function, which should look like the following:</span></span>

    ```javascript
    {
        "disabled": false,
        "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
        ]
    }
    ```

    <span data-ttu-id="b82f4-141">この構成では、HTTP 要求を受け取ったときに実行する関数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-141">This configuration declares that the function runs when it receives an HTTP request.</span></span> <span data-ttu-id="b82f4-142">出力バインディングでは、HTTP 応答として送信される応答を宣言します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-142">The output binding declares that the response will be sent as an HTTP response.</span></span>    

## <a name="test-the-function"></a><span data-ttu-id="b82f4-143">関数をテストする</span><span class="sxs-lookup"><span data-stu-id="b82f4-143">Test the function</span></span>

> [!TIP]
> <span data-ttu-id="b82f4-144">**cURL** は、ファイルを送受信するために使用できるコマンドライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="b82f4-144">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="b82f4-145">このツールは、Linux、macOS、および Windows 10 に含まれており、その他のほとんどのオペレーティング システムでもダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-145">It's included with Linux, macOS, and Windows 10, and can be downloaded for most other operating systems.</span></span> <span data-ttu-id="b82f4-146">cURL では、HTTP、HTTPS、FTP、FTPS、SFTP、LDAP、TELNET、SMTP、POP3 などのさまざまなプロトコルをサポートします。詳細については、次のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b82f4-146">cURL supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3, etc. For more information, refer to the links below:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="b82f4-147">関数をテストするには、コマンド ラインで cURL を使用して、関数 URL に HTTP 要求を送信できます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-147">To test the function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="b82f4-148">関数のエンドポイント URL を見つけるには、次のスクリーンショットに示すように、関数コードに戻り、**[関数の URL の取得]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-148">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="b82f4-149">このリンクを一時的に保存します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-149">Save this link temporarily.</span></span>

![[関数の URL の取得] ボタンが強調表示されている関数エディターが示されている Azure portal のスクリーンショット。](../media/5-get-function-url.png)

### <a name="securing-http-triggers"></a><span data-ttu-id="b82f4-151">HTTP トリガーをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="b82f4-151">Securing HTTP triggers</span></span>

<span data-ttu-id="b82f4-152">HTTP トリガーを使用すると、各要求で表示されるキーを要求することで、API キーを使用して不明な呼び出し元をブロックすることができます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-152">HTTP triggers let you use API keys to block unknown callers by requiring the key to be present on each request.</span></span> <span data-ttu-id="b82f4-153">関数を作成するときは、"_認可レベル_" を選択します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-153">When you create a function, you select the _authorization level_.</span></span> <span data-ttu-id="b82f4-154">既定では、これは関数固有の API キーが必要な "関数" に設定されますが、グローバルな "マスター" キーを使用するには "管理者" を設定したり、キーが必要ないことを示すには "匿名" を設定したりすることもできます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-154">By default, it's set to "Function", which requires a function-specific API key, but it can also be set to "Admin" to use a global "master" key, or "Anonymous" to indicate that no key is required.</span></span> <span data-ttu-id="b82f4-155">作成後に関数プロパティから認証レベルを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-155">You can also change the authorization level through the function properties after creation.</span></span>

<span data-ttu-id="b82f4-156">Microsoft でこの関数を作成したときに [関数] を指定しているため、HTTP 要求を送信するときにキーを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b82f4-156">Since we specified "Function" when we created this function, we will need to supply the key when we send the HTTP request.</span></span> <span data-ttu-id="b82f4-157">これは、`code` という名前のクエリ文字列パラメーターとして、または `x-functions-key` という名前の HTTP ヘッダー (推奨) として送信できます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-157">You can send it as a query string parameter named `code`, or as an HTTP header (preferred) named `x-functions-key`.</span></span>

<span data-ttu-id="b82f4-158">関数とマスター キーは、関数を展開したときに **[管理]** セクションに表示されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-158">The function and master keys are found in the **Manage** section when the function is expanded.</span></span> <span data-ttu-id="b82f4-159">既定では、これらは非表示であり、自分で表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b82f4-159">By default, they are hidden, and you need to display them.</span></span>

1. <span data-ttu-id="b82f4-160">関数を展開して **[管理]** セクションを選択し、既定の関数キーを表示してクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="b82f4-160">Expand your function and select the **Manage** section, show the default Function Key, and copy it to the clipboard.</span></span>

    ![公開された関数キーが強調表示されている関数の [管理] ブレードが示されている Azure portal のスクリーンショット。](../media/5-get-function-key.png)

1. <span data-ttu-id="b82f4-162">次に、**cURL** ツールをインストールしたコマンド ラインから、関数の URL と関数キーを使用して cURL コマンドの書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-162">Next, from the command line where you installed the **cURL** tool, format a cURL command with the URL for your function, and the Function key.</span></span>

    - <span data-ttu-id="b82f4-163">`POST` 要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-163">Use a `POST` request.</span></span>
    - <span data-ttu-id="b82f4-164">`application/json` の種類の `Content-Type` ヘッダー値を追加します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-164">Add a `Content-Type` header value of type `application/json`.</span></span>
    - <span data-ttu-id="b82f4-165">以下の URL は必ず実際の URL に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="b82f4-165">Make sure to replace the URL below with your own.</span></span>
    - <span data-ttu-id="b82f4-166">関数キーをヘッダー値 `x-functions-key` として渡します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-166">Pass the Function Key as the header value `x-functions-key`.</span></span>

    ```bash
    curl --header "Content-Type: application/json" --header "x-functions-key: <your-function-key>" --request POST --data "{\"name\": \"Azure Function\"}" https://<your-url-here>/api/DriveGearTemperatureService
    ```

<span data-ttu-id="b82f4-167">この関数では、`"Hello Azure Function"` というテキストで応答します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-167">The function will respond back with the text `"Hello Azure Function"`.</span></span>

> [!CAUTION]
> <span data-ttu-id="b82f4-168">Windows を使用している場合は、コマンド プロンプトから `cURL` を実行してください。</span><span class="sxs-lookup"><span data-stu-id="b82f4-168">If you are on Windows, please run  `cURL` from the command prompt.</span></span> <span data-ttu-id="b82f4-169">PowerShell には *curl* コマンドがありますが、それは Invoke-WebRequest の別名であり、`cURL` と同じものではありません。</span><span class="sxs-lookup"><span data-stu-id="b82f4-169">PowerShell has a *curl* command, but it's an alias for Invoke-WebRequest and is not the same as `cURL`.</span></span>

> [!NOTE]
> <span data-ttu-id="b82f4-170">選択した関数の横にある **[テスト]** タブで個々の関数のセクションからテストすることもできますが、関数キー システムはここでは必要ないので、それが動作していることを確認することはできません。</span><span class="sxs-lookup"><span data-stu-id="b82f4-170">You can also test from an individual function's section with the **Test** tab on the side of a selected function, though you won't be able to verify the function key system is working, as it is not required here.</span></span> <span data-ttu-id="b82f4-171">テスト インターフェイスで適切なヘッダー値とパラメーター値を追加し、**[実行]** ボタンをクリックしてテスト出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-171">Add the appropriate header and parameter values in the Test interface and click the **Run** button to see the test output.</span></span>

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="b82f4-172">関数にビジネス ロジックを追加する</span><span class="sxs-lookup"><span data-stu-id="b82f4-172">Add business logic to the function</span></span>

<span data-ttu-id="b82f4-173">次に、受け取った測定温度を確認し、それぞれの状態を設定する関数にロジックを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="b82f4-173">Next, let's add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="b82f4-174">この関数では、温度測定値の配列が要求されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-174">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="b82f4-175">次の JSON スニペットは、この関数に送信する要求本文の例です。</span><span class="sxs-lookup"><span data-stu-id="b82f4-175">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="b82f4-176">`reading` エントリごとに、ID、タイムスタンプ、温度があります。</span><span class="sxs-lookup"><span data-stu-id="b82f4-176">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

```json
{
    "readings": [
        {
            "driveGearId": 1,
            "timestamp": 1534263995,
            "temperature": 23
        },
        {
            "driveGearId": 3,
            "timestamp": 1534264048,
            "temperature": 45
        },
        {
            "driveGearId": 18,
            "timestamp": 1534264050,
            "temperature": 55
        }
    ]
}
```

<span data-ttu-id="b82f4-177">次に、関数にある既定のコードを、ビジネス ロジックを実装する次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-177">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span>

1. <span data-ttu-id="b82f4-178">**index.js** ファイルを開き、次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-178">Open the **index.js** file and replace it with the following code.</span></span>

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        req.body.readings.forEach(function(reading) {

            if(reading.temperature<=25) {
                reading.status = 'OK';
            } else if (reading.temperature<=50) {
                reading.status = 'CAUTION';
            } else {
                reading.status = 'DANGER'
            }
            context.log('Reading is ' + reading.status);
        });

        context.res = {
            // status: 200, /* Defaults to 200 */
            body: {
                "readings": req.body.readings
            }
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please send an array of readings in the request body"
        };
    }
    context.done();
};
```

<span data-ttu-id="b82f4-179">ここで追加したロジックは簡単です。</span><span class="sxs-lookup"><span data-stu-id="b82f4-179">The logic we added is straightforward.</span></span> <span data-ttu-id="b82f4-180">測定値の配列を反復処理して、温度フィールドを確認します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-180">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="b82f4-181">そのフィールドの値に応じて、**[OK]**、**[注意]**、または **[危険]** の状態を設定します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-181">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="b82f4-182">次に、各エントリに追加された状態フィールドと共に、測定値の配列を返します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-182">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="b82f4-183">`log` ステートメントに注目してください。</span><span class="sxs-lookup"><span data-stu-id="b82f4-183">Notice the `log` statements.</span></span> <span data-ttu-id="b82f4-184">関数が実行されると、これらのステートメントによってログ ウィンドウにメッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-184">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="b82f4-185">ビジネス ロジックをテストする</span><span class="sxs-lookup"><span data-stu-id="b82f4-185">Test our business logic</span></span>

<span data-ttu-id="b82f4-186">この場合、ポータル内の **[テスト]** ウィンドウを使用して、関数をテストします。</span><span class="sxs-lookup"><span data-stu-id="b82f4-186">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="b82f4-187">右側のポップアップ メニューから **[テスト]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-187">Open the **Test** window from the right-hand side flyout menu.</span></span>

1. <span data-ttu-id="b82f4-188">サンプルの要求を要求本文のテキスト ボックスに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-188">Paste the sample request into the request body text box.</span></span>

    ```json
    {
        "readings": [
            {
                "driveGearId": 1,
                "timestamp": 1534263995,
                "temperature": 23
            },
            {
                "driveGearId": 3,
                "timestamp": 1534264048,
                "temperature": 45
            },
            {
                "driveGearId": 18,
                "timestamp": 1534264050,
                "temperature": 55
            }
        ]
    }
    ```

1. <span data-ttu-id="b82f4-189">**[実行]** を選択して、出力ウィンドウに応答を表示します。</span><span class="sxs-lookup"><span data-stu-id="b82f4-189">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="b82f4-190">ログ メッセージを表示するには、ページの下部にあるポップアップで **[ログ]** タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-190">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="b82f4-191">次のスクリーンショットでは、出力ウィンドウに応答の例と **[ログ]** ウィンドウにメッセージが示されています。</span><span class="sxs-lookup"><span data-stu-id="b82f4-191">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

    ![[テスト] タブと [ログ] タブが表示されている関数エディター ブレードが示されている Azure portal のスクリーンショット。](../media/5-portal-testing.png)

    <span data-ttu-id="b82f4-194">出力ウィンドウで、状態フィールドが各測定値に正しく追加されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-194">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

    <span data-ttu-id="b82f4-195">**[監視]** ダッシュボードに移動すると、Application Insights に要求が記録されていることを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="b82f4-195">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

    ![関数の [監視] ダッシュボードに前のテストの成功結果が示されている Azure portal のスクリーンショット。](../media/5-app-insights.png)
