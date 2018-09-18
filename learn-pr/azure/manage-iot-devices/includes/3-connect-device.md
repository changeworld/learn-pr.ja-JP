気象は、トラフィック パターンから小売り店内の HVAC システムの運転状況に至るまで、すべてのことに影響するため、気象データの入手は重要な作業です。 この演習では、オンライン Raspberry Pi シミュレーターを利用してシミュレートされた気象データをキャプチャし、Azure IoT Hub を通じて上記のデータをキャプチャします。

この演習はシミュレートされた環境で実行されますが、シミュレートされたデバイス上で実行されているアプリケーションは、将来、実際のデバイスに転送することができます。

## <a name="create-an-iot-hub"></a>IoT Hub を作成する

1. [Azure portal](https://portal.azure.com/) にサインインします。

1. **[リソースの作成]** \>**[モノのインターネット]** \>**[IoT Hub]** の順に選択します。

![Azure portal での IoT Hub へのナビゲーションのスクリーンショット](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. **[IoT Hub]** ウィンドウで、IoT Hub のために以下の情報を入力します。

 - **[サブスクリプション]**: この IoT Hub を作成するために使用するサブスクリプションを選択します。
 - **[リソース グループ]**: IoT Hub をホストするリソース グループを作成するか、既存のリソース グループを使用します。 詳細については、[リソース グループを使用した Azure リソースの管理](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)に関するページを参照してください。
 - **[リージョン]**: 最も近い場所を選択します。
 - **[名前]**: IoT Hub の名前を作成します。 入力した名前が使用可能な場合は、緑色のチェック マークが表示されます。

> [!IMPORTANT]
> IoT Hub は DNS エンドポイントとして公開されます。そのため、名前を付ける際には機密情報を含めないようにしてください。

   ![IoT Hub の基本ウィンドウ](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  **[Next: Size and scale]\(次へ: サイズとスケール\)** を選択して、IoT ハブの作成を続けます。

1.  **[価格とスケールティア]** を選択します。 この記事では、**[F1 - Free]** レベルを選択します (サブスクリプションで引き続き使用可能な場合)。 詳細については、[料金とスケール レベル](https://azure.microsoft.com/pricing/details/iot-hub/)に関するページを参照してください。

   ![IoT Hub のサイズとスケールのウィンドウ](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  **[Review + create]\(レビュー + 作成\)** を選択します。

1.  IoT Hub の情報を確認してから、**[作成]** をクリックします。 IoT Hub の作成には数分かかることがあります。 **[通知]** ウィンドウで進行状況を監視できます。

IoT Hub を作成できたので、その IoT Hub にデバイスとアプリケーションを接続するために必要な重要な情報を確認します。

IoT Hub ナビゲーション メニューで**共有アクセス ポリシー**を開きます。 **[iothubowner]** ポリシーを選択し、IoT Hub の **[接続文字列 --- 主キー]** をコピーします。 詳細については、「[IoT Hub へのアクセスの制御](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security)」を参照してください。

> [!NOTE]
> このセットアップ チュートリアルでは、この iothubowner 接続文字列は必要ありません。 ただし、このセットアップの完了後、一部のチュートリアルや異なる IoT シナリオでは必要になる場合があります。

![IoT Hub の接続文字列を取得する](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>デバイスの IoT Hub にデバイスを登録する
------------------------------------------------

1.  IoT Hub ナビゲーション メニューの **[IoT デバイス]** を開き、**[追加]** をクリックして IoT Hub にデバイスを登録します。

   ![IoT Hub の [IoT デバイス] でデバイスを追加する](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  新しいデバイスの **[デバイス ID]** を入力します。 デバイス ID には大文字と小文字の区別があります。

> [!IMPORTANT]
> デバイス ID は、カスタマー サポートとトラブルシューティング目的で収集されたログに表示される場合があります。そのため、名前を付ける際は機密情報を含めないようにしてください。

1.  **[保存]** をクリックします。

1.  デバイスが作成された後、**[IoT デバイス]** ウィンドウの一覧からデバイスを開きます。

1.  後で使用するために **[接続文字列 --- 主キー]** をコピーします。

   ![デバイスの接続文字列を取得する](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="run-a-sample-application-on-pi-web-simulator"></a>Pi Web シミュレーターでサンプル アプリケーションを実行する

1. コーディング領域で、既定のサンプル アプリケーションで作業していることを確認します。 行 15 のプレースホルダーを Azure IoT Hub デバイスの接続文字列に置き換えます。

    ![デバイスの接続文字列を置き換える](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  **[実行]** をクリックするか、「npm start」と入力して、アプリケーションを実行します。

IoT Hub に送信されるセンサー データとメッセージを示す次の出力が表示されます

   ![出力 - Raspberry Pi から IoT Hub に送信されるセンサー データ](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
