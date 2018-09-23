キューを操作するクライアント アプリケーションを作成しましょう。 次に、コードに接続文字列を追加します。

> [!NOTE]
> .NET Core をインストールしている場合、クライアント アプリケーションはローカル コンピューターで作成できます。あるいは、Cloud Shell 環境で直接作成できます。

## <a name="create-the-application"></a>アプリケーションを作成する

Linux、macOS、または Windows で実行できる .NET Core アプリケーションを作成します。 これに **QueueApp** という名前を付けましょう。 わかりやすくするため、キューを介してメッセージの受信と送信の両方を行う単独のアプリを使用します。

1. `dotnet new` コマンドを使用して、**QueueApp** という名前の新しいコンソール アプリを作成します。 コマンドの入力は右側の Cloud Shell か、ローカルで作業している場合はターミナル/コンソール ウィンドウで行うことができます。 このコマンドにより、1 つのソース ファイルによる、シンプルなアプリが作成されます: `Program.cs`。

    ```azurecli
    dotnet new console -n QueueApp
    ```

1. 新しく作成された `QueueApp` フォルダーに切り替え、アプリをビルドして、すべて問題ないことを確認します。

    ```azurecli
    cd QueueApp
    ```

    ```azurecli
    dotnet build
    ```

## <a name="get-your-connection-string"></a>接続文字列を取得する

Azure Portal のご自分のストレージ アカウントの **[設定] > [アクセス キー]** セクションで、ご自分の接続文字列が使用できることを思い出してください。

Azure CLI または PowerShell のツールから取得することもできます。 Azure CLI のコマンドを使用しましょう。 `<name>` はご自分で作成したストレージ アカウントの名前に置き換えてください。 確認が必要な場合は、`az storage account list` を使用できます。

```azurecli
az storage account show-connection-string --name <name> --resource-group <rgn>[Sandbox resource group name]</rgn>
```

このコマンドは、ご自分の接続文字列が含まれる JSON ブロックを返します。 ストレージ アカウント名とアカウント キーが含まれています。

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=<name>;AccountKey=vyw6aKz2PtSAgQ4ljJQgJFgxbCETdXt39ZyYQ5fLqoBJj/gT+43TbrhoVco7Rqj/AAJVlvFORRfnYqGHiX9QcQ=="
}
```

## <a name="add-the-connection-string-to-the-application"></a>接続文字列をアプリケーションに追加する

最後に、接続文字列がストレージ アカウントにアクセスできるように、アプリに追加しましょう。

> [!WARNING]
> わかりやすくするため、接続文字列を **Program.cs** ファイルに配置します。 実稼働アプリケーションでは、安全な場所に格納する必要があります。 サーバー側で使用するために、Azure Key Vault の使用をお勧めします。

1. ターミナルに `code .` と入力して、オンライン コード エディターを開きます。 または、自分で操作する場合は、任意の IDE を使用できます。 優れたクロスプラットフォーム IDE である Visual Studio Code をお勧めします。

1. プロジェクトの `Program.cs` ソース ファイルを開きます。

1. 接続文字列を保持するため、`Program` クラスに `const string` の値を追加します。 必要なのはこの値のみです (**DefaultEndpointsProtocol** のテキストで始まります)。

1. ファイルを保存します。 クラウド エディターの右隅にある省略記号 "..." をクリックするか、アクセラレータ キーを使用できます (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Command + S</kbd>)。

次のようなコードが表示されるはずです (文字列値は、アカウントに対して一意になります)。

```csharp
...
namespace QueueApp
{
    class Program
    {
        private const string ConnectionString = "DefaultEndpointsProtocol=https; ...";
        
        ...
    }
}
```

このスタート プロジェクトがセットアップされたので、コードでキューを操作する方法を見てみましょう。 すべて_メッセージ_で始まります。