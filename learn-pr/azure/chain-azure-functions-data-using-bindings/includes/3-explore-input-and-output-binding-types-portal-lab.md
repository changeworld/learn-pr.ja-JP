<span data-ttu-id="f8e1c-101">この演習で作成するものの概要図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-101">The following is a high-level illustration of what we're going to build in this exercise.</span></span>

![HTTP 要求と HTTP 応答、およびそれぞれの req バインディング パラメーターと res バインディング パラメーターを示す、既定の HTTP トリガーの図](../media/3-default-http-trigger-visual-small.PNG)

<span data-ttu-id="f8e1c-103">HTTP 要求を受信したときに起動し、メッセージを送信して各要求に応答する関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-103">We'll create a function that will start when it receives an HTTP request and will respond to each request by sending back a message.</span></span> <span data-ttu-id="f8e1c-104">パラメーター `req` と `res` は、それぞれトリガー バインディングと出力バインディングです。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-104">The parameters `req` and `res` are the trigger binding and output binding, respectively.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-function-app"></a><span data-ttu-id="f8e1c-105">関数アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="f8e1c-105">Create a function app</span></span>

<span data-ttu-id="f8e1c-106">このモジュール全体で使用する関数アプリを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-106">Let's create a function app that we'll use throughout this entire module.</span></span> <span data-ttu-id="f8e1c-107">関数アプリを使用すると、リソースの管理、デプロイ、および共有を容易にするための論理ユニットとして関数をグループ化できます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-107">A function app lets you group functions as a logical unit for easier management, deployment, and sharing of resources.</span></span>

1. <span data-ttu-id="f8e1c-108">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-108">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="f8e1c-109">Azure portal の左上隅にある **[リソースの作成]** ボタンを選択し、**[コンピューティング]** > **[Function App]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-109">Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, then select **Compute** > **Function App**.</span></span>

1. <span data-ttu-id="f8e1c-110">関数アプリのプロパティを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-110">Set the function app properties as follows:</span></span>

    | <span data-ttu-id="f8e1c-111">プロパティ</span><span class="sxs-lookup"><span data-stu-id="f8e1c-111">Property</span></span>     | <span data-ttu-id="f8e1c-112">推奨値</span><span class="sxs-lookup"><span data-stu-id="f8e1c-112">Suggested value</span></span>  | <span data-ttu-id="f8e1c-113">説明</span><span class="sxs-lookup"><span data-stu-id="f8e1c-113">Description</span></span>  |
    |--------------|------------------|--------------|
    | <span data-ttu-id="f8e1c-114">**アプリ名**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-114">**App name**</span></span> | <span data-ttu-id="f8e1c-115">グローバルに一意の名前</span><span class="sxs-lookup"><span data-stu-id="f8e1c-115">Globally unique name</span></span> | <span data-ttu-id="f8e1c-116">新しい関数アプリを識別する名前。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-116">Name that identifies your new function app.</span></span> <span data-ttu-id="f8e1c-117">有効な文字は、`a-z`、`0-9`、および `-` です。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-117">Valid characters are `a-z`, `0-9`, and `-`.</span></span>  |
    | <span data-ttu-id="f8e1c-118">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-118">**Subscription**</span></span> | <span data-ttu-id="f8e1c-119">ご利用のサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="f8e1c-119">Your subscription</span></span> | <span data-ttu-id="f8e1c-120">この新しい関数アプリが作成されるサブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-120">The subscription under which this new function app is created.</span></span> |
    | <span data-ttu-id="f8e1c-121">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-121">**Resource Group**</span></span>|  <span data-ttu-id="f8e1c-122">**[既存のものを使用]** を選択し、_<rgn>[サンドボックス リソース グループ名]</rgn>_ を選びます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-122">Select **Use existing** and choose _<rgn>[sandbox resource group name]</rgn>_</span></span> | <span data-ttu-id="f8e1c-123">関数アプリを作成するリソース グループの名前。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-123">Name of the resource group in which to create your function app.</span></span> |
    | <span data-ttu-id="f8e1c-124">**OS**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-124">**OS**</span></span> | <span data-ttu-id="f8e1c-125">Windows</span><span class="sxs-lookup"><span data-stu-id="f8e1c-125">Windows</span></span> | <span data-ttu-id="f8e1c-126">関数アプリをホストするオペレーティング システム。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-126">The operating system that hosts the function app.</span></span>  |
    | <span data-ttu-id="f8e1c-127">**ホスティング プラン**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-127">**Hosting Plan**</span></span> |   <span data-ttu-id="f8e1c-128">従量課金プラン</span><span class="sxs-lookup"><span data-stu-id="f8e1c-128">Consumption plan</span></span> | <span data-ttu-id="f8e1c-129">Function App にどのようにリソースが割り当てられるかを定義するホスティング プラン。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-129">Hosting plan that defines how resources are allocated to your function app.</span></span> <span data-ttu-id="f8e1c-130">既定の **[従量課金プラン]** では、関数での必要に応じてリソースが動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-130">In the default **Consumption Plan**, resources are added dynamically as required by your functions.</span></span> <span data-ttu-id="f8e1c-131">このサーバーレス ホスティング モデルでは、関数が実行された時間にのみ課金されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-131">In this serverless hosting model, you only pay for the time your functions run.</span></span>   |
    | <span data-ttu-id="f8e1c-132">**場所**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-132">**Location**</span></span> | <span data-ttu-id="f8e1c-133">一覧から選択する</span><span class="sxs-lookup"><span data-stu-id="f8e1c-133">Select from the list</span></span> | <span data-ttu-id="f8e1c-134">下の一覧の許可されている "*サンドボックス リージョン*" の中から、お客様に最も近いものを選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-134">Choose the nearest one to you that is also one of the allowed *Sandbox regions* listed below.</span></span> |
    | <span data-ttu-id="f8e1c-135">**ランタイム スタック**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-135">**Runtime Stack**</span></span> | <span data-ttu-id="f8e1c-136">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f8e1c-136">JavaScript</span></span> | <span data-ttu-id="f8e1c-137">このモジュールのサンプル コードは、JavaScript で記述されています。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-137">The sample code in this module is written in JavaScript.</span></span>  |
    | <span data-ttu-id="f8e1c-138">**ストレージ**</span><span class="sxs-lookup"><span data-stu-id="f8e1c-138">**Storage**</span></span> |  <span data-ttu-id="f8e1c-139">グローバルに一意の名前</span><span class="sxs-lookup"><span data-stu-id="f8e1c-139">Globally unique name</span></span> |  <span data-ttu-id="f8e1c-140">関数アプリによって使用される新しいストレージ アカウントの名前。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-140">Name of the new storage account used by your function app.</span></span> <span data-ttu-id="f8e1c-141">ストレージ アカウント名の長さは 3 - 24 文字で、数字と小文字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-141">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="f8e1c-142">このダイアログ ボックスでは、アプリに指定した名前から派生した一意の名前がフィールドに入力されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-142">This dialog populates the field with a unique name that is derived from the name you gave the app.</span></span> <span data-ttu-id="f8e1c-143">ただし、別の名前や既存のアカウントを自由に使用できます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-143">However, feel free to use a different name or even an existing account.</span></span> |

    ### <a name="sandbox-regions"></a><span data-ttu-id="f8e1c-144">サンドボックス リージョン</span><span class="sxs-lookup"><span data-stu-id="f8e1c-144">Sandbox regions</span></span>
    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="f8e1c-145">**[作成]** を選択して、関数アプリをプロビジョニングし、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-145">Select **Create** to provision and deploy the function app.</span></span>

1. <span data-ttu-id="f8e1c-146">ポータルの右上隅の通知アイコンを選択し、"**デプロイが進行中**" であることを示す次のようなメッセージが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-146">Select the Notification icon in the upper-right corner of the portal and watch for a **Deployment in progress** message similar to the following message.</span></span>

    ![関数アプリのデプロイが進行中であることを示す通知](../media/3-func-app-deploy-progress-small.PNG)

1. <span data-ttu-id="f8e1c-148">デプロイには時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-148">Deployment can take some time.</span></span> <span data-ttu-id="f8e1c-149">そのため、通知ハブを表示した状態で、"**デプロイに成功**" したことを示す次のようなメッセージが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-149">So, stay in the notification hub and  watch for a **Deployment succeeded** message similar to the following message.</span></span>

    ![関数アプリのデプロイが完了したことを示す通知](../media/3-func-app-deploy-success-small.PNG)

 1. <span data-ttu-id="f8e1c-151">関数アプリがデプロイされたら、ポータルの **[すべてのリソース]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-151">Once the function app is deployed, go to **All resources** in the portal.</span></span> <span data-ttu-id="f8e1c-152">関数アプリは、タイプ **[App Service]** にリストされ、指定した名前になります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-152">The function app will be listed with type **App Service** and has the name you gave it.</span></span> <span data-ttu-id="f8e1c-153">リストから関数アプリを選択して開きます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-153">Select the function app from the list to open it.</span></span>

    >[!TIP]
    ><span data-ttu-id="f8e1c-154">ポータルで目的の関数アプリが見つからない場合は、[ポータルでお気に入りに関数アプリを追加する](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite)方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-154">If you are having trouble finding your function apps in the portal, find out how to [add function apps to your favorites in the portal](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).</span></span>

## <a name="create-a-function"></a><span data-ttu-id="f8e1c-155">関数を作成する</span><span class="sxs-lookup"><span data-stu-id="f8e1c-155">Create a function</span></span>

<span data-ttu-id="f8e1c-156">関数アプリを用意できたので、ここでは関数を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-156">Now that we have a function app, it's time to create a function.</span></span> <span data-ttu-id="f8e1c-157">関数はトリガーによってアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-157">A function is activated through a trigger.</span></span> <span data-ttu-id="f8e1c-158">このモジュールでは、HTTP トリガーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-158">In this module, we'll use an HTTP trigger.</span></span>

<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> <span data-ttu-id="f8e1c-159">現時点では、このモジュールの演習は Azure Functions V1 を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-159">The exercises in this module currently work with Azure Functions V1.</span></span> <span data-ttu-id="f8e1c-160">以下の手順に慎重に従って、関数アプリで V1 ランタイム バージョンが使用されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-160">Please follow these steps carefully to make sure your function app uses the V1 runtime version.</span></span> 

1. <span data-ttu-id="f8e1c-161">**[Function Apps]** リストで、関数アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-161">Select your function app in the **Function Apps** list.</span></span>
1. <span data-ttu-id="f8e1c-162">**[プラットフォーム機能]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-162">Select **Platform features**.</span></span>
1. <span data-ttu-id="f8e1c-163">**[プラットフォーム機能]** 画面の **[全般設定]** で、**[Function App の設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-163">In the **Platform features** screen, select **Function app settings** under **General Settings**.</span></span>
1. <span data-ttu-id="f8e1c-164">**[ランタイム バージョン]** で *[~1]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-164">Select *~1* in the **Runtime version** .</span></span>
1. <span data-ttu-id="f8e1c-165">**[Function App の設定]** を閉じます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-165">Close **Function app settings**.</span></span>

<span data-ttu-id="f8e1c-166">これで、Azure Functions V1 ランタイムを使用するように関数アプリが構成されました。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-166">Our function app is now configured to use the Azure Functions V1 runtime.</span></span> <span data-ttu-id="f8e1c-167">最初の関数の作成を続行できます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-167">We can now continue to create our first function.</span></span>
<!-- End temporary fix for issue #2498. --> 
1. <span data-ttu-id="f8e1c-168">**[関数]** の横にある [追加] (**+**) ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-168">Select the Add (**+**) button next to **Functions**.</span></span> <span data-ttu-id="f8e1c-169">この操作により関数作成プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-169">This action starts the function creation process.</span></span> 

1. <span data-ttu-id="f8e1c-170">**[すぐに使用を開始する]** ページで、**[関数を独自に作成する]** セクションの **[カスタム関数]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-170">On the **Get started quickly** page, select **Custom function** under the **Get started on your own** section.</span></span>

1. <span data-ttu-id="f8e1c-171">これにより、すべてのテンプレートが一覧表示されます。一覧から **[HTTP トリガー]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-171">This will list all the templates Select **HTTP Trigger** template from the list.</span></span>

1. <span data-ttu-id="f8e1c-172">**[新しい関数]** ブレードで、必要に応じて名前を変更し、**[認証レベル]** を _[関数]_ にして **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-172">On the **New Function** blade, change the name if you want, leave the **Authorization level** as _Function_, and click **Create**.</span></span>

1. <span data-ttu-id="f8e1c-173">新しい関数で、右上の **[</> 関数の URL の取得]** リンクをクリックし、**[既定値 (関数キー)]** を選択して、**[コピー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-173">In your new function, click the **</> Get function URL** link at the top right, select **default (Function key)**, and then select **Copy**.</span></span>

1. <span data-ttu-id="f8e1c-174">コピーした関数 URL をブラウザーの新しいタブのアドレス バーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-174">Paste the function URL you copied into the address bar of a new tab in your browser.</span></span>

1. <span data-ttu-id="f8e1c-175">この URL の末尾にクエリ文字列値 `&name=Azure` を追加し、キーボードの Enter キーを押して要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-175">Add the query string value `&name=Azure` to the end of this URL, and then press Enter on your keyboard to execute the request.</span></span> <span data-ttu-id="f8e1c-176">関数から次のような応答が返され、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-176">You should see a response similar to the following response returned by the function displayed in your browser.</span></span>

    ```output
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Hello Azure</string>
    ```

<span data-ttu-id="f8e1c-177">これまでの演習からわかるように、関数を作成するときはトリガーの種類を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-177">As you can see from this exercise so far, you have to select a trigger type when you create a function.</span></span> <span data-ttu-id="f8e1c-178">各関数には、トリガーが 1 つだけあります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-178">Every function has one and only one trigger.</span></span> <span data-ttu-id="f8e1c-179">この例では、HTTP トリガーを使用しています。つまり、この関数は HTTP 要求を受信すると起動します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-179">In this example, we're using an HTTP trigger, which means that our function starts when it receives an HTTP request.</span></span> <span data-ttu-id="f8e1c-180">次の JavaScript のスクリーンショットに示す既定の実装では、クエリ文字列または要求の本文で受信したパラメーター*名*の値で応答します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-180">The default implementation, shown in the following screenshot in JavaScript, responds with the value of the parameter *name* it received in the query string or body of the request.</span></span> <span data-ttu-id="f8e1c-181">文字列が指定されていない場合、関数は呼び出し元に対して名前値を指定するよう求めるメッセージで応答します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-181">If no string was provided, the function responds with a message that asks whomever is calling to supply a name value.</span></span>

![HTTP によってトリガーされる Azure 関数の既定の JavaScript 実装](../media/3-default-http-trigger-implementation-small.PNG)

<span data-ttu-id="f8e1c-183">このコードはすべて、この関数のフォルダー内の **index.js** ファイルにあります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-183">All of this code is in the **index.js** file in this function's folder.</span></span> <span data-ttu-id="f8e1c-184">では、関数のもう 1 つのファイルである **function.json** 構成ファイルを簡単に見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-184">Let's look briefly at the function's other file, the **function.json** config file.</span></span> <span data-ttu-id="f8e1c-185">次の JSON の一覧に、この構成データが示されています。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-185">This configuration data is shown in the following JSON listing.</span></span>

```json
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

<span data-ttu-id="f8e1c-186">ご覧のように、この関数には、`httpTrigger` 型の **req** という名前のトリガー バインディングと、`HTTP` 型の **res** という名前の出力バインディングが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-186">As you can see, this function has a trigger binding named **req** of type `httpTrigger` and an output binding named **res**  of type `HTTP`.</span></span> <span data-ttu-id="f8e1c-187">この関数の前述のコードでは、**req** パラメーターを使用して着信 HTTP 要求のペイロードにアクセスする方法を確認しました。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-187">In the preceding code for our function, we saw how we accessed the payload of the incoming HTTP request through our **req** parameter.</span></span> <span data-ttu-id="f8e1c-188">同様に、**res** パラメーターを設定するだけで HTTP 応答を送信しました。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-188">Similarly, we sent an HTTP response simply by setting our **res** parameter.</span></span> <span data-ttu-id="f8e1c-189">このようにバインディングを使用すると、面倒な作業の一部が自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-189">Bindings really do take care of some of the heavy lifting for us.</span></span>

>[!TIP]
><span data-ttu-id="f8e1c-190">Azure portal で関数パネルの右側にある **[ファイルの表示]** メニューを展開すると、**index.js** ファイルと **function.json** ファイルを確認できます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-190">You can see the **index.js** and **function.json** files by expanding the **View Files** menu at the right of the function panel in the Azure portal.</span></span>

### <a name="explore-binding-types"></a><span data-ttu-id="f8e1c-191">バインディングの種類を確認する</span><span class="sxs-lookup"><span data-stu-id="f8e1c-191">Explore binding types</span></span>

1. <span data-ttu-id="f8e1c-192">関数エントリの下には、次のスクリーンショットに示す一連のメニュー項目があります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-192">Notice under the function entry there is a set of menu items as shown in the following screenshot.</span></span>

    ![[Function App] ブレードの関数の下にあるメニュー項目を示すスクリーンショット。](../media/3-func-menu-small.PNG)

1. <span data-ttu-id="f8e1c-194">[統合] メニュー項目を選択して、関数の統合タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-194">Select the Integrate menu item to open the integration tab for our function.</span></span> <span data-ttu-id="f8e1c-195">このユニットの手順に従うと、統合タブは次のスクリーンショットのようになるはずです。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-195">If you have been following along with this unit, the integrate tab should look very similar to the following screenshot.</span></span>

    ![統合の UI またはタブを示すスクリーンショット。](../media/3-func-integrate-tab-small.PNG)

    > [!NOTE]
    > <span data-ttu-id="f8e1c-197">スクリーンショットに示すように、トリガー バインディングと出力バインディングを既に定義しています。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-197">We have already defined a trigger and an output binding, as shown in the screenshot.</span></span> <span data-ttu-id="f8e1c-198">また、"_複数の_" トリガーを追加できないこともわかります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-198">You can see that we can't add more than _one_ trigger.</span></span> <span data-ttu-id="f8e1c-199">実際に、この関数のトリガーを変更するには、まずトリガーを削除してから、新しいトリガーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-199">In fact, to change the trigger for our function we would have to first delete the trigger and create a new one.</span></span> <span data-ttu-id="f8e1c-200">ただし、この UI の **[入力]** セクションと **[出力]** セクションには、複数の入力値を受け入れ、複数の出力値を出力できるように、バインディングを追加するためのプラス記号 (+) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-200">However, the **Inputs** and **Outputs** sections of this UI display a plus sign (+) to add more bindings so we can accept more than one input value and emit more than one output value.</span></span>

1. <span data-ttu-id="f8e1c-201">**[入力]** 列の **[+ 新しい入力]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-201">Select **+ New Input** under the **Inputs** column.</span></span> <span data-ttu-id="f8e1c-202">次のスクリーンショットに示すように、入力バインディングとして考えられる全種類の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-202">A list of all possible input binding types is displayed as shown in the following screenshot.</span></span>

    ![考えられる入力バインディングの一覧を示すスクリーンショット。](../media/3-func-input-bindings-selector-small.PNG)

   <span data-ttu-id="f8e1c-204">これらの各入力バインディングについて、およびそれらをソリューションでどのように使用できるかについて、よく検討してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-204">Take a moment to consider each of these input bindings and how you might use them in a solution.</span></span> <span data-ttu-id="f8e1c-205">選択肢は多数あります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-205">There are a lot to choose from.</span></span> <span data-ttu-id="f8e1c-206">サポートされるデータ ソースは常に増えているので、このモジュールを読むときまでに、この一覧が変更されている可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-206">This list might even have changed by the time you read this module, as we continue to support more data sources.</span></span>

1. <span data-ttu-id="f8e1c-207">このモジュールの後半で入力バインディングの追加に戻りますが、ここでは **[キャンセル]** を選択してこの一覧を閉じます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-207">We'll get back to adding input bindings later in the module but, for now, select **Cancel** to dismiss this list.</span></span>

1. <span data-ttu-id="f8e1c-208">**[出力]** 列で **[+ 新しい出力]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-208">Select **+ New Output** under the **Outputs** column.</span></span> <span data-ttu-id="f8e1c-209">次のスクリーンショットに示すように、使用可能な出力バインディングの全種類の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-209">A list of all possible output binding types is displayed as shown in the following screenshot.\\</span></span>

    ![使用可能な出力バインディングの一覧を示すスクリーンショット。](../media/3-func-output-bindings-selector-small.PNG)

   <span data-ttu-id="f8e1c-211">ご覧のように、使用できる出力バインディングの種類が複数あります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-211">As you can see, there are several output binding types at your disposal.</span></span> <span data-ttu-id="f8e1c-212">このモジュールの後半で出力バインディングの追加に戻りますが、ここでは **[キャンセル]** を選択してこの一覧を閉じます。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-212">We'll get back to adding output bindings later in the module but, for now, select **Cancel** to dismiss this list.</span></span>

<span data-ttu-id="f8e1c-213">ここまで、関数アプリを作成し、関数を追加する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-213">So far, we've learned how to create a function app and add a function to it.</span></span> <span data-ttu-id="f8e1c-214">HTTP 要求が行われたときに実行される単純な関数の実際の動作を確認しました。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-214">We've seen a simple function in action, one that runs when an HTTP request is made to it.</span></span> <span data-ttu-id="f8e1c-215">また、Azure portal UI、および関数で使用できる入力バインディングと出力バインディングの種類も確認しました。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-215">We've also explored the Azure portal UI and types of input and output binding that are available to our functions.</span></span> <span data-ttu-id="f8e1c-216">次のユニットでは、入力バインディングを使用してデータベースからテキストを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="f8e1c-216">In the next unit, we'll use an input binding to read text from a database.</span></span>