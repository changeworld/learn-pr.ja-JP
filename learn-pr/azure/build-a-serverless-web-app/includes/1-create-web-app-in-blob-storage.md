このモジュールでは、HTML ベースのユーザー インターフェイスを表示するシンプルな Web アプリケーションをデプロイします。 サーバーレスのバック エンドには、アプリケーション イメージをアップロードし、わかりやすいキャプションを自動的に生成ができます。

![Web アプリの実行](../media/0-app-screenshot-finished.png)

次の図では、アプリケーションによって使用されている Azure サービスを示します。

![ソリューションのアーキテクチャ図](../media/0-architecture.jpg)

1. Azure Blob Storage は静的 Web コンテンツ (HTML、CSS、JS) を提供し、画像が格納されます。
2. Azure Functions によって、画像のアップロード、サイズ変更、およびメタデータ ストレージが管理されます。
3. Azure Cosmos DB には、画像のメタデータが格納されます。
4. Azure Logic Apps は、Cognitive Services の Computer Vision API からのイメージのキャプションを取得します。
5. Azure Active Directory によりユーザー認証が管理されます。

Azure Blob Storage は、静的ファイルをホストする際に使用できる、低コストで非常にスケーラブルなサービスです。 このモジュールでは web アプリをビルドする Blob storage (たとえば、HTML、JavaScript、または CSS) の静的コンテンツを処理するために使用します。

## <a name="create-an-azure-storage-account"></a>Azure Storage アカウントの作成

[!include[](../../../includes/azure-sandbox-activate.md)]

Azure Storage アカウントは、テーブル、キュー、ファイル、BLOB (オブジェクト)、仮想マシン ディスクを格納できる Azure リソースです。

1. このチュートリアルの静的コンテンツ (HTML、CSS、および JavaScript ファイル) は Blob Storage でホストされます。 Blob Storage にはストレージ アカウントが必要です。 汎用 v2 の作成、リソース グループ内 (GPv2) Storage アカウント。 `<storage account name>` を一意の名前に置き換えます。

    ```azurecli
    az storage account create -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 --https-only true --sku Standard_LRS
    ```
    
1. [Azure portal](https://portal.azure.com/?azure-portal=true) の上部にある検索バーを使用して、先ほど作成したストレージ アカウントを検索します。 アカウントを開きます。

1. 左側のナビゲーションで、**[静的な Web サイト (プレビュー)]** を選択して、静的な Web サイトをホストするコンテナーを構成します。
    - **[有効]** を選択して、静的な Web サイトを有効にします。
    - インデックス ドキュメント名として「**index.html**」と入力します。 ボックスには既にグレーのフォントで *index.html* と表示されていますが、これはテキストの例でしかありません。 やはり、ボックスに「**index.html**」と入力する必要があります。
    - **[保存]** をクリックします。
    
    ![静的な Web サイトの設定を入力する](../media/1-storage-static-website.png)

1. このチュートリアルの作業中に簡単にコピーできる場所に**プライマリ エンドポイント**を保存します。 このエンドポイントは Web アプリケーションの URL です。

## <a name="upload-the-web-application"></a>Web アプリケーションをアップロードする

1. このチュートリアルでビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure-Samples/functions-first-serverless-web-application)にあります。 Cloud Shell で、ホーム ディレクトリに移動し、このリポジトリを複製します。

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    リポジトリは `/home/<username>/functions-first-serverless-web-application` に複製されます。

1. クライアント側の Web アプリケーションは **www** フォルダーにあり、Vue.js JavaScript フレームワークを使用して構築されています。 開く、 **www**フォルダーと実行**npm**コマンドをアプリケーションの依存関係をインストールしてアプリケーションをビルドします。 これらのコマンドの最後は、完了に数分かかることがあります。

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    アプリケーションが **dist** フォルダーに生成されます。

1. 現在のディレクトリを **dist** フォルダーに変更し、アプリケーションを **$web** BLOB コンテナーにアップロードします。

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. アプリケーションを表示するには、web ブラウザーで静的な web サイトのプライマリ エンドポイントの URL を開きます。

    ![最初のサーバーレス Web アプリのホーム ページ](../media/1-app-screenshot-new.png)


## <a name="summary"></a>まとめ

このユニットでは、ストレージ アカウントを作成します。 という名前の blob コンテナー **$web**ストレージ アカウントを web アプリケーションの静的コンテンツを格納され、コンテンツをパブリックに使用できるようにします。 次に、サーバーレス関数を使用して、この web アプリケーションから Blob ストレージにイメージをアップロードする方法を学習します。