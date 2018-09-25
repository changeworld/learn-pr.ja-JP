<span data-ttu-id="d1d69-101">前回の演習では、Azure Cosmos DB データベース内のブックマークを検索するシナリオを実装しました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-101">In our last exercise, we implemented a scenario to look up bookmarks in an Azure Cosmos DB database.</span></span> <span data-ttu-id="d1d69-102">このブックマークのコレクションからデータを読み取るように入力バインディングを構成しました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-102">We configured an input binding to read data from our bookmarks collection.</span></span> <span data-ttu-id="d1d69-103">しかし、できることはもっとたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-103">But, we can do more.</span></span> <span data-ttu-id="d1d69-104">シナリオを拡張して書き込みも行ってみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-104">Let's expand the scenario to include writing.</span></span> <span data-ttu-id="d1d69-105">次のフローチャートについて考えてみます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-105">Consider the following flowchart:</span></span>

![Cosmos DB バックエンドでブックマークを見つけるプロセスを示すフロー図。](../media/7-add-bookmark-flow-small.png)

<span data-ttu-id="d1d69-110">このシナリオでは、コレクションにブックマークを追加する要求を受信します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-110">In this scenario, we'll receive requests to add bookmarks to our collection.</span></span> <span data-ttu-id="d1d69-111">この要求によって目的のキーまたは ID が、ブックマーク URL と共に渡されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-111">The requests pass in the desired key, or ID, along with the bookmark URL.</span></span> <span data-ttu-id="d1d69-112">フローチャート内で確認できるように、そのキーがバックエンド内に既に存在する場合はエラーで応答します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-112">As you can see in the flowchart, we'll respond with an error if the key already exists in our back end.</span></span>

<span data-ttu-id="d1d69-113">渡されたキーが*見つからない*場合は、新しいブックマークをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-113">If the key that was passed to us is *not* found, we'll add the new bookmark to our database.</span></span> <span data-ttu-id="d1d69-114">それで終わりにしてもかまいませんが、もう少しやってみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-114">We could stop there, but let's do a little more.</span></span>

<span data-ttu-id="d1d69-115">フローチャート内の別の手順に気付きましたか。</span><span class="sxs-lookup"><span data-stu-id="d1d69-115">Notice another step in the flowchart?</span></span> <span data-ttu-id="d1d69-116">これまで、処理の観点から受信したデータに対して大したことは行っていません。</span><span class="sxs-lookup"><span data-stu-id="d1d69-116">So far we haven't done much with the data that we receive in terms of processing.</span></span> <span data-ttu-id="d1d69-117">受信した内容をデータベースに移動します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-117">We move what we receive into a database.</span></span> <span data-ttu-id="d1d69-118">ただし、実際のソリューションでは、おそらくそのデータを何らかの方法で処理することが可能です。</span><span class="sxs-lookup"><span data-stu-id="d1d69-118">However, in a real solution, it is possible that we'd probably process the data in some fashion.</span></span> <span data-ttu-id="d1d69-119">同じ関数ですべての処理を実行することもできますが、このラボでは、追加の処理を別のコンポーネントまたはビジネス ロジックの一部にオフロードするパターンを示します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-119">We can decide to do all processing in the same function, but in this lab we'll show a pattern that offloads further processing to another component or piece of business logic.</span></span>

<span data-ttu-id="d1d69-120">このブックマーク シナリオでのこの作業のオフロードの良い例として、どのようなものが考えられるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="d1d69-120">What might be a good example of this offloading of work in our bookmarks scenario?</span></span> <span data-ttu-id="d1d69-121">新しいブックマークを QR コード生成サービスに送信するというのはどうでしょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-121">Well, what if we send the new bookmark to a QR code generation service?</span></span> <span data-ttu-id="d1d69-122">そのサービスの場合は、さらに、URL の QR コードが生成され、イメージが BLOB ストレージに格納され、QR イメージのアドレスがブックマーク コレクション内のエントリに再び追加されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-122">That service would, in turn, generate a QR code for the URL, store the image in blob storage, and add the address of the QR image back into the entry in our bookmarks collection.</span></span> <span data-ttu-id="d1d69-123">QR イメージを生成するサービスの呼び出しには時間がかかります。そのため、結果を待つのでなく、関数にサービスを渡して、その関数で非同期的に対処するようにします。</span><span class="sxs-lookup"><span data-stu-id="d1d69-123">Calling a service to generate a QR image is time consuming so, rather than wait for the result, we hand it off to a function and let it take care of this asynchronously.</span></span>

<span data-ttu-id="d1d69-124">Azure Functions では、さまざまな統合ソースに対して入力バインディングがサポートされているのと同様に、出力バインディング用のテンプレートのセットも用意されています。そのため、お客様はデータ ソースへのデータの書き込みを容易に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-124">Just as Azure Functions supports input bindings for various integration sources, it also has a set of output bindings templates to make it easy for you to write data to data sources.</span></span> <span data-ttu-id="d1d69-125">出力バインディングは、*function.json* ファイルでも構成されています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-125">Output bindings are also configured in the *function.json* file.</span></span>  <span data-ttu-id="d1d69-126">この演習で示すように、複数のデータ ソースとサービスを操作するように関数を構成できます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-126">As you'll see in this exercise, we can configure our function to work with multiple data sources and services.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d1d69-127">この演習は前の演習に基づいています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-127">This exercise builds on the previous one.</span></span> <span data-ttu-id="d1d69-128">同じ Azure Cosmos DB データベースと入力バインディングが使用されています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-128">It uses the same Azure Cosmos DB database and input binding.</span></span> <span data-ttu-id="d1d69-129">そのユニットを終えていない場合は、それを行ってからこのユニットに進むことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d1d69-129">If you haven't worked through that unit, we recommend doing so before you proceed with this one.</span></span>

## <a name="create-an-http-triggered-function"></a><span data-ttu-id="d1d69-130">HTTP によってトリガーされる関数を作成する</span><span class="sxs-lookup"><span data-stu-id="d1d69-130">Create an HTTP-triggered function</span></span>

1. <span data-ttu-id="d1d69-131">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="d1d69-131">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

2. <span data-ttu-id="d1d69-132">ポータルで、このモジュールで作成した関数アプリに移動します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-132">In the portal, navigate to the function app that you created in this module.</span></span>

3. <span data-ttu-id="d1d69-133">ご利用の関数アプリを展開し、関数のコレクションをポイントして、**[関数]** の横にある [追加] (**+**) ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-133">Expand your function app, then hover over the functions collection and select the Add (**+**) button next to **Functions**.</span></span> <span data-ttu-id="d1d69-134">この操作により関数作成プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-134">This action starts the function creation process.</span></span> <span data-ttu-id="d1d69-135">次のアニメーションで、この操作を示します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-135">The following animation illustrates this action.</span></span>

    ![ユーザーが [関数] メニュー項目をポイントしたときにプラス記号が表示されるというアニメーション。](../media/3-func-app-plus-hover-small.gif)

4. <span data-ttu-id="d1d69-137">ページのサポートされているトリガーの現在のセットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-137">The page shows us the current set of supported triggers.</span></span> <span data-ttu-id="d1d69-138">**[HTTP トリガー]** (次のスクリーン ショットの最初のエントリ) を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-138">Select **HTTP trigger**, which is the first entry in the following screenshot.</span></span>

    ![トリガー テンプレートの選択 UI の一部を示すスクリーンショット。イメージの左上に TTP トリガーが最初に表示されている。](../media/5-trigger-templates-small.PNG)


5. <span data-ttu-id="d1d69-140">右側に表示された **[新しい関数]** ウィンドウに、次の値を使用して入力します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-140">Fill out the **New Function** pane that's displayed at the right by using the following values:</span></span>

    |<span data-ttu-id="d1d69-141">フィールド</span><span class="sxs-lookup"><span data-stu-id="d1d69-141">Field</span></span>  |<span data-ttu-id="d1d69-142">値</span><span class="sxs-lookup"><span data-stu-id="d1d69-142">Value</span></span>  |
    |---------|---------|
    |<span data-ttu-id="d1d69-143">言語</span><span class="sxs-lookup"><span data-stu-id="d1d69-143">Language</span></span>     | <span data-ttu-id="d1d69-144">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="d1d69-144">**JavaScript**</span></span>        |
    |<span data-ttu-id="d1d69-145">名前</span><span class="sxs-lookup"><span data-stu-id="d1d69-145">Name</span></span>     |   [!INCLUDE [func-name-add](./func-name-add.md)]     |
    | <span data-ttu-id="d1d69-146">承認レベル</span><span class="sxs-lookup"><span data-stu-id="d1d69-146">Authorization level</span></span> | <span data-ttu-id="d1d69-147">**関数**</span><span class="sxs-lookup"><span data-stu-id="d1d69-147">**Function**</span></span> |

6. <span data-ttu-id="d1d69-148">**[作成]** を選択して関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-148">Select **Create** to create your function.</span></span> <span data-ttu-id="d1d69-149">これにより、コード エディターで **index.js** ファイルが開き、HTTP によってトリガーされる関数の既定の実装が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-149">This action opens the **index.js** file in the code editor and displays a default implementation of the HTTP-triggered function.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d1d69-150">この演習では、出発点として前のユニットからの*コード*および*構成*を使用することによりスピードアップを図ります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-150">In this exercise, we'll speed up things by using the *code* and *configuration* from the previous unit as a starting point.</span></span>

7. <span data-ttu-id="d1d69-151">**index.js** ファイル内のすべてのコードを、次のスニペットのコードに置き換え、**[保存]** を選択して変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-151">Replace all code in the **index.js** file with the code from the following snippet, and then select **Save** to save the change:</span></span>

   [!code-javascript[](../code/find-bookmark-single.js)]

   <span data-ttu-id="d1d69-152">このコードをよくご存じの場合、それは提供されている [!INCLUDE [func-name-find](./func-name-find.md)] 関数の実装だからです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-152">If this code looks familiar, that's because it's the implementation of our [!INCLUDE [func-name-find](./func-name-find.md)] function.</span></span> <span data-ttu-id="d1d69-153">予想されるとおり、同じバインディングが定義されるまで、関数は機能しません。</span><span class="sxs-lookup"><span data-stu-id="d1d69-153">As you would expect, the function won't work until we define the same bindings.</span></span>

1. <span data-ttu-id="d1d69-154">[!INCLUDE [func-name-add](./func-name-add.md)] 関数から **function.json** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-154">Open the **function.json** file from the [!INCLUDE [func-name-add](./func-name-add.md)] function.</span></span>

11. <span data-ttu-id="d1d69-155">このファイルの内容を次の JSON に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-155">Replace the contents of this file with the following JSON:</span></span>

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

12. <span data-ttu-id="d1d69-156">必ず、すべての変更を**保存**してください。</span><span class="sxs-lookup"><span data-stu-id="d1d69-156">Make sure to **Save** all changes.</span></span>

<span data-ttu-id="d1d69-157">前の手順では、新しい関数のバインディングを、別の関数からバインディング定義をコピーすることによって構成しました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-157">In the preceding steps, you configured bindings for your new function by copying binding definitions from another function.</span></span> <span data-ttu-id="d1d69-158">もちろん、UI を介して新しいバインディングを作成することもできましたが、この代替方法を利用できることを理解しておくことはよいことです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-158">Of course, you could have created a new binding through the UI, but it is good to understand that this alternative is available to you.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="d1d69-159">試してみる</span><span class="sxs-lookup"><span data-stu-id="d1d69-159">Try it out</span></span>

1. <span data-ttu-id="d1d69-160">右上の **[関数の URL の取得]**、**[既定値 (関数キー)]**、**[コピー]** の順に選択して関数の URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="d1d69-160">Select **Get function URL** at the top right, select **default (Function key)**, and then select **Copy** to copy the function's URL.</span></span>

2. <span data-ttu-id="d1d69-161">コピーした URL をブラウザーのアドレス バーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-161">Paste the copied URL into your browser's address bar.</span></span> <span data-ttu-id="d1d69-162">URL の末尾にクエリ文字列値 `&id=docs` を追加してから、Enter キーを押して要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-162">Add the query string value `&id=docs` to the end of the URL, and then press Enter to execute the request.</span></span> <span data-ttu-id="d1d69-163">すべて順調であれば、そのリソースへの URL を含む応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-163">If all goes well, you should see a response that includes a URL to that resource.</span></span>

<span data-ttu-id="d1d69-164">さて、どこまで来たでしょうか。</span><span class="sxs-lookup"><span data-stu-id="d1d69-164">So, where are we at?</span></span> <span data-ttu-id="d1d69-165">これまで、実際には前回のラボで行った内容をレプリケートしただけでした。</span><span class="sxs-lookup"><span data-stu-id="d1d69-165">Well, so far we've really just replicated what we did in the previous lab.</span></span> <span data-ttu-id="d1d69-166">しかし、それは問題ありません。</span><span class="sxs-lookup"><span data-stu-id="d1d69-166">But that's okay.</span></span> <span data-ttu-id="d1d69-167">前回のラボで行った内容をコピーして、このラボの出発点として活用します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-167">We're copying what we did in the last lab to serve as a starting point for this one.</span></span> <span data-ttu-id="d1d69-168">次は新しい機能を操作します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-168">We'll work on the new stuff next.</span></span> <span data-ttu-id="d1d69-169">つまり、データベースに書き込みます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-169">That is, we'll write to our database.</span></span> <span data-ttu-id="d1d69-170">そのため、*出力バインディング*が必要です。</span><span class="sxs-lookup"><span data-stu-id="d1d69-170">For that, we'll need an *output binding*.</span></span>

## <a name="define-azure-cosmos-db-output-binding"></a><span data-ttu-id="d1d69-171">Azure Cosmos DB の出力バインディングを定義する</span><span class="sxs-lookup"><span data-stu-id="d1d69-171">Define Azure Cosmos DB output binding</span></span>

<span data-ttu-id="d1d69-172">ユーザー インターフェイスを経由して新しい出力バインディングを定義するのでなく、構成ファイル *function.json* を手動で更新してこのバインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-172">Rather than define a new output binding by going through the user interface, you'll create this binding by updating the configuration file, *function.json*, by hand.</span></span>

1. <span data-ttu-id="d1d69-173">エディターで [!INCLUDE [func-name-add](./func-name-add.md)] の *function.json* ファイルを開いていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d1d69-173">Make sure the *function.json* file for [!INCLUDE [func-name-add](./func-name-add.md)] is open in the editor.</span></span>

1. <span data-ttu-id="d1d69-174">そのファイルの `bookmark` という名前のバインディングをコピーします。</span><span class="sxs-lookup"><span data-stu-id="d1d69-174">Copy the binding that's named `bookmark` in that file.</span></span>

1. <span data-ttu-id="d1d69-175">閉じ中かっこ (}) のすぐ後、閉じ角かっこ (]) のすぐ前にカーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-175">Place your cursor directly after the closing curly bracket (}), and right before the closing square bracket (]).</span></span> <span data-ttu-id="d1d69-176">コンマ (,) を追加し、バインディングのコピーをここに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-176">Add a comma (,), and then paste the copy of the binding here.</span></span> <span data-ttu-id="d1d69-177">ご利用の *function.json* 構成ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-177">Your *function.json* config file should now look like the following:</span></span>

   [!code-json[](../code/config-new-entry.json?highlight=22-31)]

1. <span data-ttu-id="d1d69-178">貼り付けたバインディングを編集して、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-178">Edit the binding you pasted, with the following changes:</span></span>

    |<span data-ttu-id="d1d69-179">プロパティ</span><span class="sxs-lookup"><span data-stu-id="d1d69-179">Property</span></span>   |<span data-ttu-id="d1d69-180">古い値</span><span class="sxs-lookup"><span data-stu-id="d1d69-180">Old value</span></span>  |<span data-ttu-id="d1d69-181">新しい値</span><span class="sxs-lookup"><span data-stu-id="d1d69-181">New value</span></span>  |
    |---------|---------|---------|
    |<span data-ttu-id="d1d69-182">name</span><span class="sxs-lookup"><span data-stu-id="d1d69-182">name</span></span>     |   <span data-ttu-id="d1d69-183">bookmark</span><span class="sxs-lookup"><span data-stu-id="d1d69-183">bookmark</span></span>      |  <span data-ttu-id="d1d69-184">**newbookmark**</span><span class="sxs-lookup"><span data-stu-id="d1d69-184">**newbookmark**</span></span>       |
    |<span data-ttu-id="d1d69-185">direction</span><span class="sxs-lookup"><span data-stu-id="d1d69-185">direction</span></span>     |   <span data-ttu-id="d1d69-186">in</span><span class="sxs-lookup"><span data-stu-id="d1d69-186">in</span></span>      |   <span data-ttu-id="d1d69-187">**out**</span><span class="sxs-lookup"><span data-stu-id="d1d69-187">**out**</span></span>      |
    |<span data-ttu-id="d1d69-188">id</span><span class="sxs-lookup"><span data-stu-id="d1d69-188">id</span></span>     |      <span data-ttu-id="d1d69-189">{id}</span><span class="sxs-lookup"><span data-stu-id="d1d69-189">{id}</span></span>   |   <span data-ttu-id="d1d69-190">**このプロパティを削除します。それは出力バインディングには存在しません。**</span><span class="sxs-lookup"><span data-stu-id="d1d69-190">**delete this property. It does not exist for the output binding.**</span></span>      |

1. <span data-ttu-id="d1d69-191">これらの変更を行った後、ファイルは次の JSON のようになります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-191">After you've made these changes, your file looks like the following JSON:</span></span>

    [!code-json[](../code/config-q-complete.json?highlight=22-30)]

<span data-ttu-id="d1d69-192">これは、どのようにすれば構成ファイルでもバインディングを直接作成できるかを示すデモにすぎません。</span><span class="sxs-lookup"><span data-stu-id="d1d69-192">That was just a demo of how you can also create bindings directly in the configuration file.</span></span> <span data-ttu-id="d1d69-193">この例では、別のバインディングからのプロパティを再利用するので、それは理にかなっています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-193">In this example, it makes sense because you are reusing the properties from another binding.</span></span> <span data-ttu-id="d1d69-194">つまり、Cosmos DB 入力バインディング用に既に構成してある `databaseName`、`collectionName`、および `connection` を再利用します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-194">That is, you're reusing the `databaseName`, `collectionName`, and `connection` that you already configured for your Azure Cosmos DB input binding.</span></span>

> [!NOTE]
> <span data-ttu-id="d1d69-195">上記の JSON ファイル内の `connection` の実際の値は、接続が作成されたときにそれに付けられた任意の名前です。</span><span class="sxs-lookup"><span data-stu-id="d1d69-195">The actual value of `connection` in the preceding JSON file is whatever name your connection was given when it was created.</span></span>

<span data-ttu-id="d1d69-196">コードを更新する前に、キューにメッセージを投稿できるようにバインディングをもう 1 つ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-196">Before we update our code, let's add one more binding that will enable us to post messages to a queue.</span></span>

## <a name="define-azure-queue-storage-output-binding"></a><span data-ttu-id="d1d69-197">Azure Queue Storage 出力バインディングを定義する</span><span class="sxs-lookup"><span data-stu-id="d1d69-197">Define Azure Queue Storage output binding</span></span>

<span data-ttu-id="d1d69-198">Azure Queue Storage は、世界中のどこからでもアクセスできるメッセージを格納するためのサービスです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-198">Azure Queue storage is a service for storing messages that can be accessed from anywhere in the world.</span></span> <span data-ttu-id="d1d69-199">単一のメッセージのサイズは 64 KB ほどになります。キューには、ストレージ アカウントの合計容量に達するまで、数百万のメッセージを格納できます。この合計容量はストレージ アカウントで定義されています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-199">The size of a single message can be as much as 64 KB, and a queue can contain millions of messages&mdash;up to the total capacity of the storage account in which it is defined.</span></span> <span data-ttu-id="d1d69-200">次の図に、このシナリオでのキューの使用方法の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-200">The following diagram shows at a high level how a queue is used in our scenario:</span></span>

![ストレージ キューと 2 つの関数 (1 つはキューにメッセージをプッシュし、もう 1 つはキューにメッセージをポップする) を示す図。](../media/7-q-logical-small.png)

<span data-ttu-id="d1d69-202">ここで、新しい関数 [!INCLUDE [func-name-add](./func-name-add.md)] によってキューにメッセージが追加されることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-202">Here you can see that the new function, [!INCLUDE [func-name-add](./func-name-add.md)], adds messages to a queue.</span></span> <span data-ttu-id="d1d69-203">別の関数 (例えば、*gen-qr-code* と呼ばれる架空の関数) では、同じキューからメッセージがポップされ、要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-203">Another function&mdash;for example, a fictitious function called *gen-qr-code*&mdash;will pop messages from the same queue and process the request.</span></span>  <span data-ttu-id="d1d69-204">[!INCLUDE [func-name-add](./func-name-add.md)] からキューにメッセージを書き込む (つまり、*プッシュする*) ので、このソリューションに新しい出力バインディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-204">Since we write, or *push*, messages to the queue from [!INCLUDE [func-name-add](./func-name-add.md)], we'll add a new output binding to your solution.</span></span> <span data-ttu-id="d1d69-205">今度は、ポータル UI を介してバインディングを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-205">Let's create the binding through the portal UI this time.</span></span>

1. <span data-ttu-id="d1d69-206">左側の関数メニューで **[統合]** を選択して、[統合] タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-206">Select **Integrate** in the left function menu to open the integration tab.</span></span>

2. <span data-ttu-id="d1d69-207">**[出力]** 列で **[新しい出力]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-207">Select **New Output** in the **Outputs** column.</span></span>
    <span data-ttu-id="d1d69-208">使用可能なすべての種類の出力バインディングのリストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-208">A list of all possible output binding types is displayed.</span></span>

3. <span data-ttu-id="d1d69-209">このリストで、**[Azure Queue Storage]**、**[選択]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-209">In the list, select **Azure Queue Storage**, then select **Select**.</span></span>
    <span data-ttu-id="d1d69-210">この操作によって、Azure Queue Storage 出力構成ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-210">This action opens the Azure Queue Storage output configuration page.</span></span>

   <span data-ttu-id="d1d69-211">次に、ストレージ アカウント接続を設定します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-211">Next, we'll set up a storage account connection.</span></span> <span data-ttu-id="d1d69-212">これはキューがホストされる場所です。</span><span class="sxs-lookup"><span data-stu-id="d1d69-212">This is where our queue will be hosted.</span></span>

4. <span data-ttu-id="d1d69-213">**[ストレージ アカウント接続]** フィールドの右側にある **[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-213">To the right of the **Storage account connection** field, select **new**.</span></span>
   <span data-ttu-id="d1d69-214">**ストレージ アカウント**の選択ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-214">The **Storage Account** selection pane opens.</span></span>

5. <span data-ttu-id="d1d69-215">このモジュールを開始して関数アプリを作成したとき、ストレージ アカウントも同時に作成されました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-215">When we started this module and you created your function app, a storage account was also created at that time.</span></span> <span data-ttu-id="d1d69-216">このウィンドウにリストされますので、それを選択してください。</span><span class="sxs-lookup"><span data-stu-id="d1d69-216">It's listed in this pane, so select it.</span></span> <span data-ttu-id="d1d69-217">**[ストレージ アカウント接続]** フィールドには、接続の名前が設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-217">The **Storage account connection** field is populated with the name of a connection.</span></span> <span data-ttu-id="d1d69-218">接続文字列の値を表示する場合は、**[値の表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-218">If you want to see the connection string value, select **show value**.</span></span>

6. <span data-ttu-id="d1d69-219">他のフィールドはすべて既定値のままにしておいてもかまいませんが、ここではプロパティに意味を加えるために次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-219">Although we could keep the default values in all the other fields, let's change the following to lend more meaning to the properties:</span></span>

    |<span data-ttu-id="d1d69-220">プロパティ</span><span class="sxs-lookup"><span data-stu-id="d1d69-220">Property</span></span>  |<span data-ttu-id="d1d69-221">古い値</span><span class="sxs-lookup"><span data-stu-id="d1d69-221">Old value</span></span>  |<span data-ttu-id="d1d69-222">新しい値</span><span class="sxs-lookup"><span data-stu-id="d1d69-222">New value</span></span>  | <span data-ttu-id="d1d69-223">説明</span><span class="sxs-lookup"><span data-stu-id="d1d69-223">Description</span></span> |
    |---------|---------|---------|---------|
    |<span data-ttu-id="d1d69-224">キュー名</span><span class="sxs-lookup"><span data-stu-id="d1d69-224">Queue name</span></span>     |    <span data-ttu-id="d1d69-225">outqueue</span><span class="sxs-lookup"><span data-stu-id="d1d69-225">outqueue</span></span>     |  <span data-ttu-id="d1d69-226">**bookmarks-post-process**</span><span class="sxs-lookup"><span data-stu-id="d1d69-226">**bookmarks-post-process**</span></span>      | <span data-ttu-id="d1d69-227">ブックマークを別の関数でさらに処理できるようにブックマークの配置先とするキューの名前です。</span><span class="sxs-lookup"><span data-stu-id="d1d69-227">The name of the queue where we're placing bookmarks so that they can be processed further by another function.</span></span> |
    | <span data-ttu-id="d1d69-228">メッセージ パラメーター名</span><span class="sxs-lookup"><span data-stu-id="d1d69-228">Message parameter name</span></span>    |  <span data-ttu-id="d1d69-229">outputQueueItem</span><span class="sxs-lookup"><span data-stu-id="d1d69-229">outputQueueItem</span></span>       |   <span data-ttu-id="d1d69-230">**newmessage**</span><span class="sxs-lookup"><span data-stu-id="d1d69-230">**newmessage**</span></span>      | <span data-ttu-id="d1d69-231">コードで使用するバインディング プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-231">The binding property we'll use in code.</span></span> |

7. <span data-ttu-id="d1d69-232">**[保存]** を選択して変更を保存することを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="d1d69-232">Remember to select **Save** to save your changes.</span></span>

## <a name="update-function-implementation"></a><span data-ttu-id="d1d69-233">関数の実装を更新する</span><span class="sxs-lookup"><span data-stu-id="d1d69-233">Update function implementation</span></span>

<span data-ttu-id="d1d69-234">これで、[!INCLUDE [func-name-add](./func-name-add.md)] 関数に対するバインディングがすべて設定されました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-234">We now have all our bindings set up for the [!INCLUDE [func-name-add](./func-name-add.md)] function.</span></span> <span data-ttu-id="d1d69-235">それらをこの関数で使ってみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-235">It's time to use them in our function.</span></span>

1.  <span data-ttu-id="d1d69-236">関数 [!INCLUDE [func-name-add](./func-name-add.md)] を選択して、**index.js** ファイルをコード エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-236">Select your function, [!INCLUDE [func-name-add](./func-name-add.md)], to open the **index.js** file in the code editor.</span></span>

2. <span data-ttu-id="d1d69-237">*index.js* ファイル内のすべてのコードを、次のスニペットのコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-237">Replace all the code in the *index.js* file with the code from the following snippet:</span></span>

   [!code-javascript[](../code/add-bookmark.js)]

<span data-ttu-id="d1d69-238">このコードで何が行われるのかを詳しく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-238">Let's break down what this code does:</span></span>

* <span data-ttu-id="d1d69-239">この関数によってデータが変更されるので、HTTP 要求が POST になり、ブックマーク データが要求本文の一部になると想定しています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-239">Because this function changes our data, we expect the HTTP request to be a POST and the bookmark data to be part of the request body.</span></span>
* <span data-ttu-id="d1d69-240">Azure Cosmos DB の入力バインディングでは、受信する `id` を使用してドキュメントまたはブックマークの取得が試みられます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-240">Our Azure Cosmos DB input binding attempts to retrieve a document, or bookmark, by using the `id` that we receive.</span></span> <span data-ttu-id="d1d69-241">エントリが見つかると、`bookmark` オブジェクトが設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-241">If it finds an entry, the `bookmark` object will be set.</span></span> <span data-ttu-id="d1d69-242">`if(bookmark)` 条件では、エントリが見つかったかどうかが確認されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-242">The `if(bookmark)` condition checks to see whether an entry was found.</span></span>
* <span data-ttu-id="d1d69-243">データベースへの追加は、JSON 文字列として作成した新しいブックマーク エントリに `context.bindings.newbookmark` バインディング パラメーターを設定するのと同じくらいシンプルです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-243">Adding to the database is as simple as setting the `context.bindings.newbookmark` binding parameter to the new bookmark entry, which we have created as a JSON string.</span></span>
* <span data-ttu-id="d1d69-244">キューへのメッセージの投稿は、`context.bindings.newmessage parameter` を設定するのと同じくらいシンプルです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-244">Posting a message to our queue is as simple as setting the  `context.bindings.newmessage parameter`.</span></span>

> [!NOTE]
> <span data-ttu-id="d1d69-245">行ったタスクは、キュー バインディングの作成のみです。</span><span class="sxs-lookup"><span data-stu-id="d1d69-245">The only task you performed was to create a queue binding.</span></span> <span data-ttu-id="d1d69-246">キューの作成は明示的に行っていません。</span><span class="sxs-lookup"><span data-stu-id="d1d69-246">You never created the queue explicitly.</span></span> <span data-ttu-id="d1d69-247">バインディングの機能を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-247">You are witnessing the power of bindings!</span></span> <span data-ttu-id="d1d69-248">次のコールアウトで示すように、キューは存在しない場合、自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-248">As the following callout says, the queue is automatically created for you if it doesn't exist.</span></span>

![キューが自動的に作成されることを示すコールアウトのスクリーンショット。](../media/7-q-auto-create-small.png)

<span data-ttu-id="d1d69-250">これで終了です。</span><span class="sxs-lookup"><span data-stu-id="d1d69-250">So, that's it.</span></span> <span data-ttu-id="d1d69-251">次のセクションで作業内容の動作を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-251">Let's see our work in action in the next section.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="d1d69-252">試してみる</span><span class="sxs-lookup"><span data-stu-id="d1d69-252">Try it out</span></span>

<span data-ttu-id="d1d69-253">出力バインディングが複数あるので、テストはやや複雑になります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-253">Now that we have multiple output bindings, testing becomes a little trickier.</span></span> <span data-ttu-id="d1d69-254">前のラボでは HTTP 要求とクエリ文字列を送信し、テストして満足しましたが、今度は HTTP 投稿を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-254">In previous labs we were content to test by sending an HTTP request and a query string, but we'll want to perform an HTTP post this time.</span></span> <span data-ttu-id="d1d69-255">メッセージがキューに入っているかどうかを確認する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-255">We also need to check to see whether messages are making it into a queue.</span></span>

1. <span data-ttu-id="d1d69-256">Function App ポータルで関数 [!INCLUDE [func-name-add](./func-name-add.md)] を選択した状態で、左端にある [テスト] メニュー項目を選択して展開します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-256">With our function, [!INCLUDE [func-name-add](./func-name-add.md)], selected in the Function Apps portal, select the Test menu item at the far left to expand it.</span></span>

2. <span data-ttu-id="d1d69-257">**[テスト]** メニュー項目を選択し、テスト パネルが開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-257">Select the **Test** menu item, and verify that you have the test pane open.</span></span> <span data-ttu-id="d1d69-258">次のスクリーンショットは、どのような表示になるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-258">The following screenshot shows what it should look like:</span></span>

    ![展開された関数のテスト パネルを示すスクリーンショット。](../media/7-test-panel-open-small.png)

    > [!IMPORTANT]
    > <span data-ttu-id="d1d69-260">HTTP メソッドのドロップダウン リストで、**[POST]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-260">Make sure that **POST** is selected in the HTTP method drop-down list.</span></span>

3. <span data-ttu-id="d1d69-261">要求本文の内容を、次の JSON ペイロードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-261">Replace the content of the request body with the following JSON payload:</span></span>

    ```json
    {
        "id": "docs",
        "url": "https://docs.microsoft.com/azure"
    }
    ```

4. <span data-ttu-id="d1d69-262">テスト パネルの下部にある **[実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-262">Select **Run** at the bottom of the test pane.</span></span>

5. <span data-ttu-id="d1d69-263">**[出力]** ウィンドウに、次の図に示すような "ブックマークは既に存在します" というメッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-263">Verify that the **Output** window displays the "Bookmark already exists" message, as shown in the following diagram:</span></span>

    ![テスト パネルと失敗したテストの結果を示すスクリーンショット。](../media/7-test-exists-small.png)

6. <span data-ttu-id="d1d69-265">要求本文を次のペイロードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-265">Replace the request body with the following payload:</span></span>

    ```json
    {
        "id": "github",
        "url": "https://www.github.com"
    }
    ```
7. <span data-ttu-id="d1d69-266">テスト パネルの下部にある **[実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-266">Select **Run** at the bottom of the test pane.</span></span>

8. <span data-ttu-id="d1d69-267">*[出力]* ボックスに、次の図に示すような "ブックマークが追加されました" というメッセージが表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-267">Verify the that *Output* box displays the "bookmark added" message as shown in the following diagram.</span></span>

    ![テスト パネルと成功したテストの結果を示すスクリーン ショット。](../media/7-test-success-small.png)

<span data-ttu-id="d1d69-269">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="d1d69-269">Congratulations!</span></span> <span data-ttu-id="d1d69-270">[!INCLUDE [func-name-add](./func-name-add.md)] は設計どおりに機能しますが、コード内に含めたクエリ操作についてはどうでしょうか。</span><span class="sxs-lookup"><span data-stu-id="d1d69-270">The [!INCLUDE [func-name-add](./func-name-add.md)] works as designed, but what about that queue operation we had in the code?</span></span> <span data-ttu-id="d1d69-271">キューに何か書き込まれたかどうかを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="d1d69-271">Well, let's go see whether something was written to a queue.</span></span>

### <a name="verify-that-a-message-is-written-to-the-queue"></a><span data-ttu-id="d1d69-272">キューにメッセージが書き込まれたことを確認する</span><span class="sxs-lookup"><span data-stu-id="d1d69-272">Verify that a message is written to the queue</span></span>

<span data-ttu-id="d1d69-273">Azure Queue Storage キューは、ストレージ アカウントでホストされています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-273">Azure Queue Storage queues are hosted in a storage account.</span></span> <span data-ttu-id="d1d69-274">この演習では、出力バインディングの作成時にストレージ アカウントを既に選択しています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-274">You already selected the storage account in this exercise when you created the output binding.</span></span>

1. <span data-ttu-id="d1d69-275">Azure portal 内のメインの検索ボックスに「**ストレージ アカウント**」と入力し、結果リストで **[サービス]** の下にある **[ストレージ アカウント]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-275">In the main search box in the Azure portal, type **storage accounts**, and in the results list, under **Services**, select **Storage accounts**.</span></span>

      ![メインの検索ボックスでのストレージ アカウントに対する検索結果を示すスクリーンショット。](../media/7-search-for-sa-small.png)

2. <span data-ttu-id="d1d69-277">返されたストレージ アカウントのリストで、**newmessage** 出力バインディングを作成するために使用したストレージ アカウントを選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-277">In the list of storage accounts that are returned, select the storage account that you used to create the **newmessage** output binding.</span></span>
   <span data-ttu-id="d1d69-278">ストレージ アカウントの設定はポータルのメイン ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-278">The storage account settings are displayed in the main window of the portal.</span></span>

3. <span data-ttu-id="d1d69-279">**[サービス]** リストで、**[キュー]** 項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-279">In the **Services** list, select the **Queues** item.</span></span>
   <span data-ttu-id="d1d69-280">このストレージ アカウントでホストされているキューのリストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-280">A list of queues hosted by this storage account is displayed.</span></span> <span data-ttu-id="d1d69-281">次のスクリーンショットに示すように、**bookmarks-post-process** キューが存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-281">Verify that the **bookmarks-post-process** queue exists, as shown in the following screenshot:</span></span>

      ![このストレージ アカウントでホストされているキューのリストにキューが表示されていることを示すスクリーンショット](../media/7-q-in-list-small.png)

4. <span data-ttu-id="d1d69-283">**[bookmarks-post-process]** を選択してキューを開きます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-283">Select **bookmarks-post-process** to open the queue.</span></span>
   <span data-ttu-id="d1d69-284">キュー内にあるメッセージがリストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-284">The messages that are in the queue are displayed in a list.</span></span> <span data-ttu-id="d1d69-285">すべてが計画どおりに進んでいれば、ブックマークをデータベースに追加したときに投稿したメッセージがキューに含まれています。</span><span class="sxs-lookup"><span data-stu-id="d1d69-285">If all went according to plan, the queue includes the message that you posted when you added a bookmark to the database.</span></span> <span data-ttu-id="d1d69-286">つまり、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-286">It should look like the following:</span></span>

    ![キュー内のメッセージを示すスクリーンショット](../media/7-message-in-q-small.png)

   <span data-ttu-id="d1d69-288">この例では、メッセージに一意の ID が付与されており、**MESSAGE TEXT** フィールドに、使用しているブックマークが JSON 文字列の形式で表示されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-288">In this example, you can see that the message was given a unique ID, and the **MESSAGE TEXT** field displays your bookmark in JSON string format.</span></span>

5. <span data-ttu-id="d1d69-289">関数はさらにテストすることができます。それには、テスト ウィンドウ内で、新しい id/url を設定して要求本文を変更し、関数を実行します。</span><span class="sxs-lookup"><span data-stu-id="d1d69-289">You can test the function further by changing the request body in the test pane with new id/url sets and running the function.</span></span> <span data-ttu-id="d1d69-290">このキューを見ると、さらにメッセージが届いているのがわかります。</span><span class="sxs-lookup"><span data-stu-id="d1d69-290">Watch this queue to see more messages arrive.</span></span> <span data-ttu-id="d1d69-291">また、データベースを参照することで、新しいエントリが追加されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="d1d69-291">You can also look at the database to verify that new entries have been added.</span></span>

<span data-ttu-id="d1d69-292">このラボでは、バインディングに関する知識を出力バインディング (Azure Cosmos DB へのデータの書き込み) にまで広げました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-292">In this lab, we expanded your knowledge of bindings to output bindings, writing data to your Azure Cosmos DB.</span></span> <span data-ttu-id="d1d69-293">さらに先に進んで、Azure キューにメッセージを投稿するための別の出力バインディングを追加しました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-293">We went further and added another output binding to post messages to an Azure queue.</span></span> <span data-ttu-id="d1d69-294">これにより、データを成形してそれを受信ソースからさまざまな宛先に移動するのに役立つバインディングの真のパワーが実証されました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-294">This demonstrates the true power of bindings to help you shape and move data from incoming sources to a variety of destinations.</span></span> <span data-ttu-id="d1d69-295">データベース コードの作成は行いませんでした。また、接続文字列の管理を自分で行う必要もありませんでした。</span><span class="sxs-lookup"><span data-stu-id="d1d69-295">We haven't written any database code or had to manage connection strings ourselves.</span></span> <span data-ttu-id="d1d69-296">代わりに、バインディングを宣言によって構成し、接続のセキュリティ保護、関数のスケーリング、および接続のスケーリングをプラットフォームで行いました。</span><span class="sxs-lookup"><span data-stu-id="d1d69-296">Instead, we configured bindings declaratively and let the platform take care of securing connections, scaling our function, and scaling our connections.</span></span>