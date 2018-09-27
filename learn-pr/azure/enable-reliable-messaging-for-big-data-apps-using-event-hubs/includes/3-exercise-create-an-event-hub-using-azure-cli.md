これで新しいイベント ハブを作成する準備が整いました。 イベント ハブを作成したら、Azure portal を使用してご自分の新しいハブを確認します。

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-some-defaults-in-the-azure-cli"></a>Azure CLI でいくつかの既定値を設定する

Cloud Shell で Azure CLI 用の既定値をいくつか指定することから始めましょう。 これで、毎回、それらの値を入力しなくてもよくなります。 具体的に、_リソース グループ_と_場所_を設定してみましょう。 次の一覧から場所を選択します。

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

さらに Azure CLI に次のコマンドを入力し、場所を必ず、自分に近い場所に置き換えます。

```azurecli
az configure --defaults group=<rgn>[sandbox Resource Group]</rgn> location=westus2
```

## <a name="create-an-event-hubs-namespace"></a>Event Hubs 名前空間を作成する

次の手順に従い、Azure Cloud Shell によってサポートされている bash シェルを使用して、Event Hubs 名前空間を作成します。

1. `az eventhubs namespace create` コマンドを使用して Event Hubs 名前空間を作成します。 次のパラメーターを使用します。

    > [!div class="mx-tableFixed"]
    > |パラメーター      |説明|
    > |---------------|-----------|
    > |--name (必須)      |Event Hubs 名前空間の一意の名前を、6 文字から 50 文字の長さで入力します。 名前に含めることができるのは、文字、数字、ハイフンのみです。 これは文字で始まり、文字または数字で終わる必要があります。|
    > |--resource-group (必須) | これは、既定値から指定される、事前に作成された Azure サンド ボックス リソース グループとなります。 |
    > |--l (省略可能)     |最も近くにある Azure データセンターの場所を入力します。ご利用の既定値が使用されます。|
    > |--sku (省略可能) | 名前空間 [Basic  | Standard] の価格レベルです。既定値は _Standard_ です。 これにより、接続とコンシューマーしきい値が決定されます。 |

    再利用できるように環境変数に名前を設定します。

    ```azurecli
    NS_NAME=myEvt-HubNs1
    ````

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```azurecli
    az eventhubs namespace create --name $NS_NAME
    ```

    > [!NOTE]
    > Azure では名前については非常に厳しくチェックされ、名前が既に存在しているか、または無効である場合、CLI から **[無効な要求]** が返されます。 別の名前を試してみるには、ご利用の環境変数を変更して、コマンドを再発行します。


1. 次のコマンドを使用して、ご利用の Event Hubs 名前空間の接続文字列を取得します。 これは、ご利用のイベント ハブを使用してメッセージを送受信するようにアプリケーションを構成するために必要となります。

    ```azurecli
    az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name $NS_NAME
    ```

    > [!div class="mx-tableFixed"]
    > |パラメーター      |説明|
    > |---------------|-----------|
    > |--resource-group (必須)  | これは、既定値から指定される、事前に作成された Azure サンド ボックス リソース グループとなります。 |
    > |--namespace-name (必須)  | 作成した名前空間の名前を入力します。 |

    このコマンドでは、ご利用の Event Hubs 名前空間の接続文字列を含む JSON ブロックが返されます。これは、パブリッシャーおよびコンシューマー アプリケーションを構成するために、後ほど使用します。 後で使用するために次のキーの値を保存します。

    - **primaryConnectionString**
    - **primaryKey**

## <a name="create-an-event-hub"></a>イベント ハブを作成する

次の手順に従って新しいイベント ハブを作成します。

1. `eventhub create` コマンドを使用して新しいイベント ハブを作成します。 必要なパラメーターは次のとおりです。

    > [!div class="mx-tableFixed"]
    > |パラメーター      |説明|
    > |---------------|-----------|
    > |--name (必須)  |イベント ハブの名前を入力します。|
    > |--resource-group (必須)  |リソース グループの所有者です。|
    > |--namespace-name (必須)      |作成した名前空間を入力します。|

    まず、環境変数にイベント ハブの名前を定義してみましょう。

    ```azurecli
    HUB_NAME=[name]
    ```

    ```azurecli
    az eventhubs eventhub create --name $HUB_NAME --namespace-name $NS_NAME
    ```

1. `eventhub show` を使用して、イベント ハブの詳細を確認します。 使用されるのは次のとおりです。

    > [!div class="mx-tableFixed"]
    > |パラメーター      |説明|
    > |---------------|-----------|
    > |--resource-group (必須)  |リソース グループの所有者です。|
    > |--namespace-name (必須)      |作成した名前空間を入力します。|
    > |--name (必須)|イベント ハブの名前です。|

    ```azurecli
    az eventhubs eventhub show --namespace-name $NS_NAME --name $HUB_NAME
    ```

## <a name="view-the-event-hub-in-the-azure-portal"></a>Azure portal でイベント ハブを確認する

次に、Azure portal でこれがどのように表示されるか見てみましょう。

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

1. ポータルの上部にある検索バーを使用して、Event Hubs 名前空間を検索します。

1. 名前空間を選択して開きます。

1. **[エンティティ]** セクションで **[Event Hubs 名前空間]** を選択します。

1. **[Event Hubs]** をクリックします。

    ご自分のイベント ハブが **[アクティブ]** の状態で表示され、また **[メッセージのリテンション期間]** の規定値 (*7*) と、**[パーティション数]** の規定値 (*4*) が表示されます。

    ![Azure portal に表示されるイベント ハブ](../media/3-event-hub.png)

## <a name="summary"></a>まとめ

これで新しいイベント ハブが作成されました。また、パブリッシャーおよびコンシューマー アプリケーションを構成するために必要なすべての情報の準備が完了しました。
