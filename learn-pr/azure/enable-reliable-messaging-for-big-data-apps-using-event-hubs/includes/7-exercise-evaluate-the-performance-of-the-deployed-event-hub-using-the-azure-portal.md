このユニットでは、Azure portal を使用して、イベント ハブが想定どおりに動作していることを確認します。 また、一時的に使用できなくなった場合にイベント ハブのメッセージングがどのように動作するかをテストし、Event Hubs メトリックを使用してイベント ハブのパフォーマンスを確認します。

## <a name="view-event-hub-activity"></a>イベント ハブのアクティビティの確認

1. [Azure Portal](https://portal.azure.com?azure-portal=true) にサインインします。

1. 検索バーを使用してイベント ハブを検索し、それを開きます。

1. [概要] ページでメッセージ数を確認します。

    ![イベント ハブのメッセージの確認](../media-draft/6-view-messages.png)

1. SimpleSend アプリケーションと EventProcessorSample アプリケーションは、100 件のメッセージを送信/受信するように構成されています。 イベント ハブが、SimpleSend アプリケーションからの 100 件のメッセージを処理し、EventProcessorSample アプリケーションに 100 件のメッセージを送信したことを確認します。

## <a name="test-event-hub-resilience"></a>イベント ハブの回復力のテスト

次の手順に従って、イベント ハブが一時的に使用不可になっている間にアプリケーションがメッセージを送信するとどうなるかを確認します。

1. SimpleSend アプリケーションを使用して、イベント ハブにメッセージを再送信します。 次のコマンドを使用します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. "**Send Complete...**" と表示されたら、Enter キーを押します。

1. Azure Portal で、**[Event Hubs のインスタンス]** > **[設定]** > **[プロパティ]** の順にクリックします。

1. イベント ハブの状態の下で、**[無効]** をクリックします。

    ![イベント ハブの無効化](../media-draft/7-disable-event-hub.png)

最低でも 5 分間待機します。

1. イベント ハブの状態の下で **[アクティブ]** をクリックしてイベント ハブを再び有効にし、変更を保存します。

1. EventProcessorSample アプリケーションを再実行して、メッセージを受信します。 次のコマンドを使用します。

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. メッセージがコンソールに表示されなくなったら、Enter キーを押します。

1. Azure Portal でイベント ハブの **_namespace_** を検索し、それを開きます。 

1. **[Event Hubs 名前空間]** > **[監視]** > **[メトリック (プレビュー)]** の順にクリックします。

    ![イベント ハブのメトリックの使用](../media-draft/7-event-hub-metrics.png)

1. **[メトリック]** の一覧から **[受信メッセージ]** を選択し、**[メトリックの追加]** をクリックします。

1. **[メトリック]** の一覧から **[送信メッセージ]** を選択し、**[メトリックの追加]** をクリックします。

1. グラフの上部にある **[Last 24 hours (Automatic)]\(過去 24 時間 (自動)\)** をクリックして、期間を **[過去 30 分間]** に変更します。

1. **[適用]** をクリックします。

メッセージを送信したのはイベント ハブがしばらくオフラインになる前でしたが、100 件のメッセージがすべて正常に送信されていることを確認します。

## <a name="summary"></a>まとめ

この演習では、Event Hubs メトリックを使用して、イベント ハブがメッセージの送受信を正常に処理していることをテストしました。
