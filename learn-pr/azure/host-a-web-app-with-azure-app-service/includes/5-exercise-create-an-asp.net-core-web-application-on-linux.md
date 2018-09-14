このユニットは、ASP.NET Core web アプリを作成します。

## <a name="create-a-new-web-project"></a>新しい Web プロジェクトを作成する

.NET CLI ツールの中核部分は、`dotnet`コマンド ライン ツール。 このコマンドを使用して、新しい ASP.NET Core Web プロジェクトを作成します。

1. 右側の Cloud Shell では、新しい ASP.NET Core MVC アプリケーションを作成します。 "BestBikeApp"名前を付けます。

```bash
dotnet new mvc -name BestBikeApp
```

1. "BestBikeApp"をという名前の新しいフォルダーを作成するコマンドは、プロジェクトを保持するためにします。 そのフォルダーに変更します。

1. ビルドし、それが完了することを確認するアプリケーションを実行します。

```bash
dotnet run
```

ようなものを取得する必要があります。

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

出力には、アプリを起動した後、状況がについて説明します。 アプリケーションが実行され、ポート 5000 でリッスンしています。

ブラウザーを開いて、アプリを独自のマシンで実行されていた場合は予定 http://localhost:5000します。 任意の場所にアプリを展開する必要がありますので、私たちは Azure Cloud Shell で、パブリック エンドポイントを使用します。 以前に作成した App Service インスタンスが最適です。