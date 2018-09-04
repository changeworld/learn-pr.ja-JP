これで新しいイベント ハブを作成する準備が整いました。 イベント ハブを作成したら、Azure Portal を使用して新しいハブを確認します。

Azure CLI を使用してイベント ハブを作成します。 この演習では、Azure CLI 2.0 を使用します。 

## <a name="create-an-event-hubs-namespace"></a>Event Hubs 名前空間の作成

次の手順に従い、Azure Cloud Shell によってサポートされている Bash シェルを使用して、Event Hubs 名前空間を作成します。

1. Cloud Shell (Bash) にサインインします。  

1. 次のコマンドを使用して、Azure リソース グループを作成します。

    ```azurecli
        az group create --name <resource group name> --location <location>
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--name (必須)      |新しいリソース グループの名前を入力します。|
    |--location (必須)     |最も近い Azure データセンターの場所を入力します (例: westus)。|

1. 次のコマンドを使用して Event Hubs 名前空間を作成します。

    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--name (必須)      |Event Hubs 名前空間の一意の名前を、6 文字から 50 文字の長さで入力します。 名前に含めることができるのは、文字、数字、ハイフンのみです。 これは文字で始まり、文字または数字で終わる必要があります。|
    |--resource-group (必須)  |手順 1 で作成したリソース グループを入力します。
    |--l (省略可能)     |最も近い Azure データセンターの場所を入力します (例: westus)。|

1. 次のコマンドを使用して、Event Hubs 名前空間の接続文字列を取得します。 これは、イベント ハブを使用してメッセージを送受信するようにアプリケーションを構成するために必要となります。

    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--resource-group (必須)  |手順 1 で作成したリソース グループを入力します。|
    |--namespace-name (必須)      |手順 2 で作成した名前空間を入力します。|

    このコマンドによって Event Hubs 名前空間の接続文字列が返されます。これは、パブリッシャーおよびコンシューマー アプリケーションを構成するために、後ほど使用します。 後で使用するために次のキーの値を保存します。

    - **primaryConnectionString**
    - **primaryKey**

## <a name="create-an-event-hub"></a>イベント ハブの作成

次の手順に従って新しいイベント ハブを作成します。

1. 次のコマンドを使用して新しいイベント ハブを作成します。

    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--name (必須)  |イベント ハブの名前を入力します。|
    |--resource-group (必須)  |前の手順で作成したリソース グループを入力します。|
    |--namespace-name (必須)      |前の手順で作成した名前空間を入力します。|

1. 次のコードを使用して、イベント ハブの詳細を確認します。 

    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```

    |パラメーター      |説明|
    |---------------|-----------|
    |--resource-group (必須)  |前の手順で作成したリソース グループを入力します。|
    |--namespace-name (必須)      |前の手順で作成した名前空間を入力します。|
    |--name  (必須)|手順 1 で作成したイベント ハブの名前を入力します。|

## <a name="view-the-event-hub-in-the-azure-portal"></a>Azure Portal でのイベント ハブの確認

次の手順を使用して、Azure Portal でイベント ハブを確認します。

1. [Azure Portal](https://portal.azure.com?azure-portal=true) の上部にある検索バーを使用して、Event Hubs 名前空間を検索します。

1. 名前空間をクリックして開きます。

1. **[Event Hubs 名前空間]** > **[エンティティ]** から **[イベント ハブ]** をクリックします。
    イベント ハブが **[アクティブ]** の状態で表示され、また **[メッセージのリテンション期間]** の規定値 (*7*) と、**[パーティション数]** の規定値 (*4*) が表示されます。

    ![Azure Portal に表示されるイベント ハブ](../media-draft/3-event-hub.png)

## <a name="summary"></a>まとめ

これで新しいイベント ハブが作成されました。また、パブリッシャーおよびコンシューマー アプリケーションを構成するために必要なすべての情報の準備が完了しました。
