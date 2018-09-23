このユニットでは、ASP.NET Core Web アプリを作成します。

## <a name="create-a-new-web-project"></a>新しい Web プロジェクトを作成する

.NET CLI ツールの中心になるのは、`dotnet` コマンド ライン ツールです。 このコマンドを使用して、新しい ASP.NET Core Web プロジェクトを作成します。

右側の Cloud Shell で、新しい ASP.NET Core MVC アプリケーションを作成します。 それに "BestBikeApp" という名前を付けます。

```bash
dotnet new mvc --name BestBikeApp
```

コマンドにより、プロジェクトを保持するための "BestBikeApp" という名前の新しいフォルダーが作成されます。 `cd` 次に、アプリケーションをビルドして実行し、完了したことを確認します。

```bash
cd BestBikeApp
dotnet run
```

次のように表示されます。

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

出力ではアプリ開始後の状況が説明されます。アプリケーションは実行され、ポート 5000 でリッスンしています。

独自のコンピューター上でアプリを実行していた場合は、ブラウザーで http://localhost:5000 を開くことができます。 ここでは Azure Cloud Shell を使用しているため、パブリック エンドポイントで任意の場所にアプリをデプロイする必要があります。 その場合、以前に作成した App Service インスタンスが最適です。