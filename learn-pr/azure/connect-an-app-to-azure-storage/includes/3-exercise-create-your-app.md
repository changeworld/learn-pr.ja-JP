画像やその他のビットの格納、ユーザーに代わってデータを管理する Azure Storage を使用する写真共有のアプリケーションに取り組んでいることを思い出してください。

::: zone pivot="csharp"

ストレージ Api に注力できるように、このシナリオを容易には、新しい .NET Core コンソール アプリケーションを作成します。 常にネットワーク接続があると想定されますがも。 ただし、常にネットワーク エラーがないユーザー エクスペリエンスに影響を与えるまたはアプリケーション自体の障害が発生して、アプリを強化する必要があります。

## <a name="create-a-net-core-application"></a>.NET Core アプリケーションを作成します。

.NET core は、macOS、Windows、および Linux で実行されている .NET のクロスプラット フォーム対応バージョンです。 ツールをローカルでインストールしたり、実行するウィンドウの右側にある Cloud Shell を使用して、次の手順の下。

1. Cloud Shell にサインインまたはコマンド ライン セッションを開くし、"PhotoSharingApp"という名前の新しい .NET Core コンソール アプリケーションを作成します。 追加することができます、`-o`または`--output`フラグを特定のフォルダーにアプリを作成します。

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. アプリを構築およびが正しく実行されるかどうかを確認を実行します。 「こんにちは, World!」を表示する必要があります。 コンソール。

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

ストレージ Api に注力できるように、このシナリオを容易には、コンソールから実行できる新しい Node.js アプリケーションを作成します。 常にネットワーク接続があると想定されますがも。 ただし、常にネットワーク エラーがない、ユーザー エクスペリエンスに影響を与えるまたはアプリケーション自体の障害が発生して、アプリを強化する必要があります。

## <a name="create-a-nodejs-application"></a>Node.js アプリケーションを作成する

Node.js は、JavaScript アプリを実行するための一般的なフレームワークです。 Web アプリの場合は、最もよく使用されますもコマンドラインからのロジックの実行に使用できます。 ローカルにインストールされているツールを使っている場合は、コマンドラインから、次の手順を実行できます。 実行するウィンドウの右側にある Cloud Shell を使用することができます、次の手順。

1. Cloud Shell にサインインし、またはコマンド ライン セッション"PhotoSharingApp"をという名前の新しいフォルダーを作成します。

    ```bash
    mkdir PhotoSharingApp
    ```

1. 新しいフォルダに変更し、作成、 **package.json**ファイル、新しいアプリを記述するノード パッケージ マネージャー (NPM) にします。
    - "PhotoSharingApp"名前を付けます。
    - その他のすべてのメッセージの既定値を取得できます。

    ```bash
    cd PhotoSharingApp
    npm init
    ```

1. 新しいソース ファイルを作成**index.js**、これは、コードが変わります。

    ```bash
    touch index.js
    ```

1. 開く、 **index.js**ファイル、エディターを使用します。 Cloud Shell を使用する場合は、入力`code .`エディターを開きます。

1. 次のプログラムを配置、 **index.js**ファイル。

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. ファイルを保存&mdash;Cloud Shell のエディターの右上隅に「...」メニューを使用することができます。

1. アプリを正しく実行されるかどうかを確認を実行します。 「こんにちは, World!」を表示する必要があります。 コンソール。

    ```bash
    node index.js
    ```

::: zone-end