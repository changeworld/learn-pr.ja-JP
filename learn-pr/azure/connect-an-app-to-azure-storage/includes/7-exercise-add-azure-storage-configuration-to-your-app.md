このユニットでは、アクセス キーを取得し、それをアプリ構成に追加します。 また、.NET Core コンソールを作成したので、構成ファイルからアプリケーションに読み込むためのサポートを追加する必要があります。

## <a name="create-a-json-configuration-file"></a>JSON 構成ファイルを作成する

1. 前の演習で作成した Visual Studio プロジェクトで、プロジェクトを右クリックし、**[追加]** を選択して、**[新しい項目]** を選択します (あるいは CTRL-SHIFT-A を押します)。

1. モーダル ダイアログが表示されます。 **[JavaScript JSON 構成ファイル]** を選択し、*[名前]* テキストボックスに「**appsettings.json**」と入力し、**[追加]** をクリックします。

  ![アプリケーション設定](..\media-draft\7-appsettings.png)

1. 次のようなコンテンツのプロジェクトにファイルが追加されます。

    ```json
    {
      "exclude": [
        "**/bin",
        "**/bower_components",
        "**/jspm_packages",
        "**/node_modules",
        "**/obj",
        "**/platforms"
      ]
    }
    ```
1. **除外**エントリとその関連コンテンツを削除し、**StorageAccountConnectionString** という 1 つのエントリを代わりに指定します。文字列値は空にします。 次のようになります。

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. **appsettings.json** ファイルを選択して右クリックし、**[プロパティ]** を選択します (あるいは、ALT + ENTER または F4 を押します)。

1. **[出力ディレクトリにコピー]** を **[新しい場合はコピーする]** に変更します。

  ![ビルド アクション](..\media-draft\7-build-action.png)

  > これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。

## <a name="add-support-to-read-a-json-configuration-file"></a>JSON 構成ファイルを読み込むためのサポートを追加する

.NET Core コンソール アプリケーションでは、JSON 構成ファイルからの読み込みを簡単にするために特定のライブラリを追加する必要があります。

1. プロジェクトを右クリックし、**[NuGet パッケージの管理…]** を選択します。

![Manage NuGet packages](..\media-draft\5-manage-nuget-packages.png)

1. 現在インストールされている NuGet パッケージを示すページが表示されます。 **[参照]** オプションをクリックして、検索フィールドに「**Microsoft.Extensions.Configuration.Json**」と入力します。 **Microsoft.Extensions.Configuration.Json** パッケージが検索結果に表示されます。

1. **Microsoft.Extensions.Configuration.Json** パッケージを選択し、右のウィンドウで **[インストール]** を選択します。

1. **[変更のプレビュー]** ダイアログが表示されます。 **[OK]** をクリックします。

1. ライセンス同意のダイアログが表示されます。 **[同意する]** をクリックします。


## <a name="read-from-the-configuration-file"></a>構成ファイルから読み込む

読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。

1. プロジェクトで **Program.cs** ファイルを検索し、それを Visual Studio で開きます。

1. ファイルの一番上に **using System;** 行があります。 その行の下に次のコード行を追加します。

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. **Main** メソッドの内容を次のコードに置き換えます。

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

1. **Program.cs** ファイルは次のようになります。

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;

    namespace DemoConsoleApp
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

> このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。

## <a name="add-your-connection-string-to-the-configuration-file"></a>構成ファイルに接続文字列を追加する

次に、ストレージ アカウント接続文字列を取得し、それをアプリの構成に配置します。

1. Azure portal に移動し、ストレージ アカウントを含むご利用のサブスクリプションを探します。

1. ストレージ アカウントに移動します。

1. ポータルでストレージ アカウントの [アカウント キー] ブレードに移動します。

1. **key1** 接続文字列をコピーします。

1. **StorageAccountConnectionString** の値としてポータルからコピーしたアクセス キーの内容を貼り付けます。 これで構成が次のようになるはずです。

    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```
