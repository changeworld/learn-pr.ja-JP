この時点では、アプリはユーザーの位置を取得しようとしています。また、アプリは Azure 関数に送信される準備ができています。 この演習では、Azure 関数を作成します。

## <a name="create-an-azure-functions-project"></a>Azure Functions プロジェクトを作成する

1. ソリューションを右クリックし、*[追加]、[新しいプロジェクト]* の順に選択して新しいプロジェクトを `ImHere` ソリューションに追加します。

1. 左側にあるツリーから、*[Visual C#]、[クラウド]* の順に選択し、中央のパネルから *[Azure Functions]* を選択します。

1. プロジェクトに "ImHere.Functions" という名前を付け、**[OK]** をクリックします。

    ![[新しいプロジェクトの追加] ダイアログ](../media/5-add-new-functions-project.png)

1. **[新しいプロジェクト]** 構成ダイアログが表示されます。これには更新されたテンプレートが読み込まれている間、左下にスピナーが表示される場合があります。 これが表示されたら、読み込みが完了するまでお待ちください。更新されたテンプレートが使用できるようになったら、**[更新]** オプションをクリックします。このオプションは最新の関数テンプレートを確実に取得できるようにするために表示されます。

    ![最新のテンプレートを読み込んでいる新しいプロジェクト ダイアログ](../media/5-loading-templates.png)

1. **[新しいプロジェクト]** 構成ダイアログで、Functions のバージョンが (_v1 (.NET Framework)_ **ではなく**) 確実に *Azure Functions v2 (.NET Standard)* に設定されているようにします。 *[Http トリガー]* を選択し、ストレージ アカウントを *[ストレージ エミュレーター]* に設定されているままにし、アクセス権を *[匿名]* に設定します。 次に、 **[OK]** をクリックします

    ![Azure 関数のプロジェクト構成ダイアログ](../media/5-configure-trigger.png)

    新しいプロジェクトが作成され、`Function1` という名前の既定の関数が与えられます。

> [!NOTE]
> この関数は匿名アクセスで作成されました。 Azure に公開されると、URL を知っている人は誰でもこの関数を呼び出すことができます。 実際のシナリオでは、[Azure App Service 認証](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true)または [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true) など、何らかの形式の認証でこれを保護します。

## <a name="create-the-function"></a>関数を作成する

Azure Functions プロジェクトは `Function1` という名前のシングル HTTP トリガー関数を使用して作成されます。 HTTP トリガーでは HTTP 要求を使用して関数を呼び出すことができます。 関数自体は `Function1` クラスに静的 `Run` メソッドとして実装されます。

1. ソリューション エクスプローラーのファイルの名前を "Function1.cs" から "SendLocation.cs" に変更します。 コード要素 `Function1` の参照の名前をすべて変更するように求められたら、**[はい]** をクリックします。

1. 属性の関数名を "SendLocation" に変更します。

    ```cs
    [FunctionName("SendLocation")]
    ```

1. ロガーに情報メッセージを書き込む最初の行を除き、関数の内容を削除します。

    ```cs
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                             "get", "post",
                                                             Route = null)]HttpRequestMessage req,
                                                ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a>モバイル アプリと関数の間でデータを共有するクラスを作成する

データが Azure 関数に送信されるとき、データは JSON として送信されます。 モバイル アプリによりデータが JSON にシリアル化され、関数により JSON から逆シリアル化されます。 モバイル アプリと関数の間でこのデータの整合性を維持するためには、場所と電話番号データを保持するクラスを含む新しいプロジェクトを作成します。 このプロジェクトはアプリと関数によって参照されます。

1. ソリューションを右クリックし、*[追加]、[新しいプロジェクト]* の順に選択して、`ImHere` ソリューションの下で新しいプロジェクトを作成します。

1. 左側にあるツリーから、*[Visual C#]、[.NET Standard]* の順に選択し、中央のパネルから *[クラス ライブラリ (.NET Standard)]* を選択します。

1. プロジェクトに "ImHere.Data" という名前を付け、**[OK]** をクリックします。

    ![[新しいプロジェクトの追加] ダイアログ](../media/5-add-new-net-standard-project.png)

1. 自動生成された "Class1.cs" ファイルを削除します。

1. プロジェクトを右クリックし、*[追加]、[クラス]* の順に選択して、`PostData` という名前の `ImHere.Data` プロジェクトで新しいクラスを作成します。新しいクラスに "PostData" という名前を付け、**[OK]** をクリックします。 この新しいクラスを `public` としてマークします。

1. 緯度と経度に `double` プロパティを、送信先の電話番号に `string[]` プロパティを追加します。

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. プロジェクトを右クリックし、*[追加]、[参照]* を選択して、このプロジェクトの参照を `ImHere.Functions` プロジェクトと `ImHere` プロジェクトの両方に追加します。左側にあるツリーから *[プロジェクト]* を選択し、*ImHere.Data* の隣にあるボックスをオンにします。

    ![プロジェクト参照を構成する](../media/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a>関数に送信されたデータを読み込む

Azure 関数では、`req` パラメーターには行われた HTTP 要求が含まれます。この要求の中にあるデータは JSON でシリアル化された `PostData` オブジェクトになります。

1. `ImHere.Functions` プロジェクトで `SendLocation` クラスを開きます。

1. HTTP 要求の内容を文字列に読み込んでから、それを `PostData` オブジェクトに逆シリアル化します。これにより、`ImHere.Data` 名前空間に対して using ディレクティブが追加されます。

    ```cs
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    ```

1. `PostData` からの緯度と経度を利用して Google Maps URL を作成します。

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. URL をログに記録します。

    ```cs
    log.LogInformation($"URL created - {url}");
    ```

1. 200 ステータス コードを返し、関数がエラーなく完了したことを示します。

    ```cs
    return new OkResult();
    ```

完全な関数を下に示します。

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                         Route = null)]HttpRequest req,
                                                    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");
    return new OkResult();
}
```

## <a name="run-the-azure-function-locally"></a>Azure 関数をローカルで実行する

関数は、ローカル ストレージ アカウントとローカル Azure Functions ランタイムを利用してローカルで実行できます。 このローカル ランタイムでは、Azure にデプロイする前に関数をテストできます。

1. ソリューション エクスプローラーで `ImHere.Functions` プロジェクトを右クリックし、*[スタートアップ プロジェクトに設定]* を選択します。

1. *[デバッグ]* メニューから *[デバッグなしで開始]* を選択します。 ローカル Azure Functions ランタイムはコンソール ウィンドウ内で起動され、これによってご利用の関数が開始され、`localhost` 上で利用できるポートがリッスンされます。 ファイアウォール アクセスを求めるダイアログが表示されたら、プライベート ネットワーク (既定のオプション) へのアクセスを許可します。

    ![ローカルで実行される Azure 関数](../media/5-function-running-locally.png)

1. 関数がリッスンしているポートをメモします。 これは次の演習でモバイル アプリをテストするために必要になります。 上記の図で、関数はポート **7071** でリッスンしています。

    ```sh
    Listening on http://localhost:7071/
    ```

1. 次の演習でモバイル アプリをテストできるように、関数は実行中のままにします。

## <a name="summary"></a>まとめ

この演習では、Visual Studio で Azure Functions プロジェクトを作成する方法を学習し、モバイル アプリと関数の間で共有するデータ オブジェクトを含む共有プロジェクトを追加し、渡されたデータを逆シリアル化する関数の基本実装を作成する方法を学習しました。 Azure 関数をローカルで実行する方法も学習しました。 次の演習では、モバイル アプリから Azure 関数を呼び出します。