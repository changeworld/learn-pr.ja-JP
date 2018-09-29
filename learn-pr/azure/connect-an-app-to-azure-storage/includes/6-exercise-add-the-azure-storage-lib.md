::: zone pivot="csharp" Azure Storage クライアント ライブラリを .NET Core コンソール アプリケーションに統合しましょう。

.NET 用 Azure Storage クライアント ライブラリは NuGet で配布されます。 **WindowsAzure.Storage** パッケージをご利用の .NET または .NET Core アプリケーションに追加する必要があります。

## <a name="add-the-azure-storage-nuget-package"></a>Azure Storage NuGet パッケージを追加する

1. ターミナルで、PhotoSharingApp ディレクトリに `cd` します (まだそのディレクトリに移動していない場合)。

1. **WindowsAzure.Storage** パッケージをアプリケーションに追加します。

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. これにより、クライアント ライブラリと必要なすべての依存関係がダウンロードされている間、一部のコンソール アクティビティが示されます。 完了したら、先に進み、もう一度アプリをビルドして実行し、すべて準備が整ったことを確認します。

    ```bash
    dotnet run
    ```

1. 先ほどと同様に、"Hello, World!" と出力されます。

::: zone-end

::: zone pivot="javascript"

**Node.js と JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**をアプリケーションに統合しましょう。

Node.js 用のクライアント ライブラリは、ノード パッケージ マネージャー (NPM) を通じて利用できます。 **azure-storage** パッケージを **packages.json** ファイルに追加する必要があります。

> [!NOTE]
> **Node.js と JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**はサーバー アプリケーション用です。 クライアント側の JavaScript を実行する場合は、**JavaScript 用の Azure Storage クライアント ライブラリ** を使用する必要があります (提供される機能は同じですが、ブラウザーでの実行用に調整されています)。

## <a name="add-the-azure-storage-package"></a>Azure Storage パッケージを追加する

1. Cloud Shell で、PhotoSharingApp ディレクトリに `cd` します (まだそのディレクトリに移動していない場合)。

1. **azure-storage** パッケージをアプリケーションに追加します。 必ず、`--save` オプションを指定して、**packages.json** に保持されるようにしてください。

    ```bash
    npm install azure-storage --save
    ```

1. これにより、クライアント ライブラリと必要なすべての依存関係がダウンロードされている間、一部のコンソール アクティビティが示されます。 完了したら、先に進み、もう一度アプリをビルドして実行し、すべて準備が整ったことを確認します。

    ```bash
    node index.js
    ```

1. 先ほどと同様に、"Hello, World!" と出力されます。

::: zone-end

これで必要なライブラリが準備できました。次は、Azure Storage を操作するためにコードで行う一般的なタスクを見てみましょう。
