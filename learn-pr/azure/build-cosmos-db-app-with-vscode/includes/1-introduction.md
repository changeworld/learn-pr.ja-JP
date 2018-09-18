オンライン小売業者のストレージを管理しているとします。 ユーザーおよび製品データを作成、更新、削除するためのツールが必要です。

このモジュールでは、Visual Studio Code で .NET Core コンソール アプリケーションをビルドして、C# を使用してユーザー レコードの作成、更新、削除、データのクエリを行い、ストアド プロシージャを実行します。

データは Azure Cosmos DB に格納され、Azure Cosmos DB には便利な Visual Studio Code 拡張機能があるので、前のモジュールで作成したデータベース、コレクション、およびドキュメントを簡単に確認できます。また、この拡張機能を使用して新しいリソースを作成し、Azure portal を開くことなく接続文字列をコピーすることもできます。

## <a name="learning-objectives"></a>学習の目的

このモジュールでは、次のことを行います。  

- Azure Cosmos DB でデータの格納とクエリを行うアプリケーションを作成する
- Visual Studio Code で統合ターミナルを使用してコンソール アプリケーションを簡単に作成する
- Visual Studio Code 用 Azure Cosmos DB 拡張機能を利用して Azure Cosmos DB の機能を追加する

## <a name="prerequisites"></a>前提条件

- [Visual Studio Code](https://code.visualstudio.com/) をインストールしている必要があります
- [.NET Core 2.1](https://www.microsoft.com/net/download) がインストールされている必要があります
- [Azure アカウント](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)拡張機能が Visual Studio Code にインストールされている必要があります
