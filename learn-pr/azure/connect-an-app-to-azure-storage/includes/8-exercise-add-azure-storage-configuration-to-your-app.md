::: zone pivot="csharp" .NET Core アプリケーションにサポートを追加して、構成ファイルから接続文字列を取得しましょう。 JSON ファイルで構成を管理するために必要なプラミングを追加することから始めます。

## <a name="create-a-json-configuration-file"></a>JSON 構成ファイルを作成する

1. プロジェクトの正しい作業ディレクトリで操作していることを確認してください。

1. コマンド ラインで `touch` ツールを使用して、**appsettings.json** という名前のファイルを作成します。

    ```bash
    touch appsettings.json
    ```

1. 対話型エディターを使用してプロジェクトを開きます。ローカルで作業している場合は、任意のエディターを使用します。拡張可能なクロスプラットフォーム IDE である **Visual Studio Code** をお勧めします。 次のコマンドは、Cloud Shell エディター用ですが、VS Code でもほぼ同じです。
    
    ```bash
    code .
    ```

1. エディターで **appsettings.json** ファイルを選択し、次のテキストを追加します。 ファイルを保存します。オンライン エディターでは、ページの右上に一般的なファイル操作を含むメニューがあります。

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. 次に、プロジェクト ファイル (**PhotoSharingApp.csproj**) を選択して、エディターで開きます。

1. プロジェクトに新しいファイルを含めるために次の構成ブロックを追加し、それを出力フォルダーにコピーします。 これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。

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

1. ファイルを保存します。 (これは必ず行ってください。そうしないと、以下のパッケージを追加するときの変更内容が失われます。)

## <a name="add-support-to-read-a-json-configuration-file"></a>JSON 構成ファイルを読み込むためのサポートを追加する

.NET Core アプリケーションでは、JSON 構成ファイルを読み取るために追加の NuGet パッケージが必要です。

1. ウィンドウのコマンド プロンプト セクションで、**Microsoft.Extensions.Configuration.Json** NuGet パッケージへの参照を追加します。

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>構成ファイルを読み取るためのコードを追加する

読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。

1. エディターで **Program.cs** を選択します。

1. ファイルの一番上に **using System;** 行があります。 その行の下に次のコード行を追加します。

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. **Main** メソッドの内容を次のコードに置き換えます。 このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。

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

::: zone-pivot="javascript"

Node.js アプリケーションにサポートを追加して、構成ファイルから接続文字列を取得しましょう。 JavaScript ファイルから構成を管理するために必要なプラミングを追加することから始めます。

## <a name="create-a-env-configuration-file"></a>.env 構成ファイルを作成する

1. プロジェクトの正しい作業ディレクトリで操作していることを確認してください。

1. コマンド ラインで `touch` ツールを使用して、**.env** という名前のファイルを作成します。

    ```bash
    touch .env
    ```

1. 対話型エディターを使用してプロジェクトを開きます。ローカルで作業している場合は、任意のエディターを使用します。拡張可能なクロスプラットフォーム IDE である **Visual Studio Code** をお勧めします。 次のコマンドは、Cloud Shell エディター用ですが、VS Code でもほぼ同じです。
    
    ```bash
    code .
    ```

1. エディターで **.env** ファイルを選択し、次のテキストを追加します。 ファイルを保存します。オンライン エディターでは、ページの右上に一般的なファイル操作を含むメニューがあります。

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > **AZURE_STORAGE_CONNECTION_STRING** 値は、Storage API でアクセス キーを検索するために使用される、ハード コーディングされた環境変数です。 必要に応じて独自の名前を使用することもできますが、`BlobService` オブジェクトの作成時の名前を指定する必要があります。

1. ファイルを保存します。

## <a name="add-support-to-read-an-environment-configuration-file"></a>環境構成ファイルを読み取るためのサポートを追加する

Node.js アプリに **.env** ファイルから読み取るためのサポートを含めるには、**dotenv** パッケージを追加します。

1. ウィンドウのコマンド プロンプト セクションで、依存関係を *dotenv** パッケージに追加します。

    ```bash
    node install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>構成ファイルを読み取るためのコードを追加する

構成の読み取りを有効にするために必要なライブラリが追加されたので、アプリケーション内でその機能を有効にする必要があります。

1. エディターで *index.js** を選択します。

1. ファイルの一番上に **#!/usr/bin/env node** 行があります。 その行の下に、**dotenv** パッケージを読み込むための `require` ステートメントを追加します。 これにより、**.env** ファイル内で定義された環境変数がプログラムで使用できるようになります。

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

## <a name="add-the-connection-string-to-the-configuration-file"></a>構成ファイルに接続文字列を追加する

次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。

1. [Azure Portal](https://portal.azure.com/?azure-portal=true) にサインインします。

1. ストレージ アカウントに移動します。 **[すべてのリソース]** セクションを使用して、ストレージ アカウントを検索することができます。また、ポータル ウィンドウの上部にある_検索ボックス_から名前で検索することもできます。 

1. ポータルでストレージ アカウントの **[アクセス キー]** ブレードを選択します。

1. **key1** 接続文字列をコピーします。

1. 接続文字列構成変数の値としてポータルからコピーしたアクセス キーの内容を貼り付けます。

これで構成が次のようになるはずです。

::: zone pivot="csharp"
    ```json
    {
        "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
    }
    ```
::: zone-end

::: zone-pivot="javascript"
    ```
    AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
    ```
::: zone-end

これですべて接続したので、ストレージ アカウントを使用するためのコードの追加を開始できます。