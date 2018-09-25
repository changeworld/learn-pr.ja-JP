気象は、トラフィック パターンから小売り店内の暖房、換気、空調 (HVAC) システムの運転状況に至るまで、すべてのことに影響するため、気象データの入手は重要な作業です。 この演習では、前のユニットで学習したオンライン Raspberry Pi シミュレーターと対話して、Azure IoT Hub を通じてシミュレートされた気象データをキャプチャします。

[!include[](../../../includes/azure-sandbox-activate.md)]

この演習はシミュレートされた環境で実行されますが、シミュレートされたデバイス上で実行されているアプリケーションは、将来、実際のデバイスに転送することができます。

## <a name="create-an-iot-hub"></a>IoT Hub を作成する
Azure IoT Hub には、デバイスやバックエンドの開発者が堅牢なデバイス管理ソリューションを構築するために使用できる機能や拡張モデルが用意されています。 デバイスは、リソースの制約の大きいセンサーをはじめ、専用マイクロコントローラー、デバイス グループの通信をルーティングする強力なゲートウェイなど、多岐にわたります。 また、用途や IoT オペレーターの要件は業界によってかなり異なります。 このような違いがありながら、IoT Hub によるデバイス管理で提供される機能、パターン、およびコード ライブラリは、多様なデバイスとエンド ユーザーに対応することができます。

Raspberry Pi シミュレーターからのデータの収集を開始するには、まず IoT ハブを作成する必要があります。

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

2. Azure portal の左上隅にある **[リソースの作成]** を選択します。

3. **[モノのインターネット (IoT)]**、**[Event Hubs]** の順に選択します。

![Azure portal での IoT Hub へのナビゲーションのスクリーンショット](../media/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. **[IoT Hub]** ウィンドウで、IoT Hub のために以下の情報を入力します。

   - **[サブスクリプション]**: この例に対する既定のサブスクリプションを使用します。
   - **[リソース グループ]**: 既存のリソース グループを使用します。 関連するすべてのリソースを同じグループ内に配置することで、すべてを一緒に管理できます。 たとえば、リソース グループを削除すると、そのグループに含まれているすべてのリソースが削除されます。
   - **[名前]**: IoT ハブの一意の名前を作成します。 入力した名前が使用可能な場合は、緑色のチェック マークが表示されます。
   - **[リージョン]**: 次の一覧から最も近い場所を選択します。

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    > [!IMPORTANT]
    > IoT ハブは DNS エンドポイントとして公開されます。そのため、名前を付けるときは機密情報を含めないようにしてください。

    ![IoT Hub の基本ウィンドウ](./../media/dbb7319388673b8ee0e0b407536156c0.png)

1. **[Next: Size and scale]\(次へ: サイズとスケール\)** を選択して、IoT ハブの作成を続けます。
2. **[価格とスケールティア]** を選択します。 この例では、**[F1 - Free]** レベルを選択します。

    ![IoT ハブのサイズとスケールのウィンドウ](../media/b506eb3293fa4aa9d4785ad498fc476c.png)

3. **[Review + create]\(レビュー + 作成\)** を選択します。

4. IoT Hub の情報を確認してから、**[作成]** をクリックします。 IoT Hub の作成には数分かかることがあります。 **[通知]** ウィンドウで進行状況を監視できます。

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up exercise. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a>デバイスの登録
デバイスを IoT ハブに接続するには、あらかじめ IoT ハブに登録しておく必要があります。

1. IoT ハブ ナビゲーション メニューの **[IoT デバイス]** を開き、**[追加]** をクリックして IoT ハブにデバイスを登録します。

   ![IoT Hub の [IoT デバイス] でデバイスを追加する](../media/ee5f177abcf06b86dd007fce3b8448ad.png)

2. 新しいデバイスの **[デバイス ID]** を入力します。 デバイス ID には大文字と小文字の区別があります。

    > [!IMPORTANT]
    > デバイス ID は、カスタマー サポートとトラブルシューティング目的で収集されたログに表示される場合があります。そのため、名前を付ける際は機密情報を含めないようにしてください。

3. **[保存]** をクリックします。
4. デバイスが作成された後、**[IoT デバイス]** ウィンドウの一覧からデバイスを開きます。
5. 後で使用するために **[接続文字列 --- 主キー]** をコピーします。

   ![デバイスの接続文字列を取得する](../media/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a>シミュレートされた利用統計情報の送信

1. [Raspberry Pi Azure IoT Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) を開きます。
1. 15 行目のプレースホルダーを Azure IoT ハブ デバイスのコピーした接続文字列に置き換えます。
1. [`Run`] ボタンをクリックするか、コンソール ウィンドウで「`npm start`」と入力して、アプリケーションを実行します。

    ![デバイスの接続文字列を置き換える](../media/Line15.png)

1. IoT ハブに送信されるセンサー データとメッセージを示す次の出力が表示されます。

    ![出力 - Raspberry Pi から IoT ハブに送信されるセンサー データ](../media/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a>ハブから利用統計情報を読み取る
どうなるでしょうか。 IoT ハブは、シミュレートされたデバイスから送信されたデバイスとクラウドの間のメッセージを受信します。 それを確認するために、Azure IoT Hub が受信データを処理するようすを見てみます。 対象の IoT ハブで、**[監視]** の **[メトリック]** を選びます。 データが表示されるまで数分待ちます。

![IoT ハブのメトリック](../media/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
