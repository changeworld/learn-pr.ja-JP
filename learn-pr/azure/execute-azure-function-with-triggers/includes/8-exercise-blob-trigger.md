この演習では、BLOB が作成または更新された際のサイズと名前を表示する Azure 関数を作成します。 

> [!NOTE]
> この演習を完了するには、有効なアカウントで [Azure portal](https://portal.azure.com/) にサインインしていることを確認してください。

## <a name="create-a-blob-trigger"></a>BLOB トリガーを作成する

既存の Azure Functions アプリケーションを引き続き使用し、BLOB トリガーを追加してみましょう。

1. **[関数]** をポイントし、プラス (+) アイコンを選択します。

    ![[関数] をポイントし、プラスを選択する](../media/4-hover-function.png)

1. **[BLOB トリガー]** を選択します。

1. 言語として **[C#]** を選択します。 

1. **[名前]** は既定値のままにします。

1. **[パス]** は既定値のままにします。

1. 既存の Azure Storage アカウントを選択するか、または Azure で新しいアカウントを作成する場合は **[作成]** を選択します。

## <a name="download-storage-explorer"></a>Storage Explorer をダウンロードする

これで BLOB トリガーが作成されたので Storage Explorer をダウンロードしてみましょう。これにより BLOB を簡単に作成することができます。

- [Storage Explorer](http://storageexplorer.com) をダウンロードします。

## <a name="connect-to-your-azure-storage-account"></a>Azure Storage アカウントに接続する

これで、Storage Explorer がダウンロードされました。 指定された資格情報を使用してサインインしてみましょう。

1. Storage Explorer の左側にあるプラス (+) アイコンを選択します。

1. **[Use a storage account name and key]\(ストレージ アカウント名とキーを使用\)** を選択します。

1. **[次へ]** を選択します。

1. Azure の BLOB トリガーで、**[統合]** を選択します。

1. **[ドキュメント]** を選択して、ビューを展開します。

1. **[アカウント名]** と **[アカウント キー]** をコピーします。

1. Storage Explorer に戻って、**[アカウント名]** と **[アカウント キー]** に貼り付けます。

1. **[表示名]** を入力します。 この値は、Storage Explorer の接続の名前です。

1. **[次へ]** を選択します。

1. **[接続]** を選択します。 

## <a name="create-a-blob-container"></a>BLOB コンテナーを作成する

ここでは、Azure Storage アカウントに接続されていません。 この BLOB トリガーは、**[パス]** フィールドに示されている場所だけを監視しています。 既定では、パスは次のようになります。

> samples-workitems/{name}

**samples-workitems** という名前のコンテナーを作成する必要があります。

1. Storage Explorer で、ストレージ アカウントを展開します。 名前は、接続プロセス中に指定した **[表示名]** となります。

1. **[BLOB コンテナー]** を右クリックし、**[BLOB コンテナーの作成]** を選択します。

1. 「**samples-workitems**」と入力します。

## <a name="turn-on-your-blob-trigger"></a>BLOB トリガーを有効にする

これで、監視するコンテナーが作成されたので、BLOB の作成時に出力を確認するために関数を実行してみましょう。

1. BLOB トリガーを選択してコード画面を開きます。

1. **[実行]** を選択します。

## <a name="create-a-blob"></a>BLOB を作成する

BLOB トリガーが有効になっており、アクティビティをリッスンしています。 ログ メッセージを取得しているかどうかを確認するために、BLOB を作成してみましょう。

1. Storage Explorer で、**[samples-workitems]** コンテナーを選択します。

1. **[アップロード]** を選択します。 

1. **[ファイルのアップロード]** を選択します。

1. コンピューターから任意のファイルを選択します。

1. **[アップロード]** を選択します。

1. Azure に戻ります。 ログを確認して、アップロードされたファイルを示すメッセージを見つけます。

## <a name="clean-up"></a>クリーンアップ

この関数に対して課金されないように、ログウィンドウの上にある **[一時停止]** を選択します。

![Pause](../media/4-pause-timer.png)


