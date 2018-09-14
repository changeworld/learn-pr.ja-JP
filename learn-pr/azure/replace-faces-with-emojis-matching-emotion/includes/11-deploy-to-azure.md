<span data-ttu-id="0b501-101">これまでに、このモジュールでこうして、Slack を取得する_スラッシュ_ngrok を使用して、コンピューター上にトンネリングして localhost 上で実行中にのみ操作コマンド。</span><span class="sxs-lookup"><span data-stu-id="0b501-101">So far in this module, we've managed to get a Slack _slash_ command working but only while running on localhost and by tunnelling onto our computer using ngrok.</span></span> <span data-ttu-id="0b501-102">これは、開発に適しては、クラウドで実行するコードを解放します。</span><span class="sxs-lookup"><span data-stu-id="0b501-102">This is fine for development but to release we want our code to be running in the cloud.</span></span>

<span data-ttu-id="0b501-103">この講義で行う、Azure 関数をクラウドにデプロイし、コードの新しい運用バージョンを使用する slack の構成を更新します。</span><span class="sxs-lookup"><span data-stu-id="0b501-103">In this lecture, we are going to deploy our Azure Function to the cloud and update the slack configuration to use the new production version of our code.</span></span>

<span data-ttu-id="0b501-104">この講義の終わりまでには、次の説明は。</span><span class="sxs-lookup"><span data-stu-id="0b501-104">By the end of this lecture you will learn:</span></span>

- <span data-ttu-id="0b501-105">Azure での Azure Function App を作成する方法</span><span class="sxs-lookup"><span data-stu-id="0b501-105">How to create an Azure Function App on Azure</span></span>
- <span data-ttu-id="0b501-106">Visual Studio Code を使用して Azure にデプロイする方法</span><span class="sxs-lookup"><span data-stu-id="0b501-106">How to deploy to Azure using Visual Studio Code</span></span>
- <span data-ttu-id="0b501-107">反映する方法を運用環境にローカルの設定</span><span class="sxs-lookup"><span data-stu-id="0b501-107">How to propogate your local settings to production</span></span>

## <a name="create-an-azure-function-app-on-azure"></a><span data-ttu-id="0b501-108">Azure で Azure 関数アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="0b501-108">Create an Azure Function App on Azure</span></span>

<span data-ttu-id="0b501-109">VS Code と Azure 関数の拡張機能を使用して azure 関数を作成するでしょう。</span><span class="sxs-lookup"><span data-stu-id="0b501-109">We are going to be creating the azure function using VS Code and the Azure Function Extension.</span></span>

1. <span data-ttu-id="0b501-110">コマンド パレットを開いて<kbd>Ctrl</kbd>+<kbd>P</kbd>選択と`Azure Function: Deploy to Function App`します。</span><span class="sxs-lookup"><span data-stu-id="0b501-110">Open the command palette with <kbd>Ctrl</kbd>+<kbd>P</kbd> and select `Azure Function: Deploy to Function App`.</span></span>

   ![関数アプリをデプロイします。](/media-drafts/10.deploy-to-function-app.png)

2. <span data-ttu-id="0b501-112">現在のプロジェクトをアップロードしてください、それを選択することを確認することが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0b501-112">It asks you to confirm you want to upload the current project, go ahead and select that.</span></span>

   ![配置先のフォルダーを選択します。](/media-drafts/10.select-folder-to-deploy.png)

3. <span data-ttu-id="0b501-114">アプリに関連付けるサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="0b501-114">Choose the subscription you want to associate with the app.</span></span>

   <span data-ttu-id="0b501-115">Azure に慣れていない場合は、サブスクリプションが 1 つ、その接続を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0b501-115">If you are new to Azure you should have one subscription, pick that.</span></span>

4. <span data-ttu-id="0b501-116">選択して新しい function app の作成</span><span class="sxs-lookup"><span data-stu-id="0b501-116">Choose create new function app</span></span>

   ![新しい Function App を作成します。](/media-drafts/10.create-new-function-app.png)

5. <span data-ttu-id="0b501-118">アプリの名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="0b501-118">Pick a name for your app</span></span>

   <span data-ttu-id="0b501-119">名前を選択で、必要な値に表示されます。</span><span class="sxs-lookup"><span data-stu-id="0b501-119">It asks you to choose a name, call it whatever you want.</span></span> <span data-ttu-id="0b501-120">私を呼び出している`mojifier-slack-function-app`します。</span><span class="sxs-lookup"><span data-stu-id="0b501-120">I'm calling mine `mojifier-slack-function-app`.</span></span>

   ![アプリ名を選択します。](/media-drafts/10.choose-app-name.png)

6. <span data-ttu-id="0b501-122">新しいリソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="0b501-122">Create a new resource group</span></span>

   <span data-ttu-id="0b501-123">リソース グループは、1 つに、Azure で複数のサービスをどのようにグループ化します_アプリ_します。</span><span class="sxs-lookup"><span data-stu-id="0b501-123">Resource groups are how we group multiple services in Azure into one _app_.</span></span> <span data-ttu-id="0b501-124">新しいまたは既存の 1 つのことを既に作成して使用を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="0b501-124">You can create a new one or use an existing one you've already created.</span></span>

   <span data-ttu-id="0b501-125">その 1 つは、自動的に設定します名前は新しいを作成する場合は、を選択することもか、または別を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="0b501-125">If you create a new one, then It auto-populates a name for you, you can either select that or type another.</span></span>

7. <span data-ttu-id="0b501-126">選択_新しいストレージ アカウント作成_です。</span><span class="sxs-lookup"><span data-stu-id="0b501-126">Choose _create new storage account_.</span></span>

   <span data-ttu-id="0b501-127">関数アプリは、ストアのデータ ファイルとファイルをより永続的な場所のストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="0b501-127">A function app needs a storage account, a more permanent place to store data and files.</span></span> <span data-ttu-id="0b501-128">新しいまたは既存の 1 つのことを既に作成して使用を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="0b501-128">You can create a new one or use an existing one you've already created.</span></span>

   <span data-ttu-id="0b501-129">自動的に設定します。 名前選択するかを 1 つ入力できます。</span><span class="sxs-lookup"><span data-stu-id="0b501-129">It auto-populates a name for you; you can either select that or type another.</span></span>

8. <span data-ttu-id="0b501-130">リージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="0b501-130">Pick a region</span></span>

   <span data-ttu-id="0b501-131">関数アプリは、どこにします。</span><span class="sxs-lookup"><span data-stu-id="0b501-131">Your function app has to live somewhere.</span></span> <span data-ttu-id="0b501-132">またはお客様のユーザーに最も近い場所を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0b501-132">I recommend choosing a location that is closest to you or your users.</span></span> <span data-ttu-id="0b501-133">選択したいます `West Europe`</span><span class="sxs-lookup"><span data-stu-id="0b501-133">I'm picking `West Europe`</span></span>

   ![アプリ名を選択します。](/media-drafts/10.select-region.png)

9. <span data-ttu-id="0b501-135">完了すると、出力ウィンドウに表示 URL を完了すると同様に、アップロードのため待機します。</span><span class="sxs-lookup"><span data-stu-id="0b501-135">Wait for the upload to complete, once complete the output window shows a URL like so:</span></span>

```output
Deployment to "mojifier-slack-function-app" completed.
HTTP Trigger Urls:
  MojifyImage: https://mojifier-slack-function-app.azurewebsites.net/api/MojifyImage
```

## <a name="try-it-out"></a><span data-ttu-id="0b501-136">試してみる</span><span class="sxs-lookup"><span data-stu-id="0b501-136">Try it out</span></span>

<span data-ttu-id="0b501-137">この時点では、少なくともが予想される、`RespondToSlackCommand`に他のすべての依存関係に依存しないために、作業関数。</span><span class="sxs-lookup"><span data-stu-id="0b501-137">At this point, we should at least expect the `RespondToSlackCommand` function to be working since it doesn't rely on any other dependencies.</span></span>

<span data-ttu-id="0b501-138">参照してください。 `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`</span><span class="sxs-lookup"><span data-stu-id="0b501-138">Visit `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`</span></span>

<span data-ttu-id="0b501-139">すべてが動作してもする場合はブラウザーのウィンドウで返される JSON 次のように。</span><span class="sxs-lookup"><span data-stu-id="0b501-139">If all is working well then you should some JSON returned in the browser window like so:</span></span>

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "https://mojifier-slack.azurewebsites.net/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```

## <a name="upload-settings"></a><span data-ttu-id="0b501-140">アップロードの設定</span><span class="sxs-lookup"><span data-stu-id="0b501-140">Upload Settings</span></span>

<span data-ttu-id="0b501-141">一部の設定があることに注意してください。 `local.settings.json` cognitive services のキーと URL でした。</span><span class="sxs-lookup"><span data-stu-id="0b501-141">Remember we have some settings we put in `local.settings.json` these were the keys and URL for cognitive services.</span></span>

<span data-ttu-id="0b501-142">`local.settings.json` ローカル、そのことはありませんにコピーされます運用展開するときにまったくです。</span><span class="sxs-lookup"><span data-stu-id="0b501-142">`local.settings.json` is exactly that, local, it's never copied over to production when you deploy.</span></span>

<span data-ttu-id="0b501-143">通常は、ポータルに移動し、おそらく UI または使用して経由での設定で手動で追加する必要がありますには、 `func` CLI で、同じ操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="0b501-143">Usually, you would need to head over to the portal and perhaps manually add in the settings via the UI or use the `func` CLI to do the same.</span></span>

<span data-ttu-id="0b501-144">ただし、ため VS Code を使用していること使用できます Func の Azure 拡張機能とコマンド パレットのローカル設定をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="0b501-144">However, since we are using VS Code, we can use the Azure Func extension and the command palette to upload the local settings for us.</span></span>

1.  <span data-ttu-id="0b501-145">コマンド パレットを開いて<kbd>Ctrl</kbd>+<kbd>P</kbd>選択と`Azure Functions: Upload Local Settings`します。</span><span class="sxs-lookup"><span data-stu-id="0b501-145">Open the command palette with <kbd>Ctrl</kbd>+<kbd>P</kbd> and select `Azure Functions: Upload Local Settings`.</span></span>

    ![ローカル設定をアップロードします。](/media-drafts/10.upload-local-settings.png)

2.  <span data-ttu-id="0b501-147">選択 `local.settings.json`</span><span class="sxs-lookup"><span data-stu-id="0b501-147">Choose `local.settings.json`</span></span>

    ![ローカル設定を選択します。](/media-drafts/10.choose-localsettings.png)

3.  <span data-ttu-id="0b501-149">Function app に関連付けられているサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="0b501-149">Choose the subscription your function app is associated with.</span></span>

4.  <span data-ttu-id="0b501-150">アップロードする関数アプリを選択し、私が呼び出されます `mojifier-slack-function-app`</span><span class="sxs-lookup"><span data-stu-id="0b501-150">Select the function app you want to upload to, mine is called `mojifier-slack-function-app`</span></span>

5.  <span data-ttu-id="0b501-151">すべての設定を上書きすることが求め場合の選択します。 `No To All`</span><span class="sxs-lookup"><span data-stu-id="0b501-151">If it asks you to overwrite any settings, choose `No To All`</span></span>

    <span data-ttu-id="0b501-152">これは、アップロードのみを行う新しい設定は必要な運用環境の設定を開発から設定をオーバーライドするリスクがあるそれ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="0b501-152">This only uploads new settings which is what we want, otherwise there is a risk you override production settings with settings from development.</span></span>

    ![ローカル設定を選択します。](/media-drafts/10.choose-no-to-all.png)

## <a name="try-it-out"></a><span data-ttu-id="0b501-154">試してみる</span><span class="sxs-lookup"><span data-stu-id="0b501-154">Try it out</span></span>

<span data-ttu-id="0b501-155">今すぐすべて予想どおりに動作している場合、ようになりました MojifyImage エンドポイントは動作します。</span><span class="sxs-lookup"><span data-stu-id="0b501-155">Now if everything is working as expected then now the MojifyImage endpoint should be working.</span></span>

<span data-ttu-id="0b501-156">参照してください。 `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`</span><span class="sxs-lookup"><span data-stu-id="0b501-156">Visit `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`</span></span>

<span data-ttu-id="0b501-157">問題なく動作はすべてと、ウィンドウの mojified 画像が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0b501-157">If all is working well, then you should see the mojified image in the window.</span></span>

<span data-ttu-id="0b501-158">動作しない場合は、ここの問題のトラブルシューティングから再試行してください。</span><span class="sxs-lookup"><span data-stu-id="0b501-158">If it's not working, then try troubleshooting the issues here.</span></span>

## <a name="update-slack"></a><span data-ttu-id="0b501-159">Slack を更新します。</span><span class="sxs-lookup"><span data-stu-id="0b501-159">Update Slack</span></span>

<span data-ttu-id="0b501-160">指す新しい Slack での設定をアップデートできる関数をクラウドにデプロイされましたので_パブリック_URL。</span><span class="sxs-lookup"><span data-stu-id="0b501-160">Now we have deployed our Functions to the cloud we can update the settings in Slack to point to the new _public_ URL.</span></span>

1. <span data-ttu-id="0b501-161">パブリック URL を使用して、Slack の更新、`RespondToSlackCommand`します。</span><span class="sxs-lookup"><span data-stu-id="0b501-161">Update Slack, so it uses the public URL of your `RespondToSlackCommand`.</span></span>

![運用 URL での Slack を更新します。](/media-drafts/10.deploy-update-url.png)

> <span data-ttu-id="0b501-163">**注**</span><span class="sxs-lookup"><span data-stu-id="0b501-163">**NOTE**</span></span>
>
> <span data-ttu-id="0b501-164">更新する、実稼働に使用するコマンド_パブリック_のバージョン、`RespondToSlackCommand`でホストされています。 `*.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="0b501-164">I'm updating the command to use the production, _public_, version of the `RespondToSlackCommand` hosted on `*.azurewebsites.net`</span></span>

## <a name="try-it-out"></a><span data-ttu-id="0b501-165">試してみる</span><span class="sxs-lookup"><span data-stu-id="0b501-165">Try it out</span></span>

<span data-ttu-id="0b501-166">すべてが正しく構成されていると、Slack がローカルのアプリではなく、デプロイされたアプリからデータを要求することを確認するには、オフにできるように`ngrok`今のところです。</span><span class="sxs-lookup"><span data-stu-id="0b501-166">To check that everything is configured correctly and that Slack requests data from the deployed app rather than the local app, let switch off `ngrok` for now.</span></span>

<span data-ttu-id="0b501-167">Slack ウィンドウを好きな slack スラッシュ コマンドを入力し、ヘッドします。</span><span class="sxs-lookup"><span data-stu-id="0b501-167">Then head to slack window and type your favourite slack slash command.</span></span>

`/mojify <image>`

<span data-ttu-id="0b501-168">かどうかは、すべての作業が想定どおり slack のウィンドウに表示されるイメージが表示されます、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="0b501-168">If it's all working as expected then we should see an image appear in the slack window like so:</span></span>

![Win](/media-drafts/10.publish-success.png)
