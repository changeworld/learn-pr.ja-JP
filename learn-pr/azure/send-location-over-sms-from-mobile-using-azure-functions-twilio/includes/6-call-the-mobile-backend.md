モバイル アプリが実行されます。Azure 関数の最初のバージョンが作成されました。 この演習では、モバイル アプリから Azure 関数を呼び出し、ユーザーの位置と、ユーザーが SMS メッセージを送信する電話番号の一覧を渡します。

## <a name="calling-the-azure-function-from-the-mobile-app"></a>モバイル アプリから Azure 関数を呼び出す

1. `MainViewModel` を開きます。

2. このクラスで、`client` という名前のプライベート `HttpClient` フィールドを追加します。 `System.Net.Http` 名前空間に参照を追加する必要があります。

    ```cs
    HttpClient client = new HttpClient();
    ```

3. 関数の基本 URL に定数フィールドを追加します。 ローカルの Azure Functions ランタイムがリッスンしているアドレスにこれを設定します。 関数が Azure にデプロイされたら、この定数を Azure URL に変更できます。

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

4. `SendLocation` メソッド内に、場所が見つかった後、場所とユーザーが入力した電話番号の一覧を利用して `PostData` の新しいインスタンスを作成します。 `ImHere.Data` 名前空間に対して using ディレクティブを追加する必要があります。

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > これは、`Editor` コントロールの行ごとに 1 つ、電話番号が正しい形式で入力されていることを前提とします。 運用品質のアプリでは、これに対して検証が行われ、1 つまたは複数の電話番号が入力されたこと、形式が正しいことが確認されるでしょう。

5. JSON として `PostData` をシリアル化する最も簡単な方法は、Newtonsoft.JSON NuGet パッケージを使用することです。 先の演習で Xamarin.Essentials を追加したときと同じ方法で `ImHere` プロジェクトにこの NuGet パッケージを追加します。

6. `JsonConvert` 静的クラスを使用して `PostData` を `string` にシリアル化します。 `Newtonsoft.Json` 名前空間に対して using ディレクティブを追加する必要があります。 JSON として Azure 関数に渡せるように、`StringContent` クラスにこの文字列をエンコードします。

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

7. このデータを関数に送り、結果を取得します。

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   Azure 関数は `/api/<function name>` の使用でアクセスされます。ローカル Functions ランタイムで選択されるポートが 7071 であると想定すると、`SendLocation` 関数には `http://localhost:7071/api/SendLocation` でアクセスできます。

8. 結果に応じて、UI にメッセージが表示されます。

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

新しいフィールドと `SendLocation` メソッドの完全なコードは下のようになります。

```cs
HttpClient client = new HttpClient();
const string baseUrl = "http://localhost:7071";

async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";

        PostData postData = new PostData
        {
            Latitude = location.Latitude,
            Longitude = location.Longitude,
            ToNumbers = PhoneNumbers.Split('\n')
        };

        string data = JsonConvert.SerializeObject(postData);
        StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
        HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                            content);

        if (result.IsSuccessStatusCode)
            Message = "Location sent successfully";
        else
            Message = $"Error - {result.ReasonPhrase}";
    }
}
```

## <a name="testing-it-out"></a>テスト

1. Azure 関数が引き続きローカルで実行されていること、ポートが `SendLocation` メソッドに一致することを確認します。

2. UWP アプリをスタートアップ アプリとして設定し、実行します。 **[Send Location]\(場所の送信\)** ボタンをクリックします。 Functions ランタイム コンソールに出力が表示され、呼び出されている関数を確認できます。ログには生成された URL が表示されます。

    ![呼び出されている関数の出力](../media/6-function-called.png)

3. URL 生成をテストするには、それをコンソールからブラウザーに貼り付けます。 現在の場所が表示されるはずです。

## <a name="summary"></a>まとめ

この演習では、モバイル アプリから Azure 関数を呼び出す方法について学習しました。 この呼び出しでは、ユーザーの場所と、JSON として入力した電話番号が渡されました。 次の演習では、Azure 関数を Twilio にバインドし、この場所を SMS メッセージとして送信します。