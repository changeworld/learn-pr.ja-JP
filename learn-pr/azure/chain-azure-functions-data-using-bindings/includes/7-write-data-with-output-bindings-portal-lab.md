<span data-ttu-id="afe10-101">前回の演習では、Azure Cosmos DB データベース内のブックマークを検索するシナリオを実装しました。</span><span class="sxs-lookup"><span data-stu-id="afe10-101">In our last exercise, we implemented a scenario to look up bookmarks in an Azure Cosmos DB database.</span></span> <span data-ttu-id="afe10-102">このブックマークのコレクションからデータを読み取ることができるように入力バインディングを構成しました。</span><span class="sxs-lookup"><span data-stu-id="afe10-102">We configured an input binding to read data from our bookmarks collection.</span></span> <span data-ttu-id="afe10-103">しかし、データを読み取るだけではつまらないので、もっと他のこともやってみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-103">But just reading data is boring, so let's do more.</span></span> <span data-ttu-id="afe10-104">書き込みも行うシナリオに拡張しましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-104">Let's expand the scenario to include writing.</span></span> <span data-ttu-id="afe10-105">次のフローチャートについて考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-105">Consider the following flowchart.</span></span>

![この Cosmos DB バックエンドにブックマークを追加するプロセスを示すフロー図](../media-draft/add-bookmark-flow-small.png)

<span data-ttu-id="afe10-107">このシナリオでは、リストへのブックマークの追加を求める要求を受信します。</span><span class="sxs-lookup"><span data-stu-id="afe10-107">In this scenario, we'll receive requests to add bookmarks to our list.</span></span> <span data-ttu-id="afe10-108">この要求によって目的のキーまたは ID が、ブックマーク URL と共に渡されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-108">The requests pass in the desired key, or ID, along with the bookmark URL.</span></span> <span data-ttu-id="afe10-109">フロー チャート内で確認できるように、そのキーがバックエンド内に既に存在する場合はエラーで応答します。</span><span class="sxs-lookup"><span data-stu-id="afe10-109">As you can see in the flow chart, we'll respond with an error if the key already exists in our back-end.</span></span>

<span data-ttu-id="afe10-110">渡されたキーが*見つからない*場合は、新しいブックマークをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="afe10-110">If the key that was passed to us is *not* found, we'll add the new bookmark to our database.</span></span> <span data-ttu-id="afe10-111">それで終わりにしてもかまいませんが、もう少しやってみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-111">We could stop there, but let's do a little more.</span></span>

<span data-ttu-id="afe10-112">フローチャート内の別の手順に気付きましたか。</span><span class="sxs-lookup"><span data-stu-id="afe10-112">Notice another step in the flowchart?</span></span> <span data-ttu-id="afe10-113">これまで、処理の観点から受信したデータに対して大したことは行っていません。</span><span class="sxs-lookup"><span data-stu-id="afe10-113">So far we haven't done much with the data that we receive in terms of processing.</span></span> <span data-ttu-id="afe10-114">受信した内容をデータベースに移動します。</span><span class="sxs-lookup"><span data-stu-id="afe10-114">We move what we receive into a database.</span></span> <span data-ttu-id="afe10-115">ただし、実際のソリューションでは、おそらくそのデータを何らかの方法で処理することが可能です。</span><span class="sxs-lookup"><span data-stu-id="afe10-115">However, in a real solution, it is possible that we'd probably process the data in some fashion.</span></span> <span data-ttu-id="afe10-116">同じ関数ですべての処理を実行することもできますが、このラボでは、追加の処理を別のコンポーネントまたはビジネス ロジックの一部にオフロードするパターンを示します。</span><span class="sxs-lookup"><span data-stu-id="afe10-116">We can decide to do all processing in the same function, but in this lab we'll show a pattern that offloads further processing to another component or piece of business logic.</span></span>

<span data-ttu-id="afe10-117">このブックマーク シナリオでのこの作業のオフロードの良い例として、どのようなことが考えられるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="afe10-117">What might be a good example of this offloading of work in our bookmarks scenario?</span></span> <span data-ttu-id="afe10-118">では、新しいブックマークを QR コード生成サービスに送信するというのはどうでしょうか。</span><span class="sxs-lookup"><span data-stu-id="afe10-118">Well what if we send the new bookmark to a QR code generation service?</span></span> <span data-ttu-id="afe10-119">そのサービスの場合は、さらに、URL の QR コードが生成され、イメージが BLOB ストレージに格納され、qr イメージのアドレスがブックマーク コレクション内のエントリに再び追加されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-119">That service would, in turn, generate a QR code for the URL, store the image in blob storage, and add the address of the qr image back into the entry in our bookmarks collection.</span></span> <span data-ttu-id="afe10-120">qr イメージを生成するサービスの呼び出しには時間がかかります。そのため、結果を待つのでなく、関数にサービスを渡して、その関数で非同期的に対処するようにします。</span><span class="sxs-lookup"><span data-stu-id="afe10-120">Calling a service to generate a qr image is time consuming so, rather than wait for the result, we hand it off to a function and let it take care of this asynchronously.</span></span>

<span data-ttu-id="afe10-121">Azure Functions では、さまざまな統合ソースに対して入力バインディングがサポートされているのと同様に、出力バインディング用のテンプレートのセットも用意されています。そのため、お客様はデータ ソースへのデータの書き込みを容易に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="afe10-121">Just as Azure Functions supports input bindings for different integration sources, it also has a set of output bindings templates to make it easy for you to write data to data sources.</span></span> <span data-ttu-id="afe10-122">出力バインディングはまた、*function.json* ファイルでも構成されています。</span><span class="sxs-lookup"><span data-stu-id="afe10-122">Output bindings are also configured in the *function.json* file.</span></span>  <span data-ttu-id="afe10-123">この演習で示すように、複数のデータ ソースおよびサービスを操作するように関数を構成できます。</span><span class="sxs-lookup"><span data-stu-id="afe10-123">As we'll see in this exercise, we can configure our function to work with multiple data sources and services.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="afe10-124">この演習は、前回のユニットでの演習を基に作成されています、具体的には、前回と同じ Azure Cosmos DB データベースと入力バインディングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-124">This exercise builds on the exercise in the last unit, namely, it uses the same Azure Cosmos DB database and input binding.</span></span> <span data-ttu-id="afe10-125">そのユニットを終えていない場合は、それを行ってからこのラボに進むことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="afe10-125">If you haven't worked through that unit, we recommend doing so before proceeding  with this lab.</span></span>

## <a name="create-an-httptriggered-function"></a><span data-ttu-id="afe10-126">HTTP によってトリガーされる関数を作成する</span><span class="sxs-lookup"><span data-stu-id="afe10-126">Create an HTTP_triggered Function</span></span>

1. <span data-ttu-id="afe10-127">このモジュールで使用したのと同じ Azure アカウントによって Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) にサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="afe10-127">Make sure you are signed in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with the same Azure account you've used throughout this module.</span></span>

2. <span data-ttu-id="afe10-128">Azure portal で、このモジュールで作成した関数アプリに移動します。</span><span class="sxs-lookup"><span data-stu-id="afe10-128">In the Azure portal, navigate to the function app you created in this module.</span></span>

3. <span data-ttu-id="afe10-129">ご利用の関数アプリを展開し、関数のコレクションをポイントし、**[関数]** の横にある [追加] (**+**) ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-129">Expand your function app, then hover over the functions collection and select the Add (**+**) button next to **Functions**.</span></span> <span data-ttu-id="afe10-130">この操作により関数作成プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-130">This action starts the function creation process.</span></span> <span data-ttu-id="afe10-131">次のアニメーションで、この操作を示します。</span><span class="sxs-lookup"><span data-stu-id="afe10-131">The following animation illustrates this action.</span></span>

![ユーザーが [関数] メニュー項目をポイントしたときにプラス記号が表示されるというアニメーション。](../media-draft/func-app-plus-hover-small.gif)

4. <span data-ttu-id="afe10-133">ページのサポートされているトリガーの現在のセットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-133">The page shows us the current set of supported triggers.</span></span> <span data-ttu-id="afe10-134">**[HTTP トリガー]** (次のスクリーン ショットの最初のエントリ) を選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-134">Select **HTTP trigger**, which is the first entry in the following screenshot.</span></span>

![トリガー テンプレートの選択 UI の一部を示すスクリーン ショット。イメージの左上に TTP トリガーが最初に表示されている。](../media-draft/trigger-templates-small.PNG)

5. <span data-ttu-id="afe10-136">右側に表示された **[新しい関数]** ダイアログに、次の値を使用して入力します。</span><span class="sxs-lookup"><span data-stu-id="afe10-136">Fill out the **New Function** dialog that appears to the right  using the following values.</span></span>

|<span data-ttu-id="afe10-137">フィールド</span><span class="sxs-lookup"><span data-stu-id="afe10-137">Field</span></span>  |<span data-ttu-id="afe10-138">値</span><span class="sxs-lookup"><span data-stu-id="afe10-138">Value</span></span>  |
|---------|---------|
|<span data-ttu-id="afe10-139">言語</span><span class="sxs-lookup"><span data-stu-id="afe10-139">Language</span></span>     | <span data-ttu-id="afe10-140">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="afe10-140">**JavaScript**</span></span>        |
|<span data-ttu-id="afe10-141">名前</span><span class="sxs-lookup"><span data-stu-id="afe10-141">Name</span></span>     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
| <span data-ttu-id="afe10-142">承認レベル</span><span class="sxs-lookup"><span data-stu-id="afe10-142">Authorization level</span></span> | <span data-ttu-id="afe10-143">**関数**</span><span class="sxs-lookup"><span data-stu-id="afe10-143">**Function**</span></span> |

5. <span data-ttu-id="afe10-144">**[作成]** を選択して関数を作成します。これにより、コード エディターで index.js ファイルが開き、HTTP によってトリガーされる関数の既定の実装が表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-144">Select **Create** to create our function, which opens the index.js file in the code editor and displays a default implementation of the HTTP-triggered function.</span></span>

<span data-ttu-id="afe10-145">この演習では、出発点で前回のユニットからの*コード*および*構成*を使用することによりスピードアップを図ります。</span><span class="sxs-lookup"><span data-stu-id="afe10-145">In this exercise, we'll speed up things by using the *code* and *configuration* from the previous unit as a starting point.</span></span>

6. <span data-ttu-id="afe10-146">index.js 内のすべてのコードを次のスニペットからのコードに置換します。**[保存]** をクリックして、この変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="afe10-146">Replace all code in index.js with the code from the following snippet and click **Save** to save this change.</span></span> 

[!code-javascript[](../code/find-bookmark-single.js)]

<span data-ttu-id="afe10-147">このコードに馴染みがある場合、それはご提供している [!INCLUDE [func-name-find](./func-name-find.md)] 関数の実装だからです。</span><span class="sxs-lookup"><span data-stu-id="afe10-147">If this code looks familiar, that's because it's the implementation of our [!INCLUDE [func-name-find](./func-name-find.md)] function.</span></span> <span data-ttu-id="afe10-148">予想されるとおり、同じバインディングが定義されるまで、関数は機能しません。</span><span class="sxs-lookup"><span data-stu-id="afe10-148">As you would expect, the function won't work until we define the same bindings.</span></span>  

7. <span data-ttu-id="afe10-149">[!INCLUDE [func-name-find](./func-name-find.md)] 関数から *function.json* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-149">Open the *function.json* file from the [!INCLUDE [func-name-find](./func-name-find.md)] function.</span></span> <span data-ttu-id="afe10-150">そのファイルを見つけるには、コード エディターの右側にある **[ファイルの表示]** メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-150">You'll find it by opening the **View files** menu to the right of the code editor.</span></span>

8. <span data-ttu-id="afe10-151">このファイルの内容全体をコピーします。</span><span class="sxs-lookup"><span data-stu-id="afe10-151">Copy the entire contents of this file.</span></span>

9. <span data-ttu-id="afe10-152">[!INCLUDE [func-name-add](./func-name-add.md)] 関数から *function.json* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-152">Open the *function.json* file from the [!INCLUDE [func-name-add](./func-name-add.md)] function.</span></span>

10. <span data-ttu-id="afe10-153">このファイルの内容を、[!INCLUDE [func-name-find](./func-name-find.md)] 関数に関連付けられた *function.json* ファイルからコピーした内容に置換します。</span><span class="sxs-lookup"><span data-stu-id="afe10-153">Replace the contents of this file with the content you copied from the *function.json* file associated with the [!INCLUDE [func-name-find](./func-name-find.md)] function.</span></span> <span data-ttu-id="afe10-154">完了したら、ご自分の function.json に次の JSON が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="afe10-154">When you're done, your function.json should contain the following JSON.</span></span>

```json
{
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
    },
    {
      "type": "documentDB",
      "name": "bookmark",
      "databaseName": "func-io-learn-db",
      "collectionName": "Bookmarks",
      "connection": "unit3test_DOCUMENTDB",
      "direction": "in",
      "id": "{id}"
    }
  ],
  "disabled": false
}
```

11. <span data-ttu-id="afe10-155">必ず **[保存]** を選択してすべての変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="afe10-155">Make sure to **Save** all changes.</span></span>

<span data-ttu-id="afe10-156">前の手順では、新しい関数のバインディングを、別の関数からバインディング定義をコピーすることによって構成しました。</span><span class="sxs-lookup"><span data-stu-id="afe10-156">In the preceding steps, we configured bindings for our new function by copying binding definitions from another.</span></span> <span data-ttu-id="afe10-157">もちろん、UI を介して新しいバインディングを作成することもできました。しかし、上記の方法をご利用いただけるということを理解することが重要です。</span><span class="sxs-lookup"><span data-stu-id="afe10-157">We could, of course, created a new binding through the UI, but it is good to understand that this alternative is available to you.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="afe10-158">試してみる</span><span class="sxs-lookup"><span data-stu-id="afe10-158">Try it out</span></span>

1. <span data-ttu-id="afe10-159">通常は、右上の **[</> 関数の URL の取得]** をクリックし、**[既定値 (関数キー)]** を選択して、**[コピー]** をクリックすることで、関数の URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="afe10-159">As usual, click **</> Get function URL** at the top right, select **default (Function key)**, and then click **Copy** to copy the function's URL.</span></span>

2. <span data-ttu-id="afe10-160">コピーした関数 URL をご利用のブラウザーのアドレス バーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="afe10-160">Paste the function URL you copied into your browser's address bar.</span></span> <span data-ttu-id="afe10-161">この URL の末尾にクエリ文字列 `&id=docs` を追加し、キーボードで `Enter` キーを押して要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="afe10-161">Add the query string value `&id=docs` to the end of this URL and press the `Enter` key on your keyboard to execute the request.</span></span> <span data-ttu-id="afe10-162">すべて順調であれば、そのリソースへの URL を含む応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-162">All going well, you should see a response that includes a URL to that resource.</span></span>

<span data-ttu-id="afe10-163">さて、どこまできたでしょうか。</span><span class="sxs-lookup"><span data-stu-id="afe10-163">So, where are we at?</span></span> <span data-ttu-id="afe10-164">これまで、実際には前回のラボで作成した内容をレプリケートしただけでした。</span><span class="sxs-lookup"><span data-stu-id="afe10-164">Well, so far we've really just replicated what we did in the previous lab.</span></span> <span data-ttu-id="afe10-165">しかし、それは問題ありません。</span><span class="sxs-lookup"><span data-stu-id="afe10-165">But that's ok.</span></span> <span data-ttu-id="afe10-166">前回のラボでの処理内容をコピーして、このラボの出発点として活用します。</span><span class="sxs-lookup"><span data-stu-id="afe10-166">We're copying what we did in the last lab to serve as a starting point for this one.</span></span> <span data-ttu-id="afe10-167">次に新しい機能を操作します。具体的には、データベースへの書き込みです。</span><span class="sxs-lookup"><span data-stu-id="afe10-167">We'll work on the new stuff next, namely, writing to our database.</span></span> <span data-ttu-id="afe10-168">そのため、*出力バインディング*が必要です。</span><span class="sxs-lookup"><span data-stu-id="afe10-168">For that, we'll need an *output binding*.</span></span>

## <a name="define-azure-cosmos-db-output-binding"></a><span data-ttu-id="afe10-169">Azure Cosmos DB の出力バインディングを定義する</span><span class="sxs-lookup"><span data-stu-id="afe10-169">Define Azure Cosmos DB output binding</span></span>

<span data-ttu-id="afe10-170">ユーザー インターフェイスを経由して新しい出力バインディングを定義するのでなく、構成ファイル *function.json* を手動で更新してこのバインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="afe10-170">Rather than define a new output binding by going through the user interface, we'll create this binding by updating the configuration file, *function.json*, by hand.</span></span> 

1. <span data-ttu-id="afe10-171">エディター内でこの関数用の **function.json** ファイルを開くには **[ファイルの表示]** リストでそれを選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-171">Open the **function.json** file for this function in the editor by selecting it in the **View files** list.</span></span>

2. <span data-ttu-id="afe10-172">`bookmark` という名前のバインディングをそのファイルにコピーします。</span><span class="sxs-lookup"><span data-stu-id="afe10-172">Copy the binding with the name `bookmark` in that file.</span></span>

3. <span data-ttu-id="afe10-173">閉じ中かっこのすぐ後、閉じ角かっこの直前にカーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-173">Place your cursor directly after the closing curly bracket, right before the closing square bracket.</span></span> <span data-ttu-id="afe10-174">コンマ `,` を追加し、バインディングのコピーをここに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="afe10-174">Add a comma `,` and then paste the copy of the binding here.</span></span> <span data-ttu-id="afe10-175">ご利用の *function.json* 構成ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="afe10-175">Your *function.json* config should now look like the following.</span></span>

[!code-json[](../code/config-new-entry.json?highlight=22-31)]

4. <span data-ttu-id="afe10-176">貼り付けたバインディングを編集して、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="afe10-176">Edit the binding we pasted, with the following changes.</span></span>


|<span data-ttu-id="afe10-177">プロパティ</span><span class="sxs-lookup"><span data-stu-id="afe10-177">Property</span></span>   |<span data-ttu-id="afe10-178">古い値</span><span class="sxs-lookup"><span data-stu-id="afe10-178">Old value</span></span>  |<span data-ttu-id="afe10-179">新しい値</span><span class="sxs-lookup"><span data-stu-id="afe10-179">New value</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="afe10-180">name</span><span class="sxs-lookup"><span data-stu-id="afe10-180">name</span></span>     |   <span data-ttu-id="afe10-181">bookmark</span><span class="sxs-lookup"><span data-stu-id="afe10-181">bookmark</span></span>      |  <span data-ttu-id="afe10-182">**newbookmark**</span><span class="sxs-lookup"><span data-stu-id="afe10-182">**newbookmark**</span></span>       |
|<span data-ttu-id="afe10-183">direction</span><span class="sxs-lookup"><span data-stu-id="afe10-183">direction</span></span>     |   <span data-ttu-id="afe10-184">in</span><span class="sxs-lookup"><span data-stu-id="afe10-184">in</span></span>      |   <span data-ttu-id="afe10-185">**out**</span><span class="sxs-lookup"><span data-stu-id="afe10-185">**out**</span></span>      |
|<span data-ttu-id="afe10-186">id</span><span class="sxs-lookup"><span data-stu-id="afe10-186">id</span></span>     |      <span data-ttu-id="afe10-187">{id}</span><span class="sxs-lookup"><span data-stu-id="afe10-187">{id}</span></span>   |   <span data-ttu-id="afe10-188">**このプロパティを削除します。それは出力バインディングには存在しません。**</span><span class="sxs-lookup"><span data-stu-id="afe10-188">**delete this property. It does not exist for the output binding.**</span></span>      |

<span data-ttu-id="afe10-189">そのような変更を行った場合、次の JSON のようなファイルができあがります。</span><span class="sxs-lookup"><span data-stu-id="afe10-189">When you make these changes, you end up with a file that looks like the following JSON.</span></span>

[!code-json[](../code/config-q-complete.json?highlight=22-30)]

<span data-ttu-id="afe10-190">それは、どうすれば構成ファイルでもバインディングを直接作成することができるかを示すデモにすぎませんでした。</span><span class="sxs-lookup"><span data-stu-id="afe10-190">That was just a demo of how you can also create bindings directly in the configuration file.</span></span> <span data-ttu-id="afe10-191">この例では、別のバインディングからのプロパティを再利用しているので、それは理にかなっています。具体的には、Cosmos DB 入力バインディング用に既に構成してある `databaseName`、`collectionName`、および `connection` です。</span><span class="sxs-lookup"><span data-stu-id="afe10-191">In this example, it makes sense because we are reusing the properties from another binding, namely, the `databaseName`, `collectionName` and `connection` that we already configured for our Cosmos DB input binding.</span></span>

> [!NOTE]
> <span data-ttu-id="afe10-192">上記の JSON ファイル内の `connection` の実際の値は、その接続が作成されたときそれに付けられた任意の名前となります。</span><span class="sxs-lookup"><span data-stu-id="afe10-192">The actual value of `connection` in the preceding JSON file will be whatever name your connection was given when it was created.</span></span>

<span data-ttu-id="afe10-193">コードを更新する前に、キューにメッセージを投稿できるようにするもう 1 つのバインディングを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-193">Before we update our code, let's add one more binding that will enable us to post messages to a queue.</span></span>

## <a name="define-azure-queue-storage-output-binding"></a><span data-ttu-id="afe10-194">Azure Queue Storage 出力バインディングを定義する</span><span class="sxs-lookup"><span data-stu-id="afe10-194">Define Azure Queue Storage output binding</span></span>

<span data-ttu-id="afe10-195">Azure Queue Storage は、世界中のどこからでもアクセスできるメッセージを格納するためのサービスです。</span><span class="sxs-lookup"><span data-stu-id="afe10-195">Azure Queue storage is a service for storing messages that can be accessed from anywhere in the world.</span></span> <span data-ttu-id="afe10-196">1 つのメッセージの最大サイズは 64 KB です。1 つのキューには、ストレージ アカウントの合計容量の上限に達するまで、数百万のメッセージを格納できます。合計容量の合計はストレージ アカウントで定義されています。</span><span class="sxs-lookup"><span data-stu-id="afe10-196">A single message can be up to 64 KB and a queue can contain millions of messages up to the total capacity limit of the storage account in which it is defined.</span></span> <span data-ttu-id="afe10-197">次の図に、このシナリオでのキューの使用方法の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="afe10-197">The following diagram shows at a high level how a queue will be used in our scenario.</span></span>

![ストレージ キューの概念と、キューにメッセージをプッシュおよびポップする 2 つの関数を示す図。](../media-draft/q-logical-small.png)

<span data-ttu-id="afe10-199">ここで、新しい関数 [!INCLUDE [func-name-add](./func-name-add.md)] によってキューにメッセージが追加されることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="afe10-199">Here we can see that our new function, [!INCLUDE [func-name-add](./func-name-add.md)], adds messages to a queue.</span></span> <span data-ttu-id="afe10-200">別の関数 (例えば、*gen-qr-code* と呼ばれる架空関数) では、同じキューからメッセージがポップされ、要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-200">Another function, for example a fictitious function called *gen-qr-code*, will pop messages from the same queue and process the request.</span></span>  <span data-ttu-id="afe10-201">[!INCLUDE [func-name-add](./func-name-add.md)] からメッセージをキューに書き込みまたは*プッシュ*するので、このソリューションに新しい出力バインディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="afe10-201">Since we write, or *push*, messages to the queue from [!INCLUDE [func-name-add](./func-name-add.md)], we'll add a new  output binding to our solution.</span></span> <span data-ttu-id="afe10-202">今度は UI を通してバインディングを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-202">Let's create the binding through the UI this time.</span></span>

1. <span data-ttu-id="afe10-203">左側にある関数メニューで **[統合]** を選択して、[統合] タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-203">Select **Integrate** in the function menu on the left to open the integration tab.</span></span>

2. <span data-ttu-id="afe10-204">**[出力]** 列で **[+ 新しい出力]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-204">Select **+ New Output** under the **Outputs** column.</span></span> <span data-ttu-id="afe10-205">使用可能なすべての種類の出力バインディングが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-205">A list of all possible output binding types is displayed.</span></span>

3. <span data-ttu-id="afe10-206">リストから **[Azure Queue Storage]** をクリックし、**[選択]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="afe10-206">Click on **Azure Queue Storage** from the list and then the **Select** button.</span></span> <span data-ttu-id="afe10-207">この操作によって、Azure Queue Storage 出力構成ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-207">This action opens the Azure Queue Storage output configuration page.</span></span>

<span data-ttu-id="afe10-208">次に、ストレージ アカウント接続を設定します。</span><span class="sxs-lookup"><span data-stu-id="afe10-208">Next, we'll set up a storage account connection.</span></span> <span data-ttu-id="afe10-209">これはキューがホストされる場所です。</span><span class="sxs-lookup"><span data-stu-id="afe10-209">This is where our queue will be hosted.</span></span>

4. <span data-ttu-id="afe10-210">このページの **[ストレージ アカウント接続]** という名前のフィールドで、空のフィールドの右側にある *[新規]* をクリックします。</span><span class="sxs-lookup"><span data-stu-id="afe10-210">In the field named **Storage account connection** on this page, click on *new* to the right of the empty field.</span></span> <span data-ttu-id="afe10-211">この操作により、**[ストレージ アカウント]** 選択ダイアログが開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-211">This action opens the **Storage Account** selection dialog.</span></span> 

5. <span data-ttu-id="afe10-212">このモジュールを開始して関数アプリを作成したとき、ストレージ アカウントも同時に作成されました。</span><span class="sxs-lookup"><span data-stu-id="afe10-212">When we started this module and created our function app, a storage account was also created at that time.</span></span> <span data-ttu-id="afe10-213">そのストレージ アカウントはこのダイアログに表示されるので、先に進んでそれを選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-213">It will be listed in this dialog, so go ahead and select it.</span></span> <span data-ttu-id="afe10-214">**[ストレージ アカウント接続]** フィールドには、接続の名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-214">The **Storage account connection** field is populated with the name of a connection.</span></span> <span data-ttu-id="afe10-215">接続文字列の値を表示したい場合は、**[値の表示]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="afe10-215">If you want to see the connection string value, click on **show value**.</span></span>

6. <span data-ttu-id="afe10-216">このページ上の他のフィールドはすべて既定値のままとしておくことができますが、プロパティに意味を加えるために次のように変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-216">Although we could leave all other fields on this page with their default values, let's change the following to lend more meaning to the properties.</span></span>


|<span data-ttu-id="afe10-217">プロパティ</span><span class="sxs-lookup"><span data-stu-id="afe10-217">Property</span></span>  |<span data-ttu-id="afe10-218">古い値</span><span class="sxs-lookup"><span data-stu-id="afe10-218">Old value</span></span>  |<span data-ttu-id="afe10-219">新しい値</span><span class="sxs-lookup"><span data-stu-id="afe10-219">New value</span></span>  | <span data-ttu-id="afe10-220">説明</span><span class="sxs-lookup"><span data-stu-id="afe10-220">Description</span></span> |
|---------|---------|---------|---------|
|<span data-ttu-id="afe10-221">キュー名</span><span class="sxs-lookup"><span data-stu-id="afe10-221">Queue name</span></span>     |    <span data-ttu-id="afe10-222">outqueue</span><span class="sxs-lookup"><span data-stu-id="afe10-222">outqueue</span></span>     |  <span data-ttu-id="afe10-223">**bookmarks-post-process**</span><span class="sxs-lookup"><span data-stu-id="afe10-223">**bookmarks-post-process**</span></span>      | <span data-ttu-id="afe10-224">これは、使用しているキューの中で、ブックマークを別の関数でも処理できるようにブックマークの配置先とするキューの名前です。</span><span class="sxs-lookup"><span data-stu-id="afe10-224">This is the name of the queue we are using to place bookmarks into so that they can be processed further by another function.</span></span> |
| <span data-ttu-id="afe10-225">メッセージ パラメーター名</span><span class="sxs-lookup"><span data-stu-id="afe10-225">Message parameter name</span></span>    |  <span data-ttu-id="afe10-226">outputQueueItem</span><span class="sxs-lookup"><span data-stu-id="afe10-226">outputQueueItem</span></span>       |   <span data-ttu-id="afe10-227">**newmessage**</span><span class="sxs-lookup"><span data-stu-id="afe10-227">**newmessage**</span></span>      | <span data-ttu-id="afe10-228">これはコードで使用するバインディング プロパティです。</span><span class="sxs-lookup"><span data-stu-id="afe10-228">This is the binding property we'll use in code.</span></span> |


7. <span data-ttu-id="afe10-229">**[保存]** をクリックして変更を保存することを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="afe10-229">Remember to click **Save** to save your changes.</span></span>

## <a name="update-function-implementation"></a><span data-ttu-id="afe10-230">関数の実装を更新する</span><span class="sxs-lookup"><span data-stu-id="afe10-230">Update function implementation</span></span>

<span data-ttu-id="afe10-231">これで、[!INCLUDE [func-name-add](./func-name-add.md)] 関数に対するバインディングがすべて設定されました。</span><span class="sxs-lookup"><span data-stu-id="afe10-231">We now have all our bindings set up for the [!INCLUDE [func-name-add](./func-name-add.md)] function.</span></span> <span data-ttu-id="afe10-232">設定したバインディングをこの関数で使ってみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-232">It's time to use them in our function.</span></span>

1.  <span data-ttu-id="afe10-233">関数 [!INCLUDE [func-name-add](./func-name-add.md)] をクリックして、*index.js* をコード エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-233">Click on our function, [!INCLUDE [func-name-add](./func-name-add.md)], to open up *index.js* in the code editor.</span></span>

2. <span data-ttu-id="afe10-234">index.js 内のすべてのコードを次のスニペットのコードに置換します。</span><span class="sxs-lookup"><span data-stu-id="afe10-234">Replace all code in index.js with the code from the following snippet.</span></span>

[!code-javascript[](../code/add-bookmark.js)]

<span data-ttu-id="afe10-235">このコードで何が行われるのかを分析してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-235">Let's breakdown what this code does.</span></span>

* <span data-ttu-id="afe10-236">この関数によってデータが変更されるので、HTTP 要求が POST になり、ブックマーク データが要求本文の一部になると想定しています。</span><span class="sxs-lookup"><span data-stu-id="afe10-236">Since this function changes our data, we expect the HTTP request to be a POST and the bookmark data to be part of the request body.</span></span>
* <span data-ttu-id="afe10-237">Cosmos DB の入力バインディングでは、受信している `id` を使用してドキュメントまたはブックマークの取得が試みられます。</span><span class="sxs-lookup"><span data-stu-id="afe10-237">Our Cosmos DB input binding attempts to retrieve a document, or bookmark, using the `id` that we receive.</span></span> <span data-ttu-id="afe10-238">エントリが見つかると、`bookmark` オブジェクトが設定されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-238">If it finds an entry, the `bookmark` object will be set.</span></span> <span data-ttu-id="afe10-239">`if(bookmark)` 条件では、エントリが見つかったかどうかが確認されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-239">The `if(bookmark)` condition checks whether an entry was found.</span></span>
* <span data-ttu-id="afe10-240">データベースへの追加は、`context.bindings.newbookmark` バインディング パラメーターを、JSON 文字列として作成した新しいブックマーク エントリに設定するだけで、簡単に行えます。</span><span class="sxs-lookup"><span data-stu-id="afe10-240">Adding to the database is a simple as setting the `context.bindings.newbookmark` binding parameter to the new bookmark entry, which we have created as a JSON string.</span></span>
* <span data-ttu-id="afe10-241">キューへのメッセージの投稿は、`context.bindings.newmessage parameter` を設定するだけで簡単に行えます。</span><span class="sxs-lookup"><span data-stu-id="afe10-241">Posting a message to our queue is as simple as setting the  `context.bindings.newmessage parameter`.</span></span>

> [!NOTE]
> <span data-ttu-id="afe10-242">行ったタスクは、キュー バインディングを作成することだけでした。</span><span class="sxs-lookup"><span data-stu-id="afe10-242">The only task we performed was to create a queue binding.</span></span> <span data-ttu-id="afe10-243">キューの作成は明示的に行っていません。</span><span class="sxs-lookup"><span data-stu-id="afe10-243">We never created the queue explicitly.</span></span> <span data-ttu-id="afe10-244">バインディングの機能をご確認ください。</span><span class="sxs-lookup"><span data-stu-id="afe10-244">You are witnessing the power of bindings!</span></span> <span data-ttu-id="afe10-245">次のコールアウトで示すように、キューは存在しない場合、自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-245">As the following callout says, the queue is automatically created for you if it doesn't exist!</span></span>

![キューが自動的に作成されることを示すコールアウトのスクリーンショット。](../media-draft/q-auto-create-small.png)

<span data-ttu-id="afe10-247">これで終了です。次のセクションで作業内容の動作を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-247">So, that's it - let's see our work in action in the next section.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="afe10-248">試してみる</span><span class="sxs-lookup"><span data-stu-id="afe10-248">Try it out</span></span>

<span data-ttu-id="afe10-249">出力バインディングが複数あるので、テストはやや複雑になります。</span><span class="sxs-lookup"><span data-stu-id="afe10-249">Now that we have multiple output bindings, our testing becomes a little trickier.</span></span> <span data-ttu-id="afe10-250">前のラボでは HTTP 要求とクエリ文字列を送信してテストを行っても問題なかったので、今度は HTTP Post を実行することにします。</span><span class="sxs-lookup"><span data-stu-id="afe10-250">Whereas in previous labs we were content to test by sending an HTTP request and a query string, we'll want to perform an HTTP Post this time.</span></span> <span data-ttu-id="afe10-251">また、メッセージがキューに入っているかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="afe10-251">We also need to check whether messages are making it into a queue.</span></span>

1.  <span data-ttu-id="afe10-252">Function Apps ポータルで関数 [!INCLUDE [func-name-add](./func-name-add.md)] が選択された状態で、左端にある [テスト] メニュー項目をクリックして関数を展開します。</span><span class="sxs-lookup"><span data-stu-id="afe10-252">With our function, [!INCLUDE [func-name-add](./func-name-add.md)], selected in the Function Apps portal, click on the Test menu item on the far left to expand it.</span></span>

2. <span data-ttu-id="afe10-253">**[テスト]** メニュー項目を選択し、テスト パネルが開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="afe10-253">Select the **Test** menu item and verify that you have the test panel open.</span></span> <span data-ttu-id="afe10-254">次のスクリーンショットでテスト パネルがどのように表示されるかを示します。</span><span class="sxs-lookup"><span data-stu-id="afe10-254">The following screenshot shows what it should look like.</span></span> 

![展開された関数のテスト パネルを示すスクリーン ショット](../media-draft/test-panel-open-small.png)

> [!IMPORTANT]
> <span data-ttu-id="afe10-256">HTTP メソッドのドロップダウンで、**[POST]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="afe10-256">Make sure **POST** is selected in the HTTP method dropdown.</span></span>

3. <span data-ttu-id="afe10-257">要求本文の内容を次の JSON ペイロードに置換します。</span><span class="sxs-lookup"><span data-stu-id="afe10-257">Replace the content of the request body with the following JSON payload.</span></span>

```json
  {
      "id": "docs",
      "url": "https://docs.microsoft.com/azure"
  }
  ```

4. <span data-ttu-id="afe10-258">テスト パネルの下部にある **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="afe10-258">Click **Run** at the bottom of the test panel.</span></span> 

5. <span data-ttu-id="afe10-259">*[出力]* ウィンドウに、次の図に示すような "ブックマークは既に存在します" </span><span class="sxs-lookup"><span data-stu-id="afe10-259">Verify that the *Output* window displays the "Bookmark already exists."</span></span> <span data-ttu-id="afe10-260">というメッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="afe10-260">message as shown in the following diagram.</span></span> 

![テスト パネルと失敗したテストの結果を示すスクリーン ショット。](../media-draft/test-exists-small.png)

6. <span data-ttu-id="afe10-262">ここで、要求本文を次のペイロードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="afe10-262">Now replace the Request body with the following payload.</span></span> 

```json
  {
      "id": "github",
      "URL": "https://www.github.com"
  }
  ```
7. <span data-ttu-id="afe10-263">テスト パネルの下部にある **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="afe10-263">Click **Run** at the bottom of the test panel.</span></span>

8. <span data-ttu-id="afe10-264">*[出力]* ボックスに、次の図に示すような "ブックマークが追加されました" というメッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="afe10-264">Verify the that *Output* box displays the "bookmark added" message as shown in the following diagram.</span></span>

![テスト パネルと成功したテストの結果を示すスクリーン ショット。](../media-draft/test-success-small.png)

<span data-ttu-id="afe10-266">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="afe10-266">Congratulations!</span></span> <span data-ttu-id="afe10-267">[!INCLUDE [func-name-add](./func-name-add.md)] は設計どおりに機能しますが、コード内に含めたクエリ動作についてはどうでしょうか。</span><span class="sxs-lookup"><span data-stu-id="afe10-267">The [!INCLUDE [func-name-add](./func-name-add.md)] works as designed, but what about that queue operation we had in the code?</span></span> <span data-ttu-id="afe10-268">キューに何か書き込まれたかどうかを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afe10-268">Well, let's go see if something was written to a queue.</span></span>

### <a name="verify-that-a-message-is-written-to-our-queue"></a><span data-ttu-id="afe10-269">キューにメッセージが書き込まれたことを確認する</span><span class="sxs-lookup"><span data-stu-id="afe10-269">Verify that a message is written to our queue</span></span>

<span data-ttu-id="afe10-270">Azure Queue Storage キューは、ストレージ アカウントでホストされています。</span><span class="sxs-lookup"><span data-stu-id="afe10-270">Azure Queue Storage queues are hosted in a storage account.</span></span> <span data-ttu-id="afe10-271">この演習では、出力バインディングの作成時に既にストレージ アカウントを選択しています。</span><span class="sxs-lookup"><span data-stu-id="afe10-271">You selected the storage account in this exercise  already when creating the output binding.</span></span> 

1. <span data-ttu-id="afe10-272">Azure portal 内のメインの検索ボックスに、「*ストレージ アカウント*」と入力し、検索結果で **[サービス]** カテゴリの下にある *[ストレージ アカウント]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-272">In the main search box in the Azure portal, type *storage accounts* and in the search results select **Storage accounts** under the *Services* category.</span></span> <span data-ttu-id="afe10-273">次のスクリーンショットにこれを示します。</span><span class="sxs-lookup"><span data-stu-id="afe10-273">This is illustrated in the following screenshot.</span></span> 

![メインの検索ボックスでの「ストレージ アカウント」に対する検索結果を示すスクリーンショット。](../media-draft/search-for-sa-small.png)

2. <span data-ttu-id="afe10-275">返されたストレージ アカウントのリスト内で、**newmessage** 出力バインディングを作成するために使用したストレージ アカウントを選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-275">In the list of storage accounts that are returned, select the storage account you used to create the **newmessage** output binding.</span></span> <span data-ttu-id="afe10-276">ストレージ アカウントの設定はポータルのメイン ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-276">The storage account settings are displayed in the main window the portal.</span></span>

3. <span data-ttu-id="afe10-277">サービス リストから **[キュー]** 項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="afe10-277">Select the **Queues** item from the Services list.</span></span> <span data-ttu-id="afe10-278">これによって、このストレージ アカウントでホストされているキューの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-278">This displays a list of queues hosted by this storage account.</span></span> <span data-ttu-id="afe10-279">次のスクリーン ショットに示すように、**bookmarks-post-process** キューが存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="afe10-279">Verify that the **bookmarks-post-process** queue exists, as shown in the following screenshot.</span></span>

![このストレージ アカウントでホストされているキューのリストに、キューが表示されていることを示すスクリーン ショット](../media-draft/q-in-list-small.png)

4. <span data-ttu-id="afe10-281">**[bookmarks-post-process]** をクリックしてキューを開きます。</span><span class="sxs-lookup"><span data-stu-id="afe10-281">Click on **bookmarks-post-process** to open the queue.</span></span> <span data-ttu-id="afe10-282">キュー内にあるメッセージがリストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="afe10-282">The messages that are in the queue are displayed in a list.</span></span> <span data-ttu-id="afe10-283">すべてが計画どおりに進んでいれば、ブックマークをデータベースに追加したときに投稿したメッセージは、キュー内に存在するはずであり、次のエントリのようになります。</span><span class="sxs-lookup"><span data-stu-id="afe10-283">If all went according to plan, the message we posted when we added a bookmark to our database should be in the queue and will look like the following entry.</span></span> 

![キュー内のメッセージを示すスクリーン ショット](../media-draft/message-in-q-small.png)

<span data-ttu-id="afe10-285">この例では、メッセージに一意の ID が付与されており、**MESSAGE TEXT** フィールドに、使用しているブックマークが JSON 文字列の形式で表示されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="afe10-285">In this example, you can see that the message was given a unique ID and the **MESSAGE TEXT** field displays our bookmark in JSON string format.</span></span>

5. <span data-ttu-id="afe10-286">関数はさらにテストすることができます。それには、テスト パネル内で、新しい id/url を設定することで要求本文を変更し、関数を実行します。</span><span class="sxs-lookup"><span data-stu-id="afe10-286">You can test the function further by changing the request body in the Test panel with new id/url sets and running the function.</span></span> <span data-ttu-id="afe10-287">このキューを見ると、さらなるメッセージが届いているのがわかります。</span><span class="sxs-lookup"><span data-stu-id="afe10-287">Watch this queue to see more messages arrive.</span></span> <span data-ttu-id="afe10-288">また、データベースを参照することで、新しいエントリが追加されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="afe10-288">You can also look at the database to verify new entries have been added.</span></span> 

<span data-ttu-id="afe10-289">このラボでは、バインディングに関する知識を出力バインディング (Azure Cosmos DB へのデータの書き込み) にまで広げました。</span><span class="sxs-lookup"><span data-stu-id="afe10-289">In this lab, we expanded our knowledge of bindings to output bindings, writing data to our Azure Cosmos DB.</span></span> <span data-ttu-id="afe10-290">さらに先に進めて、Azure キューにメッセージを投稿するための別の出力バインディングを追加しました。</span><span class="sxs-lookup"><span data-stu-id="afe10-290">We went further and added another output binding to post messages to an Azure queue.</span></span> <span data-ttu-id="afe10-291">これにより、データを成形してそれを受信ソースからさまざまな宛先に移動するのに役立つバインディングの真のパワーが実証されました。</span><span class="sxs-lookup"><span data-stu-id="afe10-291">This demonstrates the true power of bindings to help you shape and move data from incoming sources to a variety of destinations.</span></span> <span data-ttu-id="afe10-292">データベース コードの作成は行いませんでした。また、接続文字列の管理を自分たちで行う必要もありませんでした。</span><span class="sxs-lookup"><span data-stu-id="afe10-292">We haven't written any database code or had to manage connection strings ourselves.</span></span> <span data-ttu-id="afe10-293">代わりに、バインディングを宣言によって構成し、接続のセキュリティ保護、関数のスケーリング、および接続のスケーリングをプラットフォームで対処しました。</span><span class="sxs-lookup"><span data-stu-id="afe10-293">Instead, we configured bindings declaratively and let the platform take care of securing connections, scaling our function and scaling our connections.</span></span>