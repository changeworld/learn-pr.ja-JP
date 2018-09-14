ゾーン pivot ="csharp"を .NET Core コンソール アプリケーションに、Azure Storage クライアント ライブラリに統合しましょう。

.NET 用 Azure storage クライアント ライブラリは NuGet を使って配布されます。 追加するには、 **WindowsAzure.Storage** .NET または .NET Core アプリケーションをパッケージします。

## <a name="add-the-azure-storage-nuget-package"></a>Azure Storage NuGet パッケージを追加する

1. Cloud Shell で`cd`PhotoSharingApp ディレクトリされていない場合があります。

1. 追加、 **WindowsAzure.Storage**パッケージをアプリケーションにします。

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. これにより、クライアント ライブラリと必要なすべての依存関係がダウンロード中にいくつかのコンソールのアクティビティ。 これを完了すると、ビルドしてすべて準備が整っているかどうかを確認するには、もう一度アプリを実行します。

    ```bash
    dotnet run
    ```

1. 同様に、"Hello World!"と出力されます。

::: zone-end

::: zone pivot="javascript"

統合しましょう、 **Node.js、JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**をアプリケーションにします。

Node.js 用クライアント ライブラリは、ノード パッケージ マネージャー (NPM) を利用します。 追加するには、 **azure storage**をパッケージ化、 **packages.json**ファイル。

> [!NOTE]
> **Node.js、JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**サーバー アプリケーションが対象としています。 使用するとして、クライアント側の JavaScript を実行している場合、 **JavaScript 用の Azure Storage クライアント ライブラリ**は、ブラウザーで実行されていることにお応えが同じ機能を提供します。

## <a name="add-the-azure-storage-package"></a>Azure Storage パッケージを追加します。

1. Cloud Shell で`cd`PhotoSharingApp ディレクトリされていない場合があります。

1. 追加、 **azure storage**パッケージをアプリケーションにします。 指定することを確認、`--save`それを維持するためのオプション**packages.json**します。

    ```bash
    npm install azure-storage --save
    ```

1. これにより、クライアント ライブラリと必要なすべての依存関係がダウンロード中にいくつかのコンソールのアクティビティ。 これを完了すると、ビルドしてすべて準備が整っているかどうかを確認するには、もう一度アプリを実行します。

    ```bash
    node index.js
    ```

1. 同様に、「こんにちは, World!」と出力されます。

::: zone-end

場所に必要なライブラリをしたら、次のコードを Azure storage で作業を行い、一般的なタスクを見てみましょう。
