イベント ハブに対してパブリッシャー アプリケーションとコンシューマー アプリケーションを構成する準備ができました。

この演習では、ご利用のイベント ハブを経由してメッセージを送受信するようにこれらのアプリケーションを構成します。 これらのアプリケーションは GitHub リポジトリに格納されます。

2 つの異なるアプリケーションを構成します。1 つはメッセージの送信側として機能し (**SimpleSend**)、もう 1 つはメッセージの受信側として機能します (**EventProcessorSample**)。 これらは、ブラウザー内ですべてのことができるようにするための Java アプリケーションです。 ただし、.NET など、いずれのプラットフォームについても同じ構成が必要です。

## <a name="create-a-general-purpose-standard-storage-account"></a>汎用の Standard ストレージ アカウントを作成する

このユニットで構成する Java 受信側アプリケーションでは、メッセージが Azure Blob Storage に格納されます。 Blob Storage にはストレージ アカウントが必要です。

1. 次のコマンドを使用して、リソース グループにストレージ アカウント (汎用 V2) を作成します。

    ```azurecli
    az storage account create --name <storage account name> --resource-group <rgn>[Sandbox resource group name]</rgn>  --location <location> --sku Standard_RAGRS --encryption blob
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--name (必須)  |ストレージ アカウントの名前を入力します。|
    |--resource-group (必須)  |前のユニットで作成したリソース グループを入力します。|
    |--location (省略可能)    |前のユニットでリソース グループを作成するために使用した場所を入力します。|

1. 次のコマンドを使用して、ご利用のストレージ アカウントに関連付けられているアクセス キーをすべて一覧表示します。

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <rgn>[Sandbox resource group name]</rgn>
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--account-name (必須)  |ストレージ アカウントの名前を入力します。|
    |--resource-group (必須)  |前のユニットで作成したリソース グループを入力します。|

     ご利用のストレージ アカウントに関連付けられているアクセス キーが一覧表示されます。 今後使用するために**キー**の値をコピーして保存します。 このキーは、ストレージ アカウントにアクセスする場合に必要となります。

1. 次のコマンドを使用して、ご利用のストレージ アカウントの接続文字列を表示します。

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn>
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |-n (必須)  |ストレージ アカウントの名前を入力します。|
    |-g (必須)  |ご利用のリソース グループの名前を入力します。|

    このコマンドでは、ストレージ アカウントについて接続の詳細が返されます。 **connectionString** の値をコピーして保存します。

1. 次のコマンドを使用してストレージ アカウント内に **messages** と呼ばれるコンテナーを作成します。 前の手順でコピーした **connectionString** を使用します。

    ```azurecli
    az storage container create -n messages --connection-string "<connection string>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a>Event Hubs GitHub リポジトリを複製する

次の手順を使用して Event Hubs GitHub リポジトリを複製します。

1. この演習でビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure/azure-event-hubs)にあります。 次のコマンドを使用して、Cloud Shell でご利用のホーム ディレクトリにいることを確認してから、このリポジトリを複製します。

    ```azurecli
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    リポジトリは `/home/<username>/azure-event-hubs` に複製されます。

## <a name="edit-simplesendjava"></a>SimpleSend.java を編集します。

Cloud Shell のエディターを使用して、SimpleSend アプリケーションを編集し、Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名、および主キーを追加します。 エディター ウィンドウの下部に主要なコマンドが表示されます。このユニットでは、Ctrl + O キーを使用して編集内容を書き込み、次に Enter キーを押して出力ファイル名を確認し、さらに Ctrl + X キーを使用してエディターを終了する必要があります。

1. 次のコマンドを使用して **SimpleSend** フォルダーに移動します。

    ```azurecli
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. 開く、 **SimpleSend.java**次のコマンドを使用して Cloud Shell エディターでファイル。

    ```azurecli
    code SimpleSend.java
    ```

1. エディターで、検索し、次の文字列を置き換えます。

    - `"Your Event Hubs namespace name"` をご利用のイベント ハブ名前空間の名前に置換します。
    - `"Your event hub"` をご利用のイベント ハブの名前に置換します。
    - `"Your primary SAS key"` を、以前に保存したご利用のイベント ハブ名前空間の **primaryKey** キーの値に置換します。
    - `"Your policy name"` を **RootManageSharedAccessKey** に置換します。
 
        Event Hubs 名前空間を作成すると、**RootManageSharedAccessKey** と呼ばれる 256 ビットの SAS キーが作成されます。このキーには、送信、リッスン、および管理の権限を名前空間に付与するためのプライマリキーとセカンダリ キーのペアが関連付けられています。 前のユニットでは、Azure CLI コマンドを使用してこのキーを表示しました。このキーは、Azure portal 内でご利用の Event Hubs 名前空間の**共有アクセス ポリシー**に関するページを開くことによって検索することもできます。

1. 保存**SimpleSend.java** 「...」メニューまたはショートカット キー (Ctrl + S Windows と Linux、macos には、Cmd SS) を使用するか。

1. クリックして、コード エディターを閉じることができます、`{}`クラウド シェル エディターの上部に中かっこツールバー ボタンをクリックします。

## <a name="use-maven-to-build-simplesendjava"></a>Maven を使用して SimpleSend.java をビルドする

今度は、**mvn** コマンドを使用して Java アプリケーションをビルドします。

1. 次のコマンドを使用して、メインの **SimpleSend** フォルダーに移動します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. 次のコマンドを使用して Java SimpleSend アプリケーションをビルドします。 これにより、そのアプリケーションではご利用のイベント ハブに対する接続の詳細が確実に使用されるようになります。

    ```azurecli
    mvn clean package -DskipTests
    ```

    ビルド プロセスは、完了までに数分かかる場合があります。 次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。

    ![送信側アプリケーションのビルド結果](../media-draft/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a>EventProcessorSample.java を編集します。

次にイベント ハブからデータを取り込むことができるように**受信側** (**サブスクライバー**または**コンシューマー**とも呼ばれる) アプリケーションを構成します。

受信側アプリケーションの場合は、**EventHubReceiver** および **EventProcessorHost** の 2 つのメソッドを使用できます。 EventProcessorHost は EventHubReceiver の上にビルドされますが、そのプログラマティック インターフェイスは EventHubReceiver よりシンプルです。 EventProcessorHost では、同じストレージ アカウントを使用して、EventProcessorHost の複数のインスタンスにメッセージ パーティションを自動的に分散することができます。

このユニットでは、EventProcessorHost メソッドを使用します。 Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名と主キー、ストレージ アカウント名、接続文字列、およびコンテナー名を追加する EventProcessorSample アプリケーションを編集します。

1. 次のコマンドを使用して、**EventProcessorSample** フォルダーに移動します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. 開く、 **EventProcessorSample.java**次のコマンドを使用して Cloud Shell エディターでファイル。

    ```azurecli
    code EventProcessorSample.java
    ```
1. 探し、エディターで、次の文字列に置き換えます。

    - `----ServiceBusNamespaceName----` をご利用の Event Hubs名前空間の名前に置換します。
    - `----EventHubName----` をご利用のイベント ハブの名前に置換します。
    - `----SharedAccessSignatureKeyName----` を **RootManageSharedAccessKey** に置換します。
    - `----SharedAccessSignatureKey----` を、以前に保存したご利用の Event Hubs 名前空間の **primaryKey** キーの値に置換します。
    - `----AzureStorageConnectionString----` を以前保存したご利用のストレージ アカウント接続文字列に置換します。
    - `----StorageContainerName----` を**メッセージ**に置換します。
    - `----HostNamePrefix----` をご利用のストレージ アカウントの名前に置換します。

1. 保存**EventProcessorSample.java** 「...」メニューまたはショートカット キー (Ctrl + S Windows と Linux、macos には、Cmd SS) を使用するか。

1. クリックして、コード エディターを閉じることができます、`{}`クラウド シェル エディターの上部に中かっこツールバー ボタンをクリックします。

## <a name="use-maven-to-build-eventprocessorsamplejava"></a>Maven を使用して EventProcessorSample.java をビルドする

1. 次のコマンドを使用して、メインの **EventProcessorSample** フォルダーに移動します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. 次のコマンドを使用して Java SimpleSend アプリケーションをビルドします。 これにより、そのアプリケーションではご利用のイベント ハブに対する接続の詳細が確実に使用されるようになります。

    ```azurecli
    mvn clean package -DskipTests
    ```

    ビルド プロセスは、完了までに数分かかる場合があります。 次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。

    ![受信側アプリケーションのビルド結果](../media-draft/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a>送信側アプリと受信側アプリを起動する

1. コマンドラインから Java アプリケーションを実行するには、**java** コマンドを使用し、.jar パッケージを指定します。 次のコマンドを使用して、SimpleSend アプリケーションを起動します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. **Send Complete...** と表示されたら、Enter キーを押します。

    ![送信側アプリケーションの実行結果](../media-draft/5-sender-run.png)

1. 次のコマンドを使用して、EventProcessorSample アプリケーションを起動します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. メッセージがコンソールに表示されなくなったら、Enter キーを押します。

    ![受信側アプリケーションの実行結果](../media-draft/5-receiver-run.png)

## <a name="summary"></a>まとめ

これで、ご利用のイベント ハブにメッセージを送信できるように送信側アプリケーションが構成されました。 また、ご利用のイベント ハブからメッセージを受信できるように受信側アプリケーションも構成されました。
