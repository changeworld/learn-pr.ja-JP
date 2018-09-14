<span data-ttu-id="392f1-101">この演習では、歯車駆動の例を続け、温度サービスのロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="392f1-101">In this exercise, we'll continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="392f1-102">具体的には、HTTP 要求からデータを受信します。</span><span class="sxs-lookup"><span data-stu-id="392f1-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="392f1-103">関数の要件</span><span class="sxs-lookup"><span data-stu-id="392f1-103">Function requirements</span></span>
<span data-ttu-id="392f1-104">ロジックにいくつかの要件を定義しましょう。</span><span class="sxs-lookup"><span data-stu-id="392f1-104">Let's define some requirements for our logic:</span></span>
- <span data-ttu-id="392f1-105">0 - 25 の温度に **[OK]** のフラグを設定してください</span><span class="sxs-lookup"><span data-stu-id="392f1-105">temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="392f1-106">26 - 50 の温度に **[注意]** のフラグを設定してください</span><span class="sxs-lookup"><span data-stu-id="392f1-106">temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="392f1-107">50 以上の温度に **[危険]** のフラグを設定してください</span><span class="sxs-lookup"><span data-stu-id="392f1-107">temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="add-a-function-to-an-azure-function-app"></a><span data-ttu-id="392f1-108">Azure 関数アプリに関数を追加する</span><span class="sxs-lookup"><span data-stu-id="392f1-108">Add a function to an Azure function app</span></span>

<span data-ttu-id="392f1-109">前のユニットで説明したように、Azure では関数のビルドを始めるのに役立つテンプレートを提供しています。</span><span class="sxs-lookup"><span data-stu-id="392f1-109">As we discussed in the preceding unit, Azure provides templates that help you get started building functions.</span></span> <span data-ttu-id="392f1-110">この演習では、HttpTrigger テンプレートを使用して、温度サービスを実装します。</span><span class="sxs-lookup"><span data-stu-id="392f1-110">In this exercise, we'll use the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="392f1-111">ご自分の Azure アカウントを使用して [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="392f1-111">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true) using your Azure account.</span></span>
1. <span data-ttu-id="392f1-112">左側のメニューで **[すべてのリソース]** を選んで、**[escalator-functions-group]** を選択し、最初の演習で作成したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="392f1-112">Select the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>
1. <span data-ttu-id="392f1-113">グループのリソースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-113">The resources for the group will then be displayed.</span></span> <span data-ttu-id="392f1-114">**escalator-functions-xxxxxxx** 項目 (稲妻アイコンでも示されます) を選択し、前の演習で作成した関数アプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="392f1-114">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>
  <span data-ttu-id="392f1-115">![escalator-functions-group と作成した関数アプリが強調表示されている、ポータル内の [すべてのリソース] のスクリーンショット。](../images/6-access-function-app.png)</span><span class="sxs-lookup"><span data-stu-id="392f1-115">![Screenshot of All Resources in the portal highlighting the escalator-functions-group and the function app we created.](../images/6-access-function-app.png)</span></span>
1. <span data-ttu-id="392f1-116">左側のメニューには、関数アプリ名と 3 項目 (*[関数]*、*[プロキシ]*、*[スロット]*) を含むサブメニューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-116">The left-side menu displays your function app name and a submenu with three items - *Functions*, *Proxies*, and *Slots*.</span></span>  <span data-ttu-id="392f1-117">最初の関数の作成を開始するには、ナビゲーション ツリーの **[関数]** にマウス ポインターを置き、表示された **+** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="392f1-117">To start creating our first function, move your mouse over **Functions** in the navigation tree and click  the **+** button that appears.</span></span>
  <span data-ttu-id="392f1-118">![[関数] メニュー項目をポイントしたときに表示されるプラス記号を示すスクリーンショット。クリックすると、関数の作成手順が開始されます。](../images/5-function-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="392f1-118">![Screenshot showing plus sign when you hover over the Functions menu item that, when clicked, starts the function creation procedure.](../images/5-function-add-button.png)</span></span>
1. <span data-ttu-id="392f1-119">クイック スタート画面で、次のスクリーンショットで示されているように、**[Get started on your own]\(独自に作業を開始する\)** セクションの **[カスタム関数]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="392f1-119">In the Quickstart screen, select the **Custom function** link in the **Get started on your own** section as shown in the following screenshot.</span></span>
  <span data-ttu-id="392f1-120">![[カスタム関数] ボタンが強調表示されているクイック スタート画面のスクリーンショット。](../images/6-custom-function.png)</span><span class="sxs-lookup"><span data-stu-id="392f1-120">![Screenshot of Quickstart screen with the "Custom function" button highlighted.](../images/6-custom-function.png)</span></span>
1. <span data-ttu-id="392f1-121">次のスクリーンショットに示すように、画面に表示されたテンプレートの一覧から、HTTP トリガー テンプレートでの JavaScript の実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="392f1-121">From the list of templates displayed on the screen, select the JavaScript implementation of the HTTP trigger template as shown in the following screenshot.</span></span>
  <span data-ttu-id="392f1-122">![HTTP トリガーと JavaScript オプションが強調表示されているテンプレートの一覧のスクリーンショット。](../images/6-httptrigger-template.png)</span><span class="sxs-lookup"><span data-stu-id="392f1-122">![Screenshot of template list, with the HTTP trigger and JavaScript option highlighted.](../images/6-httptrigger-template.png)</span></span>
1.  <span data-ttu-id="392f1-123">表示された **[新しい関数]** ダイアログの名前フィールドに「**DriveGearTemperatureService**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="392f1-123">Enter **DriveGearTemperatureService** in the name field of the **New Function** dialog that appears.</span></span> <span data-ttu-id="392f1-124">関数に名前を付けたら、次のスクリーンショットに示されているように、**[作成]** ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="392f1-124">After you have named your function, press the **Create** button, as illustrated in te following screenshot.</span></span>
  <span data-ttu-id="392f1-125">![強調表示され、"DriveGearTemperatureService" の値が設定されている [名前] フィールドが示されている、新しい関数フォームのスクリーンショット](../images/6-create-httptrigger-form.png)</span><span class="sxs-lookup"><span data-stu-id="392f1-125">![Screenshot of New Function form, showing Name field highlighted and set with the value "DriveGearTemperatureService"](../images/6-create-httptrigger-form.png)</span></span>
1. <span data-ttu-id="392f1-126">関数の作成が完了すると、コード エディターが *index.js* コード ファイルの内容と共に表示されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-126">When your function creation completes, the code editor opens with the contents of the *index.js* code file.</span></span> <span data-ttu-id="392f1-127">テンプレートが生成された既定のコードは、次のスニペット内に一覧されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-127">The default code that the template generated for us is listed in the following snippet.</span></span>

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

<span data-ttu-id="392f1-128">関数では、HTTP 要求のクエリ文字列から、または要求本文の一部として渡される名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="392f1-128">Our function expects a name to be passed in either through the HTTP request query string or as part of the request body.</span></span> <span data-ttu-id="392f1-129">この関数では、メッセージ "**こんにちは、{名前}**" を返し、要求で送信された名前をエコー バックすることによって応答が行われます。</span><span class="sxs-lookup"><span data-stu-id="392f1-129">The function responds by returning the message  **Hello, {name}**, echoing back the name that was sent in the request.</span></span>

1. <span data-ttu-id="392f1-130">ソース ビューの右側に 2 つのタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-130">On the right-hand side of the source view, you'll see two tabs.</span></span> <span data-ttu-id="392f1-131">\*\*[ファイルの表示] には、関数のコードと構成ファイルが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-131">The \*\*View file lists the code and config file for your function.</span></span>  <span data-ttu-id="392f1-132">関数の構成を表示するには、**function.json** を選択します。</span><span class="sxs-lookup"><span data-stu-id="392f1-132">Select **function.json** to view the configuration of the function.</span></span> <span data-ttu-id="392f1-133">次のスニペットは、**function.json** ファイルの内容を示しています。</span><span class="sxs-lookup"><span data-stu-id="392f1-133">The following snippet shows the contents of the **function.json** file.</span></span> <span data-ttu-id="392f1-134">この構成では、HTTP 要求を受け取ったときに関数が実行されることを宣言します。</span><span class="sxs-lookup"><span data-stu-id="392f1-134">This configuration declares that the function is runs when it receives an HTTP request.</span></span> <span data-ttu-id="392f1-135">出力バインドでは、HTTP 応答として送信される応答を宣言します。</span><span class="sxs-lookup"><span data-stu-id="392f1-135">The output binding declares that the response will be sent as an HTTP response.</span></span>

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
> [!TIP]
> <span data-ttu-id="392f1-136">上記の JSON ファイルでは、トリガーがバインドの配列で定義されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="392f1-136">Notice in the preceding JSON file that the trigger is defined in an array of bindings.</span></span> <span data-ttu-id="392f1-137">そのため、トリガーは実際には特殊な種類のバインドです。</span><span class="sxs-lookup"><span data-stu-id="392f1-137">So, a trigger is actually a special type of binding.</span></span>

## <a name="run-our-function"></a><span data-ttu-id="392f1-138">関数を実行する</span><span class="sxs-lookup"><span data-stu-id="392f1-138">Run our function</span></span>

> [!TIP]
> <span data-ttu-id="392f1-139">**cURL** は、ファイルを送受信するために使用できるコマンドライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="392f1-139">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="392f1-140">cURL.exe では、HTTP、HTTPS、FTP、FTPS、SFTP、LDAP、TELNET、SMTP、POP3 などのさまざまなプロトコルがサポートされます。詳細については、次のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="392f1-140">cURL.exe supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3 etc. For more information please refer the below links:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/>

<span data-ttu-id="392f1-141">関数をテストするため、コマンド ラインで cURL を使用して、関数 URL に HTTP 要求を送信できます。</span><span class="sxs-lookup"><span data-stu-id="392f1-141">To test our function, you can send an HTTP request to the function URL using cURL on the command line.</span></span> <span data-ttu-id="392f1-142">関数のエンドポイント URL を見つけるには、次のスクリーンショットに示すように、関数コードに戻り、**[関数の URL の取得]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="392f1-142">To find the endpoint URL of the function, return to your function code and select the **Get function URL** link, as shown in the following screenshot.</span></span> <span data-ttu-id="392f1-143">このリンクをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="392f1-143">Copy this link to your clipboard.</span></span>  

 ![ポータルの [Function App] セクションにあるコード エディターのスクリーンショット。](../images/6-get-function-url.png)

<span data-ttu-id="392f1-146">この関数の URL を使用して、URL を自分の URL に置き換える次の cURL コマンドをコマンド ラインから実行します。</span><span class="sxs-lookup"><span data-stu-id="392f1-146">Using this function URL, run the following cURL command from your command line, replacing the URL with your own.</span></span>

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

<span data-ttu-id="392f1-147">次のスクリーン ショットは、コマンドラインで既定の実装から受信した応答の例を示しています。</span><span class="sxs-lookup"><span data-stu-id="392f1-147">The following screenshot shows an example of the response you receive from the default implementation in the command line.</span></span>

![サンプルの cUrl コマンドと応答のコマンド ラインのスクリーンショット。](../images/6-premadefunction-curl.png)

## <a name="add-business-logic-to-the-function"></a><span data-ttu-id="392f1-149">関数にビジネス ロジックを追加する</span><span class="sxs-lookup"><span data-stu-id="392f1-149">Add business logic to the function</span></span>

<span data-ttu-id="392f1-150">次は、受け取った測定温度を確認し、それぞれの状態を設定する関数にロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="392f1-150">It's time to add the logic to the function that checks temperature readings that it receives and sets a status for each.</span></span>

<span data-ttu-id="392f1-151">この関数では、温度測定値の配列が要求されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-151">Our function is expecting an array of temperature readings.</span></span> <span data-ttu-id="392f1-152">次の JSON スニペットは、この関数に送信する要求本文の例です。</span><span class="sxs-lookup"><span data-stu-id="392f1-152">The following JSON snippet is an example of the request body that we'll send to our function.</span></span> <span data-ttu-id="392f1-153">`reading` エントリごとに、ID、タイムスタンプ、温度があります。</span><span class="sxs-lookup"><span data-stu-id="392f1-153">Each `reading` entry has an ID, timestamp, and temperature.</span></span>

```javascript
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

<span data-ttu-id="392f1-154">次に、関数にある既定のコードを、ビジネス ロジックを実装する次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="392f1-154">Next, we'll replace the default code in our function with the following code that implements our business logic.</span></span> 

1. <span data-ttu-id="392f1-155">**index.js** ファイルを開き、次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="392f1-155">Open the **index.js** file, and replace it with the following code.</span></span>

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
<span data-ttu-id="392f1-156">ここで追加したロジックは簡単です。</span><span class="sxs-lookup"><span data-stu-id="392f1-156">The logic we added is straightforward.</span></span> <span data-ttu-id="392f1-157">測定値の配列を反復処理して、温度フィールドを確認します。</span><span class="sxs-lookup"><span data-stu-id="392f1-157">We iterate over the array of readings and check the temperature field.</span></span> <span data-ttu-id="392f1-158">そのフィールドの値に応じて、**[OK]**、**[注意]**、または **[危険]** の状態を設定します。</span><span class="sxs-lookup"><span data-stu-id="392f1-158">Depending on the value of that field, we set a status of **OK**, **CAUTION**, or **DANGER**.</span></span> <span data-ttu-id="392f1-159">次に、各エントリに追加された状態フィールドと共に、測定値の配列を返信します。</span><span class="sxs-lookup"><span data-stu-id="392f1-159">We then send back the array of readings with a status field added to each entry.</span></span>

<span data-ttu-id="392f1-160">ログ ステートメントに注目してください。</span><span class="sxs-lookup"><span data-stu-id="392f1-160">Notice the log statements.</span></span> <span data-ttu-id="392f1-161">関数が実行されると、これらのステートメントによってログ ウィンドウにメッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="392f1-161">When the function runs, these statements will add messages in the log window.</span></span>

## <a name="test-our-business-logic"></a><span data-ttu-id="392f1-162">ビジネス ロジックをテストする</span><span class="sxs-lookup"><span data-stu-id="392f1-162">Test our business logic</span></span>

<span data-ttu-id="392f1-163">この場合、ポータル内の **[テスト]** ウィンドウを使用して、関数をテストします。</span><span class="sxs-lookup"><span data-stu-id="392f1-163">In this case, we're going to use the **Test** pane in the portal to test our function.</span></span>

1. <span data-ttu-id="392f1-164">右側のポップアップ メニューから **[テスト]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="392f1-164">Open the **Test** window from the right-hand side flyout menu.</span></span>
1. <span data-ttu-id="392f1-165">上記の要求のサンプルを要求本文のテキスト ボックスに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="392f1-165">Paste the sample request from above into the request body text box.</span></span> 
1. <span data-ttu-id="392f1-166">**[実行]** を選択して、出力ウィンドウに応答を表示します。</span><span class="sxs-lookup"><span data-stu-id="392f1-166">Select **Run** and view the response in the output pane.</span></span> <span data-ttu-id="392f1-167">ログ メッセージを表示するには、ページの下部にあるポップアップで **[ログ]** タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="392f1-167">To see log messages, open the **Logs** tab in the bottom flyout of the page.</span></span> <span data-ttu-id="392f1-168">次のスクリーンショットでは、出力ウィンドウに応答の例と **[ログ]** ウィンドウにメッセージが示されています。</span><span class="sxs-lookup"><span data-stu-id="392f1-168">The following screenshot shows an example response in the output pane and messages in the  **Logs** pane.</span></span>

![ポータルでの関数インターフェイス内の [テスト] と [ログ] タブのスクリーンショット。](../images/6-portal-testing.png)

<span data-ttu-id="392f1-171">出力ウィンドウで、状態フィールドが各測定値に正しく追加されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="392f1-171">You can see in the output pane that our status field has been correctly added to each of the readings.</span></span>

<span data-ttu-id="392f1-172">**[監視]** ダッシュボードに移動すると、Application Insights に要求が記録されていることを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="392f1-172">If you navigate to the **Monitor** dashboard, you'll see that the request has been logged to Application Insights.</span></span>

![関数からの成功メッセージを表示している "監視" ダッシュボードの一部のスクリーンショット。](../images/6-app-insights.png)

