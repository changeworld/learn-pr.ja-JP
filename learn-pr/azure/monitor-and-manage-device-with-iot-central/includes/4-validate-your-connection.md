Azure IoT Central アプリケーションを使用し、コーヒー マシンを Azure IoT Central に接続しました。 リモート コーヒー マシンの監視と管理を開始するまでの手順を順調に進んでいます。 このユニットでは、少し時間を取って、先ほど定義した接続されているコーヒー メーカー テンプレートを使用して、セットアップと接続を検証します。 設定の最適温度を更新し、コマンドを実行してマシンの状態を確認し、ダッシュボードで接続されているコーヒー マシンを表示します。 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>設定を更新してアプリケーションとコーヒー マシンを同期させる

**[設定]** ページで、アプリケーションからコーヒー マシンに構成データを送信します。 

このシナリオでは、最適温度を変更して、**[更新]** を選択します。 設定が変更されると、コーヒー マシンで設定変更に対する応答が確認されるまで、設定は UI で保留中としてマークされます。 

> [!NOTE]
> 設定が正常に更新されると、データ フローが示され、接続が検証されます。 テレメトリの測定値は、最適温度の更新に応答します。 **[測定]** ページで変更を確認できます。 

## <a name="run-commands-on-the-coffee-machine"></a>コーヒー マシンに対してコマンドを実行する 
次の演習では、**[コマンド]** ページに移動します。 コマンドのセットアップを検証するには、IoT Central からコーヒー マシンに対してリモートでコマンドを実行します。 コマンドが正常に実行された場合は、コーヒー マシンから確認メッセージが送信されます。

1. **[実行]** を選択し、リモートで抽出を開始します。 
    
    次の 3 つの条件が満たされている場合、コーヒー マシンが起動します。
    - カップが検出された
    - メンテナンス中ではない
    - まだ抽出していない  

    > [!NOTE]
    > 抽出が正常に開始されると、**[測定]** > **[状態]** で示されるマシンの状態が黄色に変わります。 
    
    node.js でシミュレートされたコーヒー マシンのコンソール ログで確認メッセージを探します。 

    ![コマンドを実行する](../media/4-commands-brewing.png)

1. **[実行]** を選択して、メンテナンス モードを設定します。 まだメンテナンスになって*いない*場合は、コーヒー マシンがメンテナンスに設定されます。
    
    コーヒー マシンのコンソール ログで確認メッセージを探します。 

    > [!NOTE]
    > 実際に技術者がマシンをオフラインにして必要な修理を行ってからオンラインに戻すのと同じように、クライアント コードを再起動するまで、コーヒー マシンはメンテナンス モードのままになっています。

    ![コマンドを実行する](../media/4-commands-maintenance.png)

> [!IMPORTANT]
> Node.js アプリケーションを 60 分を超えない程度で実行し、アプリケーションから不要な通知や電子メールが送信されないようにすることをお勧めします。 モジュールで操作していない場合はアプリケーションを停止することで、1 日のメッセージ クォータを使い切ることもなくなります。

## <a name="view-the-coffee-machine-in-the-dashboard"></a>ダッシュボードでコーヒー マシンを表示する

1. **[ダッシュボード]** タブに移動します。

1. **[テンプレートの編集]** を選択してダッシュボードを編集します。

1. サイド メニューから **[折れ線グラフ]** を選択します。

1. **[グラフの構成]** の **[タイトル]** フィールドに「`Telemetry`」と入力します。 このグラフで利用統計情報を確認しましょう。 

1. **[時間範囲]** で **[過去 30 分]** を選択します。 

1. **[測定]** 領域で、各測定値の右側にある表示アイコンを選択して、測定値がグラフに表示されるようにします。 

1. **[保存]** を選択して構成を保存し、折れ線グラフを表示します。 

    ![ダッシュボードの表示](../media/4-dashboard-a.png)

1. 左側のメニューにある **[Settings and Properties]\(設定とプロパティ\)** を選択し、**[Configure Device Details]\(デバイスの詳細の構成\)** パネルを開きます。 

1. **[タイトル]** フィールドに「`Device properties`」と入力します。

1. **[追加/削除]** で、**[Coffee Makers Max Temperature]\(コーヒー メーカー最高温度\)**、**[Coffee Makers Min Temperature]\(コーヒー メーカー最低温度\)**、**[Device Warranty Expired]\(デバイスの保証期限切れ\)** を選択し、**[OK]** を選択して選択内容を確定します。

1. **[保存]** を選択して、ダッシュボードに新しい *[デバイスのプロパティ]* パネルを作成します。 

1. **[Settings and Properties]\(設定とプロパティ\)** をもう一度選択し、タイトルに「`Optimal Temperature`」と入力します。 **[追加/削除]** で、**[Optimal Temperature]\(最適温度\)** を選択します。

1. **[保存]** を選択して作業内容を保存し、ダッシュボードに新しいウィンドウを表示します。 

1. **[完了]** を選択して編集モードを終了し、新しいダッシュボードを表示します。 