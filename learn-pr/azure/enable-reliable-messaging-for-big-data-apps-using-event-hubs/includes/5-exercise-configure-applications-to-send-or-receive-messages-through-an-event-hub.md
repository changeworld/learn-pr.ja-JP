イベント ハブに対してパブリッシャー アプリケーションとコンシューマー アプリケーションを構成する準備ができました。

このユニットでは、ご利用のイベント ハブを経由してメッセージを送受信するようにこれらのアプリケーションを構成します。 これらのアプリケーションは GitHub リポジトリに格納されます。

2 つの異なるアプリケーションを構成します。1 つはメッセージの送信側として機能し (**SimpleSend**)、もう 1 つはメッセージの受信側として機能します (**EventProcessorSample**)。 これらは、ブラウザー内ですべてのことができるようにするための Java アプリケーションです。 ただし、.NET など、いずれのプラットフォームについても同じ構成が必要です。

## <a name="create-a-general-purpose-standard-storage-account"></a>汎用の Standard ストレージ アカウントを作成する

このユニットで構成する Java 受信側アプリケーションでは、メッセージが Azure Blob Storage に格納されます。 Blob Storage にはストレージ アカウントが必要です。

1. `storage account create` コマンドを利用し、ストレージ アカウント (汎用 V2) を作成します。 既定のリソース グループと場所を設定したことを思い出してください。通常、これらのパラメーターは_必須_ですが、オフのままにしてもかまいません。

    |パラメーター      |説明|
    |---------------|-----------|
    |--name (必須)  | ストレージ アカウントの名前。 |
    |--resource-group (必須)  |リソース グループの所有者。 事前に作成した Sandbox リソース グループを使用します。|
    |--location (省略可能)    |特定の場所でストレージ アカウントを必要とする場合の、リソース グループの場所ではない任意の場所。|

    ストレージ アカウント名を変数に設定します。 小文字と数字だけで作成し、区切り記号にはハイフンを使用します。 また、Azure 内で一意にする必要があります。

    ```azurecli
    STORAGE_NAME=[name]
    ```

    次に、このコマンドを使用し、ストレージ アカウントを作成します。

    ```azurecli
    az storage account create --name $STORAGE_NAME --sku Standard_RAGRS --encryption blob
    ```

    > [!TIP]
    > ストレージ アカウントを作成できなかった場合、環境変数を変更してもう一度お試しください。

1. `account keys list` コマンドを使用し、ご利用のストレージ アカウントに関連付けられているアクセス キーをすべて一覧表示します。 アカウント名とリソース グループ (既定) が必要です。

    ```azurecli
    az storage account keys list --account-name $STORAGE_NAME
    ```

     ご利用のストレージ アカウントに関連付けられているアクセス キーが一覧表示されます。 今後使用するために**キー**の値をコピーして保存します。 このキーは、ストレージ アカウントにアクセスする場合に必要となります。

1. 次のコマンドを使用し、ご利用のストレージ アカウントの接続文字列を表示します。

    ```azurecli
    az storage account show-connection-string -n $STORAGE_NAME
    ```

    このコマンドでは、ストレージ アカウントに関する接続の詳細が返されます。 **connectionString** の_値_をコピーして保存します。 次のようなものになります。

    ```output
    "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage_account_name;AccountKey=VZjXuMeuDqjCkT60xX6L5fmtXixYuY2wiPmsrXwYHIhwo736kSAUAj08XBockRZh7CZwYxuYBPe31hi8XfHlWw=="
    ```

1. 次のコマンドを使用してストレージ アカウント内に **messages** と呼ばれるコンテナーを作成します。 前の手順でコピーした **connectionString** を使用します。

    ```azurecli
    az storage container create -n messages --connection-string "<connection string here>"
    ```

## <a name="clone-the-event-hubs-github-repository"></a>Event Hubs GitHub リポジトリを複製する

次の手順を使用し、`git` で Event Hubs GitHub リポジトリを複製します。 Cloud Shell でこの権限を実行できます。

1. このユニットでビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure/azure-event-hubs)にあります。 次のコマンドを使用し、Cloud Shell で現在のディレクトリがホーム ディレクトリになっていることを確認してからこのリポジトリを複製します。

    ```bash
    cd ~
    git clone https://github.com/Azure/azure-event-hubs.git
    ```
    リポジトリがホーム フォルダーに複製されます

## <a name="edit-simplesendjava"></a>SimpleSend.java を編集する

組み込みの Cloud Shell Code エディターを使用します。 これは Monaco エディターに基づいており、Visual Studio Code に似ていますが、完全にオンラインです。

このエディターを使用して SimpleSend アプリケーションを変更し、ご利用の Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名、プライマリ キーを追加します。 エディター ウィンドウの下部に主要なコマンドが表示されます。 

<kbd>Ctrl + O</kbd> キーを使用して編集内容を書き込み、次に <kbd>Enter</kbd> キーを押して出力ファイル名を確定し、<kbd>Ctrl + X</kbd> キーを使用してエディターを終了する必要があります。 あるいは、エディターの右上隅にある "..." メニューからすべての編集コマンドを実行できます。

1. **SimpleSend** フォルダーに移動します。

    ```bash
    cd azure-event-hubs/samples/Java/Basic/SimpleSend/src/main/java/com/microsoft/azure/eventhubs/samples/SimpleSend
    ```

1. 現在のフォルダーでコード エディターを開きます。 左側にファイルの一覧が、右側にエディター スペースが表示されます。

    ```bash
    code .
    ```

1. ファイルの一覧から **SimpleSend.java** ファイルを選択して開きます。

1. エディターで、次の文字列を検索して置換します。

    - `"Your Event Hubs namespace name"` をご利用のイベント ハブ名前空間の名前に置換します。
    - `"Your Event Hub"` をご利用のイベント ハブの名前に置換します。
    - `"Your policy name"` を **RootManageSharedAccessKey** に置換します。
    - `"Your primary SAS key"` を、以前に保存したご利用のイベント ハブ名前空間の **primaryKey** キーの値に置換します。
 
    > [!TIP]
    > ターミナル ウィンドウとは異なり、このエディターではお使いの OS で一般的なコピー/貼り付けキーボード アクセラレータ キーを使用できます。

    キーを思い出せない場合、エディターの下にあるターミナル ウィンドウに切り替え、`echo` コマンドで環境変数の 1 つを一覧表示できます。 例:

    ```bash
    echo $NS_NAME
    ```
    Event Hubs 名前空間を作成すると、**RootManageSharedAccessKey** と呼ばれる 256 ビットの SAS キーが作成されます。このキーには、送信、リッスン、管理の権限を名前空間に付与するためのプライマリキーとセカンダリ キーのペアが関連付けられています。 前のユニットでは、Azure CLI コマンドを使用してキーを表示しました。このキーは、Azure portal 内でご利用の Event Hubs 名前空間の**共有アクセス ポリシー**に関するページを開くことによって検索することもできます。

1. [...] メニューまたはアクセラレータ キー (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Command + S</kbd>) を使用して、**SimpleSend.java** を保存します。

1. "..." メニューまたはアクセラレータ キー <kbd>CTRL + Q</kbd> でエディターを閉じます。

## <a name="use-maven-to-build-simplesendjava"></a>Maven を使用して SimpleSend.java をビルドする

次に、**mvn** コマンドを使用して Java アプリケーションをビルドします。

1. メインの **SimpleSend** フォルダーに戻ります。

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    ```

1. Java SimpleSend アプリケーションをビルドします。 これにより、イベント ハブの接続に指定した詳細設定がアプリケーションで確実に使用されます。

    ```bash
    mvn clean package -DskipTests
    ```

    ビルド プロセスは、完了までに数分かかる場合があります。 次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。

    ![送信側アプリケーションのビルド結果](../media/5-sender-build.png)

## <a name="edit-eventprocessorsamplejava"></a>EventProcessorSample.java を編集する

次に、イベント ハブからデータを取り込むように**受信側** (**サブスクライバー**または**コンシューマー**とも呼ばれる) アプリケーションを構成します。

受信側アプリケーションの場合は、**EventHubReceiver** と **EventProcessorHost** の 2 つのメソッドを使用できます。 EventProcessorHost は EventHubReceiver の上にビルドされますが、そのプログラマティック インターフェイスは EventHubReceiver よりシンプルです。 EventProcessorHost では、同じストレージ アカウントを使用して、EventProcessorHost の複数のインスタンスにメッセージ パーティションを自動的に分散することができます。

このユニットでは、EventProcessorHost メソッドを使用します。 EventProcessorSample アプリケーションを編集し、ご利用の Event Hubs 名前空間、イベント ハブ名、共有アクセス ポリシー名とプライマリ キー、ストレージ アカウント名、接続文字列、コンテナー名を追加します。

1. 次のコマンドを使用し、**EventProcessorSample** フォルダーに移動します。

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample/src/main/java/com/microsoft/azure/eventhubs/samples/eventprocessorsample
    ```

1. コード エディターを開きます。

    ```bash
    code .
    ```
    
1. **EventProcessorSample.java** ファイルを選択します。

1. エディターで次の文字列を検索して、次のように置換します。

    - `----ServiceBusNamespaceName----` をご利用の Event Hubs 名前空間の名前に置換します。
    - `----EventHubName----` をご利用のイベント ハブの名前に置換します。
    - `----SharedAccessSignatureKeyName----` を **RootManageSharedAccessKey** に置換します。
    - `----SharedAccessSignatureKey----` を、以前に保存したご利用の Event Hubs 名前空間の **primaryKey** キーの値に置換します。
    - `----AzureStorageConnectionString----` を以前保存したご利用のストレージ アカウント接続文字列に置換します。
    - `----StorageContainerName----` を**メッセージ**に置換します。
    - `----HostNamePrefix----` をご利用のストレージ アカウントの名前に置換します。

1. [...] メニュー、またはアクセラレータ キー (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Command + S</kbd>) を使用し、**EventProcessorSample.java** を保存します。

1. エディターを閉じます。

## <a name="use-maven-to-build-eventprocessorsamplejava"></a>Maven を使用して EventProcessorSample.java をビルドする

1. 次のコマンドを使用して、メインの **EventProcessorSample** フォルダーに移動します。

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    ```

1. 次のコマンドを使用して Java SimpleSend アプリケーションをビルドします。 これにより、イベント ハブの接続に指定した詳細設定がアプリケーションで確実に使用されます。

    ```bash
    mvn clean package -DskipTests
    ```

    ビルド プロセスは、完了までに数分かかる場合があります。 次に進む前に **[INFO] BUILD SUCCESS** メッセージが表示されていることを確認します。

    ![受信側アプリケーションのビルド結果](../media/5-receiver-build.png)

## <a name="start-the-sender-and-receiver-apps"></a>送信側アプリと受信側アプリを起動する

1. コマンドラインから Java アプリケーションを実行するには、**java** コマンドを使用し、.jar パッケージを指定します。 次のコマンドを使用して SimpleSend アプリケーションを起動します。

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. "**Send Complete...**" と表示されたら、<kbd>Enter</kbd> キーを押します。

    ```output
    jar-with-dependencies.jar
    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2018-09-18T19:42:15.146Z: Send Complete...
    ```

1. 次のコマンドを使用して EventProcessorSample アプリケーションを起動します。

    ```bash
    cd ~/azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. メッセージがコンソールに表示されなくなったら、<kbd>ENTER</kbd> キーを押すか、<kbd>CTRL + C</kbd> キーを押してプログラムを終了します。

    ```output
    ...
    SAMPLE: Partition 0 checkpointing at 1064,19
    SAMPLE (3,1120,20): "Message 80"
    SAMPLE (3,1176,21): "Message 84"
    SAMPLE (3,1232,22): "Message 88"
    SAMPLE (3,1288,23): "Message 92"
    SAMPLE (3,1344,24): "Message 96"
    SAMPLE: Partition 3 checkpointing at 1344,24
    SAMPLE (2,1120,20): "Message 83"
    SAMPLE (2,1176,21): "Message 87"
    SAMPLE (2,1232,22): "Message 91"
    SAMPLE (2,1288,23): "Message 95"
    SAMPLE (2,1344,24): "Message 99"
    SAMPLE: Partition 2 checkpointing at 1344,24
    SAMPLE: Partition 1 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE (0,1120,20): "Message 81"
    SAMPLE (0,1176,21): "Message 85"
    SAMPLE: Partition 0 batch size was 10 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 got event batch
    SAMPLE (0,1232,22): "Message 89"
    SAMPLE (0,1288,23): "Message 93"
    SAMPLE (0,1344,24): "Message 97"
    SAMPLE: Partition 0 checkpointing at 1344,24
    SAMPLE: Partition 3 batch size was 8 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 2 batch size was 9 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    SAMPLE: Partition 0 batch size was 3 for host mystorageacct2018-46d60a17-7060-4b53-b0e0-cca70c970a47
    ```

## <a name="summary"></a>まとめ

これで、ご利用のイベント ハブにメッセージを送信できるように送信側アプリケーションが構成されました。 また、ご利用のイベント ハブからメッセージを受信できるように受信側アプリケーションが構成されました。