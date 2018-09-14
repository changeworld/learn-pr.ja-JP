ゾーン pivot ="csharp"が構成から接続文字列を取得し、Azure ストレージ アカウントへの接続に使用するコードを追加してみましょう。

## <a name="retrieve-the-connection-string"></a>接続文字列を取得する

1. 選択**Program.cs**コード エディターで開きます。

1. 追加、`using`参照するファイルの上部にあるステートメント、`Microsoft.WindowsAzure.Storage`名前空間。

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. 最後に、`Main`メソッドでは、構成ファイルから Azure storage アカウント接続文字列を取得するには、次の行を追加します。 渡された_キー_で使用される名前に一致する必要があります、 **appsettings.json**ファイル。

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>BLOB クライアントを作成する

1. 静的なを使用して、`CloudStorageAccount.TryParse`メソッドを作成する、`CloudStorageAccount`オブジェクト - 接続文字列と`out`パラメーターを作成したオブジェクトを返します。 返します、`bool`に正常に文字列を解析するかどうかを示す値。
    - 失敗した場合は、コンソールにメッセージを出力し、メソッドから返します。

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. 使用して、返された`CloudStorageAccount`blob クライアントを作成するオブジェクト。

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. 次に、blob クライアントを使用して、"photoblobs"という名前のコンテナーへの参照を取得します。 はるかなどのアカウント名を Blob コンテナー名がありますの文字と数字で構成され、小文字。

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. 使用して、`CreateIfNotExistsAsync`コンテナーを作成します。 これにより返されます、`bool`コンテナーが作成されているかどうかを示します。 このという名前の変数に格納`created`します。
    - これは、通知、 **async**メソッド - ことを意味する実際のネットワーク呼び出しが実行されます。
    - 使用する必要があります、`await`キーワードを取得する、`bool`結果。

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. 使用しているため、`await`キーワード、さあ、署名の変更、`Main`メソッド`async`戻って、`Task`します。

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. 最後に、Blob コンテナーを作成したかどうかを出力します。

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. ファイルを保存します。

最終的なファイルは、作業内容を確認したい場合もこれのようになります。

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a>C# 7.1 を使用して、アプリを構築するには

サポート`async`と`await`で`Main`c# 7.1 にメソッドが追加されました。 既定のバージョンのコンパイラを使用している可能性がありますできません。 そのバージョンのコンパイラを使用して、構成ファイルで設定することでを確認しましょう。

1. 開く、`PhotoSharingApp.csproj`追加`<LangVersion>7.1</LangVersion>`を`PropertyGroup`を指定する、`TargetFramework`します。 完了したら、次のように表示する必要があります。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion> 
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. ファイルを保存します。

## <a name="run-the-app"></a>アプリの実行

1. アプリケーションをビルドし、実行します。 **注:** 正しい作業ディレクトリにしているかどうかを確認します。

    ```bash
    dotnet run
    ```

::: zone-end

ゾーン pivot ="javascript"は、保存されている接続文字列を使用して Azure ストレージ アカウントに接続するコードを追加してみましょう。 Azure クライアント ライブラリが自動的に使用、 **AZURE_STORAGE_CONNECTION_STRING**環境変数、接続文字列を取得します。

## <a name="create-a-blob-client"></a>BLOB クライアントを作成する

1. 開いている**index.js**コード エディターにします。

1. 開始を含めることによって、 **azure storage**モジュール。 モジュールをという名前の変数に格納**ストレージ**します。

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. 次に、右その後、使用して、**ストレージ**オブジェクトを作成する、`BlobService`オブジェクトし、グローバルという名前で保存**blobService**します。 これらは、ストレージ アカウントへのアクセスを表す軽量のオブジェクトに注意してください。

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. 作成するコンテナーを表す定数を追加します。 コンテナーを"photoblobs"名前します。

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>コンテナーの作成

使用して、 `BlobService` Azure storage に blob Api を使用するオブジェクト。 前述のように、ネットワーク呼び出しを行うすべての Api は、アプリの応答性を維持するために非同期にします。 `createContainerIfNotExists`メソッドは、このような 1 つのメソッド。 使用して_promise_応答が含まれるコールバックを処理します。

1. 使用`util.promisify`するのコールバック バージョン`createContainerIfNotExists`promise を返すメソッドにします。
    - オブジェクトにコールバック メソッドがあるので、追加することを確認して、`bind`呼び出しの末尾に、そのコンテキストに接続します。
    - という名前のファイルの上部にある定数を戻り値を割り当てる`createContainerAsync`以下に示すようにします。

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. 「こんにちは, World!」を削除します。 出力`main()`します。

1. 新しい呼び出し`createContainerAsync`約束。
    - 渡す、 **containerName**定数。
    - 適用、`await`呼び出しにキーワード。
    - 呼び出しをラップ、 `try`  /  `catch`を構築し、すべてのエラーを出力します。

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. `createContainerAsync` Promise では、最初の値を返します、基礎となる`createContainerIfNotExists`コンテナーの結果であります。 これには、コンテナーかどうかを作成したかどうかに関する情報が含まれます。 変数に結果をキャプチャし、出力コンテナーが作成されているかどうかに基づいて、`result.created`プロパティ。

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. 最後に、装飾、`main`関数と、`async`キーワード。
        
1. ファイルを保存します。

作業内容を確認したい場合、最終的なファイルは、ようになります。

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function run() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

run();
```

## <a name="run-the-app"></a>アプリの実行

1. アプリケーションをビルドし、実行します。 **注:** 正しい作業ディレクトリにしているかどうかを確認します。

    ```bash
    node index.js
    ```

::: zone-end

Blob コンテナーが作成されたことを報告する必要があります。 を実行する場合に、2 回目にするように指示されますが既に存在します。

コンテナーを確認します。

1. [Azure Portal](https://portal.azure.com/?azure-portal=true) にサインインします。

1. ストレージ アカウントに移動します。 使用することができます、**すべてのリソース**から名前で、ストレージ アカウントまたは検索を検索するセクション、_検索ボックス_ポータル ウィンドウの上部にあります。 

1. 選択、 **Blob**でストレージ アカウントのエントリ、 **Blob service**セクション。

1. 表示する必要があります、 **photoblobs** Blob パネル内のコンテナー。 アプリで再作成することをお試しくださいするエントリの右側にある [...] メニューからコンテナーを削除することができます。

> [!NOTE]
> コンテナーは、非常に高速のポータルから表示されませんが、実際に削除するまで数分かかります。 Azure から削除中である場合は再作成しようとしたときにエラー応答が表示されます。

## <a name="delete-the-app"></a>アプリを削除します。
クラウド シェル環境内のアプリケーションのソース コードを保持したくない場合は、フォルダーとすべての内容を削除する、次のコマンドを使用することができます。

```bash
rm -r PhotoSharingApp/
```
