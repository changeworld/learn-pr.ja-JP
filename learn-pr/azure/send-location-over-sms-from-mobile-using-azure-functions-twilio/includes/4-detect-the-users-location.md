アプリには UI と ViewModel があります。 この演習では、Xamarin.Essentials を使用して、ViewModel に場所の参照を追加します。

## <a name="enable-location-permissions"></a>場所のアクセス許可を有効にする

すべてのモバイル プラットフォームでは、カメラ、フォト ライブラリ、ユーザーの場所など、ユーザーの情報と特定のハードウェアに関するセキュリティが適用されます。 アプリからユーザーの場所にアクセスできるようにするには、ユーザーはアクセス許可を付与する必要があります。その場合、インストール時に暗黙的にアクセス許可を付与するか、実行時にアクセス許可を付与するように選択します。 ストアで UWP アプリを表示すると、一覧にアプリで必要なアクセス許可が表示されます。 アプリをインストールすることで、アクセス許可を暗黙的に付与します。 これらのアクセス許可はアプリのマニフェスト ファイルで構成されます。

1. `ImHere.UWP` アプリ プロジェクトで、`Package.appxmanifest` ファイルを開きます。

2. **[機能]** タブに移動して、*位置情報*機能をオンにします。

    ![UWP の [機能] タブ](../media/4-uwp-location-capability.png)

> Android や iOS をサポートする場合は、アクセス許可を個別に構成する必要があります。 詳細については、[Xamarin.Essentials の地理的位置情報に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started)を参照してください。

## <a name="query-for-the-users-location"></a>ユーザーの場所をクエリする

ユーザーの場所を取得するには、2 つの方法があります。つまり、最新の場所または現在の場所を取得します。 現在の場所を取得するには時間がかかる場合があります。これは、デバイスで GPS リンクを確立し、正確な場所が取得されるまで待機する必要がある場合があるためです。 最も簡単な方法は、デバイスによって検出された最新の場所を取得することです。 最新の場所は正確性に欠ける可能性がありますが、呼び出しははるかに速くなります。 場所は緯度と経度 ([10 進経緯度](https://en.wikipedia.org/wiki/Decimal_degrees)に関する記述を参照) で表され、デバイスの海抜高度 (メートル単位) が示されます。

1. `ImHere` .NET 標準プロジェクトで `MainViewModel` クラスを開きます。

2. `SendLocation` メソッドで、`Xamarin.Essentials` 名前空間の `Geolocation` クラスに対して `GetLastKnownLocationAsync` 静的メソッドを呼び出します。

    ```cs
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

3. ユーザーの場所が見つかった場合は、その場所で `Message` プロパティを更新します。

    ```cs
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

このメソッドの完全なコードは次のとおりです。

```cs
async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
}
```

アプリを実行し、**[場所の送信]** ボタンをクリックして UI で場所を表示します。

![ユーザーの場所を示す実行中のアプリ](../media/4-running-app-showing-location.png)

> このアプリでは最新の場所を使用します。 運用品質アプリで、タイムアウトを設定して現在の正確な場所を取得する必要があります。時間内に見つからない場合は、最新の場所にフォールバックします。 これを行う方法の詳細については、[Xamarin.Essentials の地理的位置情報に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation)を参照してください。このアプリではエラー処理は行われません。 運用品質アプリで、発生した例外を処理する必要があります。たとえば、場所を使用できなかった場合や、例外がスローされた場合です。

## <a name="summary"></a>まとめ

この演習では、Xamarin.Essentials を使用して、ユーザーの場所を取得する方法を学習しました。 次の演習では、モバイル アプリ用のバック エンドとして動作する Azure 関数を作成します。