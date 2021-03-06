ダッシュボードは、ポータルを通じて Azure サービスのさまざまな側面を管理するための柔軟なツールです。 これにより、サービスの状態を簡単に監視できるようになります。 ダッシュボードは共有できるため、チームの全員が同じデータを見て、重要なコンポーネントの状態を常に認識するのに役立ちます。 新しいダッシュボードを作成し、タイルをいくつか追加してみましょう。

## <a name="create-a-new-dashboard"></a>新しいダッシュボードを作成する

1. Azure Portal で **[新しいダッシュボード]** ボタンをクリックします。

1. **[マイ ダッシュボード]** ボックスで、名前を**顧客ダッシュボード**に変更します。

## <a name="add-and-configure-the-clock-tile"></a>[時計] タイルを追加して構成する

1. タイル ギャラリーからワークスペースに時計をドラッグします。 それを、使用可能な領域の右上に配置します。

1. **[時計の編集]** ブレードで、[場所] を **[大阪、札幌、東京]** に変更します。

1. **[時刻の形式]** で、**[24 時間]** をクリックします。

1. **[完了]** をクリックします。

1. 上の 4 つの手順を繰り返し、今度は **[東部標準時 (米国およびカナダ)]** を選択します。 日本の時刻と米国東海岸の時刻を示す 2 つの時計が表示されるようになりました。

## <a name="resize-a-tile"></a>タイルのサイズを変更する

1. **[タイル ギャラリー]** ウィンドウで **[すべてのリソース]** をクリックし、新しいダッシュボード ワークスペースの左上にタイルをドロップします。

1. タイルをクリックし、省略記号を右クリックして、**[6 x 6]** をクリックします。

1. タイルの右下にあるグレーのコーナーをクリックし、タイルのサイズを縦 3.5、横 6 に変更します。 サイズ変更が完了したら、4 x 6 に調整されることに注意してください。

1. タイル ギャラリーで **[リソース グループ]** タイルをクリックして、ワークスペースにドラッグします。 **[すべてのリソース]** タイルの下に配置します。

1. タイル ギャラリーで **[Service Health]** タイルをクリックして、ワークスペースにドラッグします。 **[すべてのリソース]** タイルの右側に配置します。

1. 続けて、次のタイルを追加し、適切に再配置します。

    - ヘルプとサポート
    - クイック タスク
    - マーケットプレース
    - 新機能

1. これらのタイルを追加したら、**[カスタマイズ完了]** をクリックします。 **顧客ダッシュボード** ダッシュボードが表示されます。

## <a name="clone-a-dashboard"></a>ダッシュボードを複製する

次に、他の顧客用に非常によく似たダッシュボードを作成します。

1. **[複製]** ボタンをクリックします。

1. ダッシュボードの名前を、**顧客ダッシュボードの複製**から **Azure AD 管理ダッシュボード**に変更します。

1. **[リソース グループ]** タイルで、ごみ箱アイコンをクリックしてこのタイルを削除します。

1. タイル ギャラリーから、次のタイルを追加します。

    - 組織 ID
    - ユーザーとグループ
    - ユーザー アクティビティの概要
    - Azure AD 管理センターへようこそ

1. 必要に応じてタイルの位置を変更し、**[カスタマイズ完了]** をクリックします。

## <a name="share-a-dashboard"></a>ダッシュボードを共有する

次に、このダッシュボードを他のユーザーも利用できるようにします。 そのためには、次の手順を行います。

1. Azure Active Directory (AD) 管理ダッシュボードが選択されていることを確認し、**[共有]** をクリックします。

1. **[共有 + アクセス制御]** ブレードで、**['dashboards' リソース グループに発行します]** がオンになっていることを確認します。

1. **[場所]** を地理に合わせて適切に設定します。 通常、この値の既定値は最も近いデータ センターです。

1. **[発行]** をクリックした後、**[共有 + アクセス制御]** ブレードを閉じます。

1. **Azure AD 管理ダッシュボード**を選択し、**顧客ダッシュボード**を選択します。

    **[すべてのリソース]** には共有ダッシュボードのリソースが表示され、**[リソース グループ]** にはダッシュボード リソース グループも表示されていることを確認してください。

1. 手順 1 から 3 を繰り返し、顧客ダッシュボードを共有します。

## <a name="edit-a-dashboardjson-file"></a>ダッシュボード.json ファイルを編集する

ダッシュボード ファイルをダウンロードして編集する方法を示すため、次の手順を行います。

1. **[Download]** をクリックします。

1. エクスプローラーを開き、[ダウンロード] フォルダーに移動します。

1. *顧客ダッシュボード.json* ファイルを探し、そのファイルをダブルクリックします。

1. お使いのファイル エディターで、*ClockPart* というテキストを探します。

1. 最初に見つかった ClockPart で、前の **rowSpan** の値を 1 に変更します。

1. 次に見つかった ClockPart でも、前の **rowSpan** の値を 1 に変更します。

1. 2 番目に見つかった Clockpart の Y の値を 2 から 1 に変更します。

1. *顧客ダッシュボード.json* ファイルを保存して、コード エディターを閉じます。

1. Azure ダッシュボードで、**[アップロード]** をクリックします。

1. **[開く]** ダイアログ ボックスで、[ダウンロード] フォルダーに移動し、*顧客ダッシュボード.json* をダブルクリックします。

    時計の高さがどちらも 1 行になり、下の時計が 1 行だけ上に移動していることを確認してください。

## <a name="select-a-shared-dashboard"></a>共有ダッシュボードを選択する

小さい時計は不便なので、顧客ダッシュボードの以前の共有バージョンに戻すことにします。 これは、ファイルを編集して再びアップロードするか、共有バージョンにアクセスすることによって実現できます。 そのためには、次の手順を行います。

1. **顧客ダッシュボード**の隣の下向き矢印をクリックしします。

1. **[すべてのダッシュボードを参照]** をクリックします。

1. **[すべてのダッシュボード]** ブレードの **[タイプ]** で、**[共有ダッシュボード]** を選択します。

1. **顧客ダッシュボード**をクリックします。

1. **[すべてのダッシュボード]** ブレードを閉じます。

    時計が元のサイズに戻ったことを確認してください。

## <a name="switch-to-full-screen"></a>全画面表示に切り替える

1. **顧客ダッシュボード**の隣の下向き矢印をクリックします。 

    横に共有シンボルが付いていないもう 1 つの顧客ダッシュボードがあることに注意してください。 そのバージョンの顧客ダッシュボードをクリックすると、時計が再び小さくなります。

1. 共有バージョンの顧客ダッシュボードに切り替えます。

1. **[全画面表示]** ボタンをクリックします。 

    ブラウザーのメニューとバーがすべて表示されなくなります。

1. 標準の画面に戻すには、**[全画面表示を終了]** をクリックします。

## <a name="unshare-a-dashboard"></a>ダッシュボードの共有を解除する

共有ダッシュボードを選択できないようにする場合は、"_共有を解除_" できます。 ダッシュボードの共有を解除するには、次の手順を行います。

1. **[共有の解除]** ボタンをクリックします。 **[共有 + アクセス制御]** ブレードが表示されます。

1. **[発行の取り消し]** ボタンをクリックします。

1. 確認メッセージ ボックスで、**[はい]** をクリックします。

1. **顧客ダッシュボード**の隣の下向き矢印をクリックしします。

1. **[すべてのダッシュボードを参照]** をクリックします。

1. **[すべてのダッシュボード]** ブレードの **[タイプ]** で、**[共有ダッシュボード]** を選択します。

    使用可能なダッシュボードの一覧に**顧客ダッシュボード**が表示されなくなります。

1. **[すべてのダッシュボード]** ブレードを閉じます。

## <a name="delete-a-dashboard"></a>ダッシュボードを削除する

1. **Azure AD 管理**ダッシュボードが選択されていることを確認します。

1. **[削除]** をクリックします。

1. **[確認]** メッセージ ボックスで、このダッシュボードが表示されなくなることを確認するチェック ボックスをオンにして、**[OK]** をクリックします。

## <a name="reset-a-dashboard"></a>ダッシュボードをリセットする

1. **顧客ダッシュボード**が選択されていることを確認します。

1. **[編集]** をクリックします。

1. ワークスペースを右クリックして、**[既定の状態にリセット]** をクリックします。

1. **[ダッシュボードを既定の状態にリセットしますか?]** メッセージ ボックスで、**[はい]** をクリックします。

    顧客ダッシュボードが既定のタイルにリセットされたことを確認してください。

1. **[カスタマイズ完了]** をクリックします。

1. ポータルの右上にある自分の名前をクリックします。

1. **[サインアウト]** をクリックします。

1. ブラウザーを閉じます。

## <a name="summary"></a>まとめ

ここでは、ダッシュボードの作成と編集、共有、**.JSON** ファイルとしての変更、共有解除、既定の状態へのリセットを行いました。 次は、強力なツールとしてダッシュボードを使用する方法、およびダッシュボードを使用して組織内の異なる役割用に効果的なインターフェイスを作成する方法をご覧ください。
