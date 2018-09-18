保管した写真やその他のデータをユーザーの代わりに Azure Storage で管理する写真共有アプリケーションについて学習していることを思い出してください。

::: zone pivot="csharp"

Storage API に集中できるようにシナリオを簡単にする目的で、新しい .NET Core Console アプリケーションを作成します。 また、ネットワークに常時接続できるものと想定します。 ただし、ネットワークの故障が利用者に影響を与えないように、アプリケーション自体の失敗につながらないように、常にアプリを強固にしてください。

## <a name="create-a-net-core-application"></a>.NET Core アプリケーションの作成

.NET Core は .NET のクロスプラットフォーム版であり、macOS、Windows、Linux で動作します。 ツールをローカルでインストールしたり、ウィンドウの右側にある Cloud Shell を使用して下の手順を実行したりできます。 

1. Cloud Shell にサインインするか、コマンド ライン セッションを開き、"PhotoSharingApp" という名前で新しい .NET Core Console アプリケーションを作成します。 `-o` または `--output` フラグを追加し、特定のフォルダーでアプリを作成できます。

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. アプリを実行し、正しくビルドされ、実行されることを確認します。 "Hello, World!" と コンソールに表示されるはずです。

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

Storage API に集中できるようにシナリオを簡単にする目的で、コンソールから実行できる新しい Node.js アプリケーションを作成します。 また、ネットワークに常時接続できるものと想定します。 ただし、ネットワークの故障が利用者に影響を与えないように、アプリケーション自体の失敗につながらないように、常にアプリを強固にしてください。

## <a name="create-a-nodejs-application"></a>Node.js アプリケーションを作成する

Node.js は JavaScript アプリの実行に利用される人気のフレームワークです。 一般的には Web アプリで使用されることが多いですが、コマンド ラインからロジックを実行するために使用することもできます。 ツールをローカルでインストールした場合、コマンド ラインから次の手順を実行できます。 あるいは、ウィンドウの右側にある Cloud Shell を使用して下の手順を実行できます。

1. Cloud Shell にサインインするか、コマンド ライン セッションを開き、"PhotoSharingApp" という名前で新しいフォルダーを作成します。

    ```bash
    mkdir PhotoSharingApp
    ```

1. 新しいフォルダーに移動し、Node Package Manager (NPM) を利用し、新しいアプリを説明する **package.json** ファイルを作成します。
    - それに "PhotoSharingApp" という名前を付けます。
    - その他のプロンプトには既定値を選択してかまいません。

    ```bash
    cd PhotoSharingApp
    npm init
    ```

1. 新しいソース ファイルとして **index.js** を作成します。コードはこのファイルに置かれます。

    ```bash
    touch index.js
    ```

1. エディターで **index.js** ファイルを開きます。 Cloud Shell を使用する場合、`code .` と入力してエディターを開くことができます。

1. **index.js** ファイルに次のプログラムを置きます。

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. ファイルを保存します。Cloud Shell エディターの右上隅にある "..." メニューを使用できます。

1. アプリを実行し、正しく実行されることを確認します。 "Hello, World!" と コンソールに表示されるはずです。

    ```bash
    node index.js
    ```

::: zone-end