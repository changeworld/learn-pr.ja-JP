ゾーン pivot ="csharp"は、構成ファイルから接続文字列を取得するように .NET core アプリケーションをサポートを追加しましょう。 JSON ファイルで構成を管理するために必要なプラミングを追加することから始めます。

## <a name="create-a-json-configuration-file"></a>JSON 構成ファイルを作成する

1. プロジェクトの正しい作業ディレクトリにいることを確認します。

1. 使用して、`touch`という名前のファイルを作成するコマンド ライン ツール**appsettings.json**します。

    ```bash
    touch appsettings.json
    ```

1. 対話型のエディターでプロジェクトを開きます。 ローカルで作業している場合は、任意のエディターを使用します。 お勧め**Visual Studio Code**、拡張可能なクロスプラット フォーム IDE であります。 次のコマンドは、Cloud Shell エディター向けですが、VS Code によく似ています。

    ```bash
    code .
    ```

1. 選択、 **appsettings.json**エディターでファイルを開き、次のテキストを追加します。 ファイルを保存します。 オンライン エディターで、一般的なファイル操作が右上隅のメニューですがあります。

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. 次に、プロジェクト ファイルを選択 (**PhotoSharingApp.csproj**)、エディターで開きます。

1. プロジェクトに新しいファイルを含めるし、出力フォルダーにコピーする次の構成ブロックを追加します。 これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. ファイルを保存します。 (これを行うか、以下のパッケージを追加すると、変更が失われますことを確認してください。)

## <a name="add-support-to-read-a-json-configuration-file"></a>JSON 構成ファイルを読み込むためのサポートを追加する

.NET Core アプリケーションでは、JSON 構成ファイルの読み取りに NuGet パッケージの追加が必要です。

1. ウィンドウの [コマンド プロンプト] セクションへの参照を追加、 **Microsoft.Extensions.Configuration.Json** NuGet パッケージ。

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>構成ファイルを読み取るコードを追加します。

読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。

1. 選択**Program.cs**エディターでします。

1. ファイルの一番上に **using System;** 行があります。 その行の下に次のコード行を追加します。

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. 内容を置き換える、 **Main**メソッドを次のコード。 このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

**Program.cs** ファイルは次のようになります。

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();            
        }
    }
}
```

::: zone-end

::: zone pivot="javascript"

Node.js アプリケーション構成ファイルから接続文字列を取得するには、サポートを追加してみましょう。 JavaScript ファイルから構成を管理するために必要なプラミングを追加することから始めます。

## <a name="create-a-env-configuration-file"></a>.Env 構成ファイルを作成します。

1. プロジェクトの正しい作業ディレクトリにいることを確認します。

1. 使用して、`touch`という名前のファイルを作成するコマンド ライン ツール **.env**します。

    ```bash
    touch .env
    ```

1. オープン、対話型エディターを使用してプロジェクトをローカルで作業している場合を使用して、任意のエディターをお勧め**Visual Studio Code**クロスプラット フォーム対応の拡張可能な IDE であります。 次のコマンドは、Cloud Shell のエディターしますが、VS Code とよく似ています。
    
    ```bash
    code .
    ```

1. 選択、 **.env**エディターでファイルを開き、次のテキストを追加します。 メニューがファイルを保存、-オンライン エディターでは、ページの右上にあるが一般的なファイル操作であります。

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > **AZURE_STORAGE_CONNECTION_STRING**値は、ハード コーディングされた環境変数を Storage Api のアクセス キーを検索するために使用します。 独自の名前を使用するには、必要に応じてが、名前を指定する必要がありますを作成するときに、`BlobService`オブジェクト。

1. ファイルを保存します。

## <a name="add-support-to-read-an-environment-configuration-file"></a>環境の構成ファイルを読み取るサポートを追加します。

Node.js アプリからの読み取りのサポートを含めることができます、 **.env**ファイルを追加することで、 **dotenv**パッケージ。

1. ウィンドウの [コマンド プロンプト] セクションで追加する依存関係、 *dotenv** を使用してパッケージ化`npm`します。

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>構成ファイルを読み取るコードを追加します。

構成の読み取りを有効にする必要なライブラリを追加しましたが、アプリケーション内でその機能を有効にする必要があります。

1. 選択*index.js**、エディターでします。

1. ファイルの上部にある、 **#!/usr/bin/env ノード**行が存在します。 、その行の下に追加、`require`を読み込むステートメント、 **dotenv**パッケージ。 定義されている環境変数をこのように、 **.env**ファイルをプログラムに使用します。

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

## <a name="add-the-connection-string-to-the-configuration-file"></a>構成ファイルへの接続文字列を追加します。

次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。

1. [Azure Portal](https://portal.azure.com/?azure-portal=true) にサインインします。

1. ストレージ アカウントに移動します。 使用することができます、**すべてのリソース**、ストレージ アカウントを検索するセクションまたはから名前で検索することができます、_検索ボックス_ポータル ウィンドウの上部にあります。

1. 選択、**アクセスキー**ポータルでストレージ アカウントのブレード。

1. コピー、 **key1**接続文字列。

1. 接続文字列の構成変数の値として、ポータルからコピーしたアクセス キーの内容を貼り付けます。

これで構成が次のようになるはずです。

::: zone pivot="csharp"
```json
{
    "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
}
```
::: zone-end

::: zone pivot="javascript"
```
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
```
::: zone-end

できたことすべてワイヤード (有線)、ストレージ アカウントを使用するコードを追加し始めることができます。