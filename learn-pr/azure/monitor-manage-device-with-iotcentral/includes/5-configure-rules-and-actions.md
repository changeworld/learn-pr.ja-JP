コーヒー マシンを Azure IoT Central アプリケーションに接続し、コーヒー マシンを監視して管理できるようにするデータの交換を有効にしました。 このモジュールでは、コーヒー マシンの水の温度が通常の範囲外にあるときにアクションをトリガーするルールを作成します。 アクションは、マシンが保証期間内かどうかに応じて、メールまたはモバイル通知のどちらかです。 アクションとして Microsoft Flow を追加するには、Azure サブスクリプションが必要です。 Azure サブスクリプションがない場合は、アクションとしての Microsoft Flow の追加は省略可能です。

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>電子メールをアクションとして IoT Central でルールを作成する
Azure IoT Central には、通知を送信するネイティブなメール機能があります。 このシナリオでは、コーヒー マシンが最適温度の範囲外にあり、保証で保護されていない場合、IoT Central によってクライアントのメンテナンス部門にメールが送信されます。

コーヒー マシンの保証期限が切れていて、水の温度が最適範囲外になったときの、次の 2 つのルールを追加します。 完了したら、**[保存]** を選択します。 

> [!NOTE]
> 条件が適用されるには、実行されるルールのすべてのステートメントが真になる必要があります。 条件がこのシナリオのように "or" ステートメントの場合 (たとえば、保証が期限切れで、温度が 90 度より低いか、または 98 度より高い)、ここで示すように 2 つのルールにステートメントを分割します。

1. ルールの名前: コーヒー メーカーの水温が低すぎ (期限切れ)

    条件の追加:      
    * Device Warranty Expired equals 1
    * Water Temperature is less than Coffee Makers Min Temperature ![ルールの使用](../images/5-flow-g.png)

1. ルールの名前: コーヒー メーカーの水温が高すぎ (期限切れ)

    条件の追加:      
    * Device Warranty Expired equals 1
    * Water Temperature is greater than Coffee Makers Max Temperature ![ルールの使用](../images/5-flow-k.png)     

1. **アクション**を追加するには、[Configure Telemetry Rule]\(テレメトリ ルールの構成\) パネルを下にスクロールし、[アクション] の横にある [+] を選択してから **[メール]** を選択します。

1. アクションを定義するには、IoT Central アプリケーションへのサインインに使用したメール アドレスを追加します。 水温度が暑すぎるときの通知メッセージを追加します。"コーヒー メーカーの水が熱すぎます。 メンテナンスが必要です。  保証は終了しています。" 水温が低すぎる場合について同じ手順を繰り返します。 メッセージを追加します。"コーヒー メーカーの水が冷たすぎます。 メンテナンスが必要です。  保証は終了しています。"

1. **[保存]** を選択します。 ルールが [ルール] ページに表示されます。

1. ルールをトリガーするには、温度が範囲外になるまで待つか、または設定の最適範囲外の温度に意図的に設定します。 メール通知の受信を開始したら、忘れずにルールを無効にして、Azure IoT Central からのメールで受信トレイがあふれないようにします。 

> [!NOTE]
> 検証が終わったら、メールで受信トレイがいっぱいにならないようにルールを無効にします。 

## <a name="create-rules-in-iot-central-with-microsoft-flow-as-the-action"></a>Microsoft Flow をアクションとして IoT Central でルールを作成する

Microsoft Flow は、多くのアプリケーション間のワークフローを自動化します。 IoT Central でルールが発動したときにトリガーできるアクションの 1 つです。 このシナリオでは、コーヒー マシンが特定の温度しきい値に達して、保証が有効なときは、Microsoft Flow は地域の担当者にモバイル通知を送信します。 **[ルール]** に移動して、条件を構成し、ルールが発動したときのアクションとしてフローを追加します。 
 
> [!NOTE]
> Microsoft Flow を有効にするための Azure サブスクリプションがない場合、この演習は省略してかまいません。

コーヒー マシンが保証期間内のときの次のルールを追加します。 

1. ルールの名前: コーヒー メーカーの水温が低すぎ (保証)

    条件の追加:      
    * Device Warranty Expired equals 0
    * Water Temperature is less than Coffee Makers Min Temperature ![ルールの使用](../images/5-flow-l.png)

1. ルールの名前: コーヒー メーカーの水温が高すぎ (保証)

    条件の追加:      
    * Device Warranty Expired equals 0
    * Water Temperature is greater than Coffee Makers Max Temperature ![ルールの使用](../images/5-flow-h.png)

1. ルールの条件を保存した後、コーヒー メーカーが保証期間内のときの新しいアクションとして Microsoft Flow を選択します。 ブラウザー内に新しいタブまたはウィンドウが開き、Microsoft Flow が表示されます。 概要ページが開き、カスタム アクションに接続している IoT Central コネクタが表示されます。 **[続行]** を選択します。 

    ワークフローを作成するための Microsoft Flow デザイナーが表示されます。 ワークフローには、アプリケーションとルールが既に入力されている IoT Central トリガーが含まれています。

    ここでは、必要なすべてのアクションをワークフローに追加できます。 例として、モバイル通知を送信してみましょう。 通知を検索し、[通知] > [Send me a mobile notification]\(モバイル通知を自分に送信する\) を選択します。

    このアクションの [テキスト] フィールドに通知の内容を入力します。 IoT Central ルールから動的なコンテンツを追加して、デバイスの ID や名前などの重要な情報を渡すことができます。
    ![アクションとしての Microsoft Flow の使用](../images/5-flow-f.png)

1. Microsoft Flow でのワークフローの設定が済んだら、Microsoft Store からモバイル デバイスに [Flow アプリ](https://www.microsoft.com/en-us/p/microsoft-flow/9nkn0p5l9n84?activetab=pivot%3aoverviewtab)をダウンロードします。 Flow Web アプリでフローを設定するために使用したものと同じアカウントを使用してサインインします。 テストのため、最適範囲外の温度を設定してルールをトリガーします。 

    > [!NOTE]
    > デバイスのプロパティ: Device Warranty Expired (デバイス コードで 1 は保証期限切れ、0 は期間内) は、デバイスによってランダムに生成され、デバイスによって Azure IoT Central アプリケーションに送信されます。 テストのため、Device Warranty Expired (1 または 0) を制御したい場合は、テストするアクションをトリガーする目的の保証状態を受信するまで、シミュレーターを再起動します。 モバイル通知を受信するには、コンソール ログで Device Warranty Expired が 0 と表示されるまで、シミュレーターを再起動します。 

    数分後、Flow モバイル アプリに通知が表示されます。

    ![アクションとしての Microsoft Flow の使用](../images/5-flow-a.png)

## <a name="summary"></a>まとめ
IoT Central でルールを作成する方法を学習し、ルールが発動したときに Microsoft Flow を介してメールやモバイル通知などのアクションをトリガーしました。 この例では、コーヒー マシンの水温が最適範囲から外れると、保証の状態に応じて、通知が修理技術者またはクライアントに送信されます。 


