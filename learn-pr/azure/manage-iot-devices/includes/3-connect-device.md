気象データのキャプチャは、トラフィック パターン暖房、換気、および小売り店で空調 (HVAC) システムの運用に至る気象に与える影響との重要なタスクです。 この演習でシミュレートされたキャプチャの気象データと Azure IoT Hub を使用して、前の単位で学習したオンラインの Raspberry Pi シミュレーター対話するされます。

この演習は、シミュレートされた環境で実施されているが、中に、シミュレートされたデバイスで実行されているアプリケーションに転送できます実際のデバイスを今後。

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

## <a name="create-an-iot-hub"></a>IoT Hub の作成
Azure IoT Hub には、デバイスやバックエンドの開発者が堅牢なデバイス管理ソリューションを構築するために使用できる機能や拡張モデルが用意されています。 デバイスは、リソースの制約の大きいセンサーをはじめ、専用マイクロコントローラー、デバイス グループの通信をルーティングする強力なゲートウェイなど、多岐にわたります。 また、用途や IoT オペレーターの要件は業界によってかなり異なります。 このような違いがありながら、IoT Hub によるデバイス管理で提供される機能、パターン、およびコード ライブラリは、多様なデバイスとエンド ユーザーに対応することができます。

Raspberry Pi シミュレーターからのデータの収集を開始するには、まず、IoT hub を作成する必要があります。

1. [Azure Portal](https://portal.azure.com?azure-portal=true)を開きます。
2. Azure portal の左上隅にある **[リソースの作成]** を選択します。
3. 選択**モ ノのインターネット**、し、 **IoT Hub**します。

![Azure Portal での IoT Hub へのナビゲーションのスクリーン ショット](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. **[IoT Hub]** ウィンドウで、IoT Hub のために以下の情報を入力します。
   
   **サブスクリプション**: この例では、既定のサブスクリプションを使用します。
   **[リソース グループ]**: IoT Hub を入れるリソース グループを作成するか、既存のリソース グループを使用します。 関連するすべてのリソースを 1 つのグループ (*TestResources* など) 内に配置することで、それらを一緒に管理できます。 たとえば、リソース グループを削除すると、そのグループに含まれているすべてのリソースが削除されます。
   **[リージョン]**: 最も近い場所を選択します。
   **[名前]**: IoT ハブの一意の名前を作成します。 入力した名前が使用可能な場合は、緑色のチェック マークが表示されます。

> [!IMPORTANT]
> IoT ハブは DNS エンドポイントとして公開されます。そのため、名前を付ける際は機密情報を含めないようにしてください。

   ![IoT ハブの基本ウィンドウ](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

5. **[Next: Size and scale]\(次へ: サイズとスケール\)** を選択して、IoT ハブの作成を続けます。
6. **[価格とスケールティア]** を選択します。 この例では、選択、 **F1 - 無料**層。

   ![IoT ハブのサイズとスケールのウィンドウ](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

7. **[Review + create]\(レビュー + 作成\)** を選択します。

8. IoT ハブの情報を確認してから、**[作成]** をクリックします。 IoT ハブの作成には数分かかることがあります。 **[通知]** ウィンドウで進行状況を監視できます。

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up tutorial. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a>デバイスの登録
デバイスを IoT ハブに接続するには、あらかじめ IoT ハブに登録しておく必要があります。

1. IoT Hub ナビゲーション メニューの **[IoT デバイス]** を開き、**[追加]** をクリックして IoT Hub でデバイスを登録します。

   ![IoT ハブの [IoT デバイス] でデバイスを追加する](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

2. 新しいデバイスの **[デバイス ID]** を入力します。 デバイス ID には大文字と小文字の区別があります。

> [!IMPORTANT]
> デバイス ID は、カスタマー サポートとトラブルシューティング目的で収集されたログに表示される場合があります。そのため、名前を付ける際は機密情報を含めないようにしてください。

3. **[保存]** をクリックします。
4. デバイスが作成された後、**[IoT デバイス]** ウィンドウの一覧からデバイスを開きます。
5. 後で使用するために **[接続文字列 --- 主キー]** をコピーします。

   ![デバイスの接続文字列を取得する](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a>シミュレートされた利用統計情報の送信

1. 開く、 [Raspberry Pi Azure IoT シミュレーター](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true)します。
2. 行 15 のプレース ホルダーをコピーした Azure IoT hub デバイスの接続文字列に置き換えます。
3. をクリックして、`Run`ボタンまたは型`npm start`アプリケーションを実行するコンソール ウィンドウにします。
   
   ![デバイス接続文字列を置換します。](../media-draft/Line15.png)

センサー データと、IoT hub に送信されるメッセージを表示する次の出力を表示する必要があります。

![出力 - Raspberry Pi から IoT Hub に送信されるセンサー データ](../media-draft/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a>ハブから利用統計情報を読み取る
 これは起こっているでしょうか。 IoT hub には、シミュレートされたデバイスから送信されたデバイスからクラウドへのメッセージが受信します。 ことはしましょうを Azure IoT Hub の概要が受信データが処理を参照してください。 IoT Hub で **監視**を選択します**メトリック**します。 付けます数分の登場にデータを待機するようにします。
   
   ![IoT Hub メトリック](../media-draft/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
