<span data-ttu-id="244e5-101">この演習では、歯車駆動の例を続け、温度サービスのロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="244e5-101">In this exercise, we'll continue with our gear drive example and add the logic for the temperature service.</span></span> <span data-ttu-id="244e5-102">具体的には、HTTP 要求からデータを受信します。</span><span class="sxs-lookup"><span data-stu-id="244e5-102">Specifically, we're going to receive data from an HTTP request.</span></span>

## <a name="function-requirements"></a><span data-ttu-id="244e5-103">関数の要件</span><span class="sxs-lookup"><span data-stu-id="244e5-103">Function requirements</span></span>
<span data-ttu-id="244e5-104">ロジックにいくつかの要件を定義しましょう。</span><span class="sxs-lookup"><span data-stu-id="244e5-104">Let's define some requirements for our logic:</span></span>
- <span data-ttu-id="244e5-105">0 - 25 の温度に **[OK]** のフラグを設定してください</span><span class="sxs-lookup"><span data-stu-id="244e5-105">temperatures between 0-25 should be flagged as **OK**</span></span>
- <span data-ttu-id="244e5-106">26 - 50 の温度に **[注意]** のフラグを設定してください</span><span class="sxs-lookup"><span data-stu-id="244e5-106">temperatures between 26-50 should be flagged as **CAUTION**</span></span>
- <span data-ttu-id="244e5-107">50 以上の温度に **[危険]** のフラグを設定してください</span><span class="sxs-lookup"><span data-stu-id="244e5-107">temperatures above 50 should be flagged as **DANGER**</span></span>

## <a name="adding-a-function-to-an-azure-function-app"></a><span data-ttu-id="244e5-108">Azure 関数アプリに関数を追加する</span><span class="sxs-lookup"><span data-stu-id="244e5-108">Adding a function to an Azure function app</span></span>

<span data-ttu-id="244e5-109">既に理解していることですが、Azure Functions の使用方法を学習しているとき、Azure のヘルプを利用できます。</span><span class="sxs-lookup"><span data-stu-id="244e5-109">As we have already learned, Azure provides a helping hand when you're learning how to work with Azure Functions.</span></span> <span data-ttu-id="244e5-110">Functions を実際に始めるための優れた機能の 1 つがさまざまなテンプレートの 1 つを利用して関数を生成するという機能です。</span><span class="sxs-lookup"><span data-stu-id="244e5-110">One great feature to get your feet wet with Functions is to generate one using one of many templates.</span></span> <span data-ttu-id="244e5-111">この演習では、HttpTrigger テンプレートを使用して温度サービスを実装します。</span><span class="sxs-lookup"><span data-stu-id="244e5-111">In this exercise, you will be using the HttpTrigger template to implement the temperature service.</span></span>

1. <span data-ttu-id="244e5-112">Azure アカウントを使用して [Azure Portal](https://portal.azure.com) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="244e5-112">Sign in to the [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
1. <span data-ttu-id="244e5-113">左側のメニューで **[すべてのリソース]** を選んで、**[escalator-functions-group]** を選択して、最初の演習で作成したリソース グループにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="244e5-113">Access the resource group you created in the first exercise by choosing **All resources** in the left-hand menu, and then selecting **escalator-functions-group**.</span></span>
1. <span data-ttu-id="244e5-114">グループのリソースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-114">The resources for the group will then be displayed.</span></span> <span data-ttu-id="244e5-115">**escalator-functions-xxxxxxx** 項目 (稲妻アイコンでも示されます) を選択し、前の演習で作成した関数アプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="244e5-115">Access the function app that you created in the previous exercise by selecting the **escalator-functions-xxxxxxx** item (also indicated by the lightning bolt icon).</span></span>
  <span data-ttu-id="244e5-116">![関数アプリにアクセスする](../images/6-access-function-app.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-116">![Access the function app](../images/6-access-function-app.png)</span></span>
1. <span data-ttu-id="244e5-117">ブレードに関数アプリの概要が表示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-117">The blade will show an overview of your function app.</span></span> <span data-ttu-id="244e5-118">左側にナビゲーション ツリーもあります。関数、プロキシ、スロットが定義されていれば、このツリーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-118">There is also a navigation tree on the left that shows any functions, proxies or slots that are defined.</span></span> <span data-ttu-id="244e5-119">関数はまだないため、空になります。</span><span class="sxs-lookup"><span data-stu-id="244e5-119">We don't have a function yet, so this  will be empty.</span></span> <span data-ttu-id="244e5-120">関数を作成するには、ナビゲーション ツリーの **[関数]** にマウス カーソルを合わせ、表示される **+** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="244e5-120">To create a function, move your mouse over **Function** in the navigation tree and click  the **+** button that appears.</span></span>
  <span data-ttu-id="244e5-121">![関数ナビゲーションを追加する](../images/5-function-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-121">![Add function navigation](../images/5-function-add-button.png)</span></span>
1. <span data-ttu-id="244e5-122">クイックスタートの用意されている関数フォームで、**[関数を独自に作成する]** セクションの **[カスタム関数]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="244e5-122">In the quickstart premade function form, select the **Custom function** link in the **Get started on your own** section.</span></span>
  <span data-ttu-id="244e5-123">![用意されている関数フォーム](../images/6-custom-function.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-123">![Premade function form](../images/6-custom-function.png)</span></span>
1. <span data-ttu-id="244e5-124">これでテンプレートの一覧が提示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-124">You are now presented with a list of templates.</span></span> <span data-ttu-id="244e5-125">HTTP トリガー テンプレートの JavaScript 実装を選択します。</span><span class="sxs-lookup"><span data-stu-id="244e5-125">Select the JavaScript implementation of the HTTP trigger template.</span></span>
  <span data-ttu-id="244e5-126">![HTTP トリガー テンプレート](../images/6-httptrigger-template.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-126">![HTTP trigger template](../images/6-httptrigger-template.png)</span></span>
1. <span data-ttu-id="244e5-127">ブレードが表示されます。テンプレートで生成する内容を定義できます。</span><span class="sxs-lookup"><span data-stu-id="244e5-127">A blade will appear, allowing you to define what the template will generate.</span></span> <span data-ttu-id="244e5-128">今回、**DriveGearTemperatureService** という名前の JavaScript 関数を生成します。</span><span class="sxs-lookup"><span data-stu-id="244e5-128">In this case, you are interested in generating a JavaScript function named **DriveGearTemperatureService**.</span></span> <span data-ttu-id="244e5-129">関数に名前を付けたら、**[作成]** ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="244e5-129">After you have named your function, press the **Create** button.</span></span>
  <span data-ttu-id="244e5-130">![HTTP トリガー フォームを作成する](../images/6-create-httptrigger-form.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-130">![Create HTTP trigger form](../images/6-create-httptrigger-form.png)</span></span>
1. <span data-ttu-id="244e5-131">数秒後、関数のテンプレート化されたソース コードが提示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-131">After a few moments, you will be presented with the templated source code for your function.</span></span> <span data-ttu-id="244e5-132">用意されている関数は名前が渡されることを要求し、**Hello, {name}** を返します。</span><span class="sxs-lookup"><span data-stu-id="244e5-132">The premade function expects a name to be passed in, and it will return **Hello, {name}**.</span></span>

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

1. <span data-ttu-id="244e5-133">ソース ビューの右側に 2 つのタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-133">On the right-hand side of the source view, you will see two tabs.</span></span> <span data-ttu-id="244e5-134">**[ファイルの表示]** タブに、関数をサポートしているすべてのファイルが表示されます。**function.json** を選択すると、関数の構成が表示されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-134">You are able to view all the files supporting the function in the **View files** tab. You can select **function.json** to view the configuration of the function.</span></span> <span data-ttu-id="244e5-135">ここでは httpTrigger と出力バインドが定義されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="244e5-135">Here you can see the httpTrigger defined, as well as the output binding.</span></span> <span data-ttu-id="244e5-136">この構成からは、関数が HTTP 要求によって始動することがわかります。出力バインドからは、応答が HTTP 応答として送信されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="244e5-136">This configuration describes that the function is initiated by an HTTP request, and the output binding describes that the response will be sent as an HTTP response.</span></span>

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

## <a name="running-the-premade-azure-function"></a><span data-ttu-id="244e5-137">用意されている Azure 関数を実行する</span><span class="sxs-lookup"><span data-stu-id="244e5-137">Running the premade Azure function</span></span>

<span data-ttu-id="244e5-138">関数を実行するには、コマンド プロンプトから cURL からの HTTP 要求を開始できます。</span><span class="sxs-lookup"><span data-stu-id="244e5-138">To run the function, you can initiate an HTTP request from cURL from a command prompt.</span></span> <span data-ttu-id="244e5-139">エンドポイント URL を見つけるには、関数コードに戻り、**[関数の URL の取得]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="244e5-139">To find the endpoint URL, return to your function code and select the **Get function URL** link.</span></span> <span data-ttu-id="244e5-140">このリンクをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="244e5-140">Copy this link to your clipboard.</span></span>  <span data-ttu-id="244e5-141">URL は次のようになります: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![エンドポイント URL の取得](../images/6-get-function-url.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-141">Your URL should look something similar to the following: https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg== ![Get endpoint URL](../images/6-get-function-url.png)</span></span>

<span data-ttu-id="244e5-142">その URL を使用し、cURL コマンドを実行して関数を開始します (URL は自分の URL に変更します)。</span><span class="sxs-lookup"><span data-stu-id="244e5-142">With that URL, run a cURL command to initiate your function (replacing the URL with your own):</span></span>

```bash
curl --header "Content-Type: application/json" --request POST --data "{\"name\": \"Azure Function\"}" https://escalator-functions-xxxxxxx.azurewebsites.net/api/DriveGearTemperatureService?code=RMt1K0AfulJmF4Ui8OSNLXsI3AFjbHznS8BcJsinBox3nDEKEwy1sg==
```

![用意されている関数 cURL 応答](../images/6-premadefunction-curl.png)

## <a name="adding-business-logic-to-the-function"></a><span data-ttu-id="244e5-144">関数にビジネス ロジックを追加する</span><span class="sxs-lookup"><span data-stu-id="244e5-144">Adding business logic to the function</span></span>

<span data-ttu-id="244e5-145">この関数は、顧客からの温度測定値の配列を要求します。</span><span class="sxs-lookup"><span data-stu-id="244e5-145">Our function is expecting an array of temperature readings from the customer.</span></span> <span data-ttu-id="244e5-146">これは要求本文の例です。</span><span class="sxs-lookup"><span data-stu-id="244e5-146">This is an example of the request body:</span></span>

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

<span data-ttu-id="244e5-147">用意されている関数コードを変更し、必要なビジネス ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="244e5-147">Modify the premade function code to implement the required business logic.</span></span> <span data-ttu-id="244e5-148">index.js ファイルを開き、次のリストで置換します。</span><span class="sxs-lookup"><span data-stu-id="244e5-148">Open the index.js file, and replace it with the following listing:</span></span>

```javascript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        for(var i=0; i<req.body.readings.length; i++){
            var reading = req.body.readings[i];
            if(reading.temperature<=25){
                context.log('Reading is OK');
                reading.status = 'OK';
                continue;
            }
            if(reading.temperature<=50){
                context.log('Reading is CAUTION');
                reading.status = 'CAUTION';
                continue;
            }
            context.log('Reading is DANGER');
            reading.status = 'DANGER'
        }
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

<span data-ttu-id="244e5-149">ログ ステートメントに注目してください。</span><span class="sxs-lookup"><span data-stu-id="244e5-149">Notice the log statements.</span></span> <span data-ttu-id="244e5-150">関数が実行されると、ログ ウィンドウにメッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="244e5-150">When the function runs, they will add messages in the log window.</span></span>

## <a name="testing-your-business-logic"></a><span data-ttu-id="244e5-151">ビジネス ロジックのテスト</span><span class="sxs-lookup"><span data-stu-id="244e5-151">Testing your business logic</span></span>

<span data-ttu-id="244e5-152">右側のポップアップ メニューからテスト ウィンドウを開き、要求本文テキスト ボックスに上のサンプル要求を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="244e5-152">Open the test window from the right-hand side flyout menu, and paste the sample request from above into the request body text box.</span></span> <span data-ttu-id="244e5-153">**[実行]** ボタンを押し、出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="244e5-153">Press the **Run** button and view the output.</span></span> <span data-ttu-id="244e5-154">下部のポップアップ メニューからログ ウィンドウを開き、ログのログ ステートメントを参照することもできます。</span><span class="sxs-lookup"><span data-stu-id="244e5-154">You can also open the log window from the bottom flyout menu to see the logging statements in the log.</span></span>
<span data-ttu-id="244e5-155">![ビジネス ロジックのテスト](../images/6-portal-testing.png) 出力ウィンドウの JSON データから、それぞれの測定値に温度ステータス フィールドが正しく追加されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="244e5-155">![Testing the business Logic](../images/6-portal-testing.png) You can see from the JSON data in the output window that our temperature status field has been added to each of the readings correctly.</span></span> <span data-ttu-id="244e5-156">場合によっては、モニター ダッシュボードにアクセスし、Application Insights に要求が記録されていることを確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="244e5-156">You may also visit the Monitor dashboard to see that the request has been logged to Application Insights.</span></span>
<span data-ttu-id="244e5-157">![Application Insights の要求](../images/6-app-insights.png)</span><span class="sxs-lookup"><span data-stu-id="244e5-157">![Request in Application Insights](../images/6-app-insights.png)</span></span>
