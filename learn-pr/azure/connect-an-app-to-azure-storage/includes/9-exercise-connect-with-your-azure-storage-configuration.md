この演習では、構成から Azure Storage アカウントの接続文字列を読み込み、その接続文字列を使用して Azure Storage アカウントに接続します。

## <a name="retrieve-the-connection-string"></a>接続文字列を取得する

1. 前のユニットからのコンソール アプリケーションが確実に Visual Studio に読み込まれているようにします。
1. **Program.cs** ファイルでは、**Microsoft.WindowsAzure.Storage** ライブラリを参照するために、ファイルの上部で *using* ステートメントを追加します。
    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. **Main** メソッドの下部に、次の行を追加して、構成ファイルから Azure Storage アカウントの接続文字列を取得します。
    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>BLOB クライアントを作成する

1. **Main** メソッドの下部に次のコードを挿入して、接続文字列を解析し、BLOB クライアントを作成します。
    ```csharp
    CloudStorageAccount storageAccount;
    if (!CloudStorageAccount.TryParse(connectionString,out storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```
1. 次のコードを挿入し、BLOB クライアントを作成してコンテナーへの参照を取得します。**Main** メソッドの下部に存在しない場合は、必要に応じて作成します。
    ```csharp
    var containerName = "MyBlobContainer";
    var success = blobClient
        .GetContainerReference(containerName)
        .CreateIfNotExistsAsync()
        .Result;
    ```

  ***async** メソッドの **CreateIfNotExistsAsync()** と対応する **Result** プロパティの使用に注意してください。一般的なアプリケーションでは、通常、**await** キーワードを使用します。ただし、このアプリケーションはコンソール アプリケーションであり、非同期ではないため、**Result** プロパティを呼び出して **CreateIfNotExistsAsync()** メソッドによって作成された非同期タスクの結果を取得する必要があります。*

## <a name="print-a-status-message"></a>状態メッセージを出力する

1. **Main** メソッドの下部に次のコードを追加して、成功または失敗のメッセージを出力します。
    ```csharp
    if (!success)
    {
        Console.WriteLine("Error: Could not connect to Azure Storage container");
        return;
    }
    Console.WriteLine("Successfully connected to Azure Storage container");
    ```
1. 最後に、アプリケーションを実行して接続が成功したことを確認し、Azure portal を表示して、以前に存在していなかった場合は、Storage コンテナーが作成されていることを確認します。
1. **省略可能**: コンテナーを使用する計画がない場合は、コンテナーが配置されているリソース グループを削除し、確実に使用していないリソースに対して請求されないようにします。


