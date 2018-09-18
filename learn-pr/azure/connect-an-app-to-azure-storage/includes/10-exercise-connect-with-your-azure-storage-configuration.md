::: zone pivot="csharp" コードを追加して構成から接続文字列を取得し、その接続文字列を使用して Azure ストレージ アカウントに接続してみましょう。

## <a name="retrieve-the-connection-string"></a>接続文字列を取得する

1. **Program.cs** を選択してコード エディターで開きます。

1. ファイルの上部に `using` ステートメントを追加して、`Microsoft.WindowsAzure.Storage` 名前空間を参照します。

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. `Main` メソッドの末尾に、次の行を追加して、構成ファイルから Azure ストレージ アカウントの接続文字列を取得します。 渡された_キー_は、**appsettings.json** ファイルで使用される名前と一致する必要があります。

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>BLOB クライアントを作成する

1. 静的な `CloudStorageAccount.TryParse` メソッドを使用して `CloudStorageAccount` オブジェクトを作成します。作成されたオブジェクトを返すには、接続文字列と `out` パラメーターを使用します。 文字列が正常に分析されたかどうかを示す `bool` 値が返されます。
    - 正常に分析されなかった場合は、メッセージをコンソールに出力して、メソッドから返します。

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. 返された `CloudStorageAccount` オブジェクトを使用して、BLOB クライアントを作成します。

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. 次に、BLOB クライアントを使用して、"photoblobs" という名前のコンテナーへの参照を取得します。 アカウント名と同じように、BLOB コンテナー名は小文字にして、文字と数字で構成する必要があります。

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. `CreateIfNotExistsAsync` メソッドを使用してコンテナーを作成します。 これにより、コンテナーが作成されたかどうかを示す `bool` が返されます。 これを `created` という名前の変数に格納します。
    - これは **async** メソッドで、つまり、これにより実際のネットワーク呼び出しが行われることに注目してください。
    - `await` キーワードを使用して、`bool` の結果を取得する必要があります。

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. `await` キーワードを使用するので、先に進み、`Main` メソッドの署名を `async` になるように変更して `Task` を返します。

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. 最後に、BLOB コンテナーを作成したかどうかを出力します。

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. ファイルを保存します。

作業内容を確認する場合の最終的なファイルは次のようになります。

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

## <a name="use-c-71-to-build-our-app"></a>C# 7.1 を使用してアプリをビルドする

C# 7.1 に、`Main` メソッドでの `async` と `await` のサポートが追加されました。 これが、使用しているコンパイラの既定のバージョンではない可能性があります。 構成ファイルで設定して、そのバージョンのコンパイラを使用していることを確認しましょう。

1. `PhotoSharingApp.csproj` を開き、`TargetFramework` を指定する `PropertyGroup` に `<LangVersion>7.1</LangVersion>` を追加します。 完了すると、次のようになります。

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

## <a name="run-the-app"></a>アプリを実行する

1. アプリケーションをビルドして実行します。 **注:** 正しい作業ディレクトリで操作していることを確認してください。

    ```bash
    dotnet run
    ```

::: zone-end

::: zone-pivot="javascript" コードを追加し、格納された接続文字列を使用して Azure ストレージ アカウントに接続してみましょう。 Azure クライアント ライブラリでは、自動的に **AZURE_STORAGE_CONNECTION_STRING** 環境変数を使用して接続文字列が取得されます。

## <a name="create-a-blob-client"></a>BLOB クライアントを作成する

1. コード エディターで **index.js** を開きます。

1. まず、**azure-storage** モジュールを含めます。 モジュールを **storage** という名前の変数に格納します。

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. 次に、その直後に、**storage** オブジェクトを使用して `BlobService` オブジェクトを作成し、グローバルな名前の **blobService** に格納します。 これらは、ストレージ アカウントへのアクセスを表す軽量なオブジェクトであることに注意してください。

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. 作成するコンテナーを表す定数を追加します。 コンテナーに "photoblobs" という名前を付けます。

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>コンテナーを作成する

`BlobService` オブジェクトを使用して、Azure Storage で BLOB API を操作することができます。 前述のとおり、ネットワーク呼び出しを行うすべての API は、アプリの応答性を維持するために非同期になっています。 `createContainerIfNotExists` メソッドは、そのようなメソッドの 1 つです。 _promise_ を使用して、応答を含むコールバックを処理します。

1. `util.promisify` を使用してコールバック バージョンの `createContainerIfNotExists` を取得し、promise-returning メソッドに変換します。
    - コールバック メソッドはオブジェクトにあるため、必ず、末尾に `bind` 呼び出しを追加して、そのコンテキストに接続するようにしてください。
    - 以下のように、`createContainerAsync` という名前のファイルの先頭にある定数に戻り値を割り当てます。

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. "Hello, World!" という出力を  `main()` から削除します。

1. 新しい `createContainerAsync` promise を呼び出します。
    - それに **containerName** 定数を渡します。
    - 呼び出しに `await` キーワードを適用します。
    - `try` / `catch` コンストラクトに呼び出しをラップして、すべてのエラーを出力します。

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. `createContainerAsync` promise によって、基になる `createContainerIfNotExists` から最初の値が返されます。これはコンテナーの結果です。 これには、コンテナーが作成されたかどうかに関する情報が含まれます。 結果を変数にキャプチャし、`result.created` プロパティに基づいてコンテナーが作成されたかどうかを出力します。

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

1. 最後に、`main` 関数を `async` キーワードで修飾します。
        
1. ファイルを保存します。

作業内容を確認する場合の最終的なファイルは次のようになります。

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

## <a name="run-the-app"></a>アプリを実行する

1. アプリケーションをビルドして実行します。 **注:** 正しい作業ディレクトリで操作していることを確認してください。

    ```bash
    node index.js
    ```

::: zone-end

BLOB コンテナーが作成されたことが報告されます。 2 回目の実行時には、既に存在することが示されます。

コンテナーを確認するには:

1. [Azure Portal](https://portal.azure.com/?azure-portal=true) にサインインします。

1. ストレージ アカウントに移動します。 **[すべてのリソース]** セクションを使用して、ストレージ アカウントを検索することができます。また、ポータル ウィンドウの上部にある_検索ボックス_から名前で検索することもできます。 

1. **[Blob service]** セクションで、ストレージ アカウントの **[BLOB]** エントリを選択します。

1. [BLOB] パネルに **photoblobs** コンテナーが表示されます。 エントリの右側にある [...] メニューを使用してコンテナーを削除し、アプリで再作成を試すことができます。

> [!NOTE]
> ポータルではコンテナーはすぐに表示されなくなりますが、実際に削除されるまでは数分かかります。 再作成を試す場合、削除されている間に、Azure からエラー応答を受け取ります。

## <a name="delete-the-app"></a>アプリを削除する
ご利用の Cloud Shell 環境でアプリケーションのソース コードを保持しないようにした場合は、次のコマンドを使用して、フォルダーとすべての内容を削除することができます。

```bash
rm -r PhotoSharingApp/
```
