構築しているアプリケーションは、フォト ギャラリーです。 このアプリケーションでは、クライアント側 JavaScript を使用して API を呼び出し、画像をアップロードおよび表示します。 このユニットは、イメージをアップロードする時間制限付き URL を生成するサーバーレス関数を使用して API を作成します。 Web アプリケーションでは、この URL を使用して、使用して Blob ストレージにイメージをアップロード、 [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api)します。

## <a name="create-a-blob-storage-container-for-images"></a>画像用の Blob Storage コンテナーを作成する

アプリケーションには、画像をアップロードしてホストするために、別個のストレージ コンテナーが必要です。

1. Azure Cloud Shell (Bash) に引き続きサインインしていることを確認します。 そうでない場合は、**[Enter focus mode]\(フォーカス モードにする\)** を選択して、Cloud Shell ウィンドウを開きます。

1.  という名前のストレージ アカウントで新しいコンテナーを作成**イメージ**のすべての blob へのパブリック アクセスを使用してストレージ アカウントにします。

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-a-function-app"></a>関数アプリの作成

Azure Functions はサーバーレス関数を実行するためのサービスです。 サーバーレス関数は、HTTP 要求などのイベントによって、または BLOB がストレージ コンテナーに作成されたときに、トリガーする (呼び出す) ことができます。

関数アプリは、サーバーレス関数を 1 つまたは複数のコンテナーです。

- 一意の名前で新しい function app を作成、**サーバーレス アプリの最初**先ほど作成したリソース グループ。 関数アプリでは、ストレージ アカウントが必要です。 このユニットでは、最後の単位で作成した既存のストレージ アカウントを使用します。

    ```azurecli
    az functionapp create -n <function app name> -g <rgn>[Sandbox resource group name]</rgn> -s <storage account name>
    ```

## <a name="create-an-http-triggered-serverless-function"></a>HTTP によってトリガーされるサーバーレス関数を作成する

画像を Blob Storage に安全にアップロードするため、フォト ギャラリー Web アプリでは、サーバーレス関数に HTTP 要求を行い、時間制限付き URL を生成します。 関数は HTTP 要求によってトリガーされます。この関数では Azure Storage SDK が使用されます。これにより、セキュリティで保護された URL が生成され、返されます。

1. Function app を作成すると後でそれを検索、 [Azure portal](https://portal.azure.com/?azure-portal=true)を使用して、**検索**ボックス。 アプリをクリックして開きます。

    ![関数アプリを開く](../media/2-search-function-app.png)

1. ポイントして、関数アプリのウィンドウの左側のナビゲーションで**関数**新しいサーバーレス関数を作成するには、プラス記号 (+) をクリックします。

    ![新しい関数を作成する](../media/2-new-function.png)

1. **[カスタム関数]** をクリックして、関数テンプレートの一覧を表示します。

1. 検索、 **HttpTrigger**テンプレート (C#) または JavaScript をクリックします。

1. 次の値を使用して、BLOB のアップロード URL を生成する関数を作成します。

    | 設定      |  推奨値   | 説明                                        |
    | --- | --- | ---|
    | **言語** | C# または JavaScript | 使用する言語を選びます。 |
    | **関数名の指定** | GetUploadUrl | アプリケーションから関数を検出できるように、この名前を、表示されているとおりに入力します。 |
    | **承認レベル** | Anonymous | により、パブリックにアクセスする関数。 |

    ![HTTP によってトリガーされる新しい関数の設定を入力する](../media/2-new-function-httptrigger.png)

1. **[作成]** をクリックして、関数を作成します。

::: zone pivot="csharp"
1. (C#)関数のソース コードが表示されたら、すべてのコンテンツを置換、 **run.csx**でコンテンツを含むファイル、 [ **csharp/GetUploadUrl/run.csx** ](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx)ファイル。

::: zone-end

::: zone pivot="javascript"
1. (JavaScript) この関数には、npm の `azure-storage` パッケージが必要です。 このパッケージによって、セキュリティで保護された URL を構築するために必要な Shared Access Signature (SAS) トークンが生成されます。 npm パッケージをインストールするには、左側のナビゲーションで Functions アプリをクリックし、**[プラットフォーム機能]** をクリックします。

1. (JavaScript) **[コンソール]** をクリックしてコンソール ウィンドウを表示します。

    ![コンソール ウィンドウを開く](../media/2-open-console.jpg)

1. (JavaScript) `cd d:\home\site\wwwroot` コマンドを実行して、現在のディレクトリが **d:\home\site\wwwroot** であることを確認します。

1. (JavaScript) `npm init -y` コマンドを実行して、空の **package.json** ファイルを作成します。

1. (JavaScript) パッケージをインストールするには、コンソールでコマンド `npm install --save azure-storage` を実行します。 パッケージを **package.json** として保存します。 操作が完了するまで数分かかることがあります。

1. (JavaScript) 左側のナビゲーションで関数 (**GetUploadUrl**) をクリックして、関数を表示します。 **index.js** ファイルのすべての内容を、[**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) ファイルの内容で置き換えます。

    ![更新後の index.js の内容](../media/2-paste-js.jpg)

::: zone-end

1. コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。

1. **[保存]** をクリックします。 関数が正常にコンパイルされていることを、ログ パネルで確認します。

関数は、Blob storage にファイルをアップロードするために使用する共有アクセス署名 (SAS) URL を生成します。 SAS URL は短時間有効で、アップロードする 1 つのファイルのみを許可します。 詳細については、Blob storage のドキュメントを参照してください[shared access signature を使用する方法](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)します。


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>ストレージ接続文字列に環境変数を追加する

作成した関数で SAS URL を生成できるようにするには、ストレージ アカウントの接続文字列が必要です。 接続文字列は、関数本体でハードコーディングするのではなく、アプリケーション設定として格納できます。 アプリケーションの設定では、関数アプリ内のすべての関数で環境変数としてアクセスできます。

1. Cloud Shell で、ストレージ アカウント接続文字列のクエリを実行し、**STORAGE_CONNECTION_STRING** という名前の Bash 変数に保存します。

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    変数が正常に設定されていることを確認します。

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. 前の手順で保存した値を使用して、**AZURE_STORAGE_CONNECTION_STRING** という名前の新しいアプリケーション設定を作成します。

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    コマンドの出力に、正しい値が指定された新しいアプリケーション設定が含まれていることを確認します。


## <a name="test-the-serverless-function"></a>サーバーレス関数をテストする

Azure portal には、関数の作成と編集だけでなく、関数をテストするツールも組み込まれています。

1. HTTP サーバーレス関数をテストするには、コード ウィンドウの右側にある **[テスト]** タブをクリックして、テスト パネルを展開します。

1. **[HTTP メソッド]** を **[GET]** に変更します。

1. **[クエリ]** の下の **[パラメーターの追加]** をクリックし、次のパラメーターを追加します。

    | 名前      |  値   | 
    | --- | --- |
    | **filename** | image1.jpg |

1. テスト パネルで **[実行]** をクリックして、HTTP 要求を関数に送信します。

1. 関数の出力にアップロード URL が返されます。 関数の実行は、ログ パネルに表示されます。

    ![関数が正常に実行されたことを示すログ](../media/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a>関数アプリでの CORS を構成する

関数のフロント エンドが Blob storage でホストされているため、関数アプリの別のドメイン名があります。 正常に作成した関数を呼び出すクライアント側の JavaScript 関数アプリは、クロス オリジン リソース共有 (CORS) 用に構成する必要があります。

1. 関数アプリのウィンドウの左側のナビゲーションで、関数アプリの名前をクリックします。

1. **[プラットフォーム機能]** をクリックして、高度な機能の一覧を表示します。

1. **[API]** で **[CORS]** をクリックします。

    ![CORS を選択する](../media/2-open-cors.jpg)

1. 前の単位で作成した web サイトの URL を許可配信元を追加し、末尾にスラッシュ (/) を省略します。 たとえば、「`https://firstserverlessweb.z4.web.core.windows.net`」のように入力します。

    ![追加されたサーバーレス Web アプリの URL を示す CORS 設定](../media/2-add-cors.png)

1. **[保存]** をクリックします。

::: zone pivot="csharp"
1. (C#) `GetUploadUrl` 関数に戻り、**[統合]** タブを選択します。

1. (C#) **[選択した HTTP メソッド]** で、**[OPTIONS]** を選択します。

    **[GET]**、**[POST]**、および **[OPTIONS]** がすべて選択されている必要があります。 CORS では **OPTIONS** メソッドが使用されます。C# 関数の既定では、これは選択されていません。  

1. (C#) **[保存]** をクリックします。

::: zone-end

1. 引き続き、Azure portal で function app に移動します。 **[概要]** タブを選択します。**[再起動]** をクリックして、CORS への変更が有効になっていることを確認します。

## <a name="configure-cors-in-the-storage-account"></a>ストレージ アカウントで CORS を構成する

関数アプリでは、ファイルをアップロードする Blob storage へのクライアント側 JavaScript の呼び出しでためには、CORS のストレージ アカウントを構成する必要があります。

- 次のコマンドを実行して、すべてのオリジンが、ファイルをストレージ アカウントにアップロードできるようにします。

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Web アプリを変更して画像をアップロードする

Web アプリでは、**settings.js** という名前のファイルから設定を取得します。 次の手順では、Cloud Shell を使用してファイルを作成します。 設定する`window.apiBaseUrl`、関数アプリの url と`window.blobBaseUrl`Azure Blob ストレージ エンドポイントの URL にします。

1. Cloud Shell で、現在のディレクトリが **www/dist** フォルダーであることを確認します。

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. コマンドを入力して、Cloud Shell のエディターを開く`code`します。

    ```azurecli
    code
    ```

1. Cloud Shell ウィンドウで、エディターの下で、関数アプリの URL をクエリします。

    ```azurecli
    echo "https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

1. 前の手順で取得した関数アプリの URL を使用して、エディター ウィンドウに次の行を追加します。

    ```
    window.apiBaseUrl = '<function app url>'
    ```

1. Cloud Shell ウィンドウで、エディターの下で、Azure Blob Storage エンドポイントの URL をクエリします。

    ```azurecli
    echo $(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

1. 前の手順で取得したストレージ エンドポイントの URL を使用して、エディター ウィンドウに 2 つ目の行を追加します。

    ```
    window.blobBaseUrl = '<blob storage endpoint url>'
    ```

1. ファイルに保存します**settings.js**エディターを閉じます。

1. ファイルが正常に書き込まれたこと、および 2 行が含まれていることを確認します。

    ```azurecli
    cat settings.js
    ```

1. ファイルを Blob Storage にアップロードします。

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```

## <a name="test-the-web-application"></a>Web アプリケーションをテストする

この時点で、ギャラリー アプリケーションでは Blob Storage に画像をアップロードできますが、まだ画像は表示できません。 `GetImages` 関数の呼び出しが試行されますが、この関数はまだ存在しません。これは後のモジュールで作成します。 この呼び出しは失敗し、Web ページには "分析中…" と表示されたままになりますが、選択する画像は正常にアップロードされます。

画像が正常にアップロードされていることを確認するには、Azure portal で **images** コンテナーの内容を確認します。

1. ブラウザー ウィンドウで、アプリケーションを参照します。 画像ファイルを選択してアップロードします。 アップロードが完了しますが、画像を表示する機能はまだ追加していないため、アップロードした写真はアプリに表示されません  (Web ページには "画像分析中..." と表示されたままになります。これは後で修正します)。

1. Cloud Shell で、**images** コンテナーに画像がアップロードされていることを確認します。

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. 次のチュートリアルに進む前に、**images** コンテナー内のすべてのファイルを削除します。

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```

## <a name="summary"></a>まとめ

このユニットでは、Azure Functions アプリを作成しました。また、サーバーレス関数を使用して、Web アプリケーションで画像を Blob Storage にアップロードできるようにする方法を学習しました。 次に、blob によってトリガーされるサーバーレス関数を使用してアップロードされた画像のサムネイルを作成する方法を学習します。