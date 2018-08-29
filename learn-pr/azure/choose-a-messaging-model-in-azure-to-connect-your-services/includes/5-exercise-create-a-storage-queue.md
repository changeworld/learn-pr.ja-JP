この演習では、Azure サブスクリプションで新しいストレージ アカウントを作成します。 次に Azure Cloud Shell を使用して新しいキューを作成し、それにメッセージを追加し、そのメッセージを読み、キューから削除します。

これらは、分散アプリケーションのコンポーネントによって行われる同じアクションです。 たとえば、モバイル アプリがメッセージをキューに追加し、Web サービスがメッセージを取得して処理するまで待機することがあります。

## <a name="create-a-storage-account"></a>ストレージ アカウントの作成

ストレージ キューは Azure の汎用ストレージ アカウントの一部です。 まず、ストレージ アカウントを作成する必要があります。

1. ブラウザーで [Azure Portal](http://portal.azure.com) に移動し、通常の資格情報でサインインします。
1. 左上の **[すべてのサービス]** をクリックします。
1. 下へスクロールして **[ストレージ]** セクションを表示し、**[ストレージ アカウント]** をクリックします。
1. **[ストレージ アカウント]** ブレードの左上にある **[追加]** をクリックします。

    ![ストレージ アカウントの作成](../images/5-create-a-storage-account-1.png)

1. **[名前]** テキスト ボックスにストレージ アカウントの一意の名前を入力します。
1. **[デプロイ モデル]** で **[Resource Manager]** が選択されていることを確認します。
1. **[アカウントの種類]** ドロップダウン リストで **[ストレージ (汎用 v2)]** を選択します。
1. **[場所]** ドロップダウン リストで、近くのリージョンを選択します。
1. **[レプリケーション]** ドロップダウン リストで、**[ローカル冗長ストレージ (LRS)]** を選択します。
1. **[パフォーマンス]** で **[Standard]** を選択します。
1. **[アクセス層]** で **[クール]** を選択します。
1. **[安全な転送が必須]** で **[無効]** を選択します。
1. **[サブスクリプション]** でご使用のサブスクリプションを選択します。

    ![ストレージ アカウントの作成](../images/5-create-a-storage-account-2.png)

1. **[リソース グループ]** で **[新規作成]** を選択し、テキスト ボックスに「**MusicSharingResourceGroup**」と入力します。
1. **[仮想ネットワーク]** で **[無効]** を選択し、**[作成]** をクリックします。

    ![ストレージ アカウントの作成](../images/5-create-a-storage-account-3.png)

Azure によって新しいストレージ アカウントと新しいリソース グループが作成されます。

## <a name="create-a-queue"></a>キューを作成する

これでストレージ アカウントが作成されたので、それに新しいキューを追加できます。 PowerShell コマンドを使用してキューを作成する必要があります。

1. ポータルの上部で、**[Cloud Shell]** リンクをクリックします。

    ![Cloud Shell の起動](../images/5-create-a-storage-queue-1.png)

1. **[Azure Cloud Shell へようこそ]** 画面で **[PowerShell (Linux)]** をクリックします。
1. **[ストレージがマウントされていません]** 画面が表示されたら、**[ストレージの作成]** をクリックします。
1. `PS Azure` プロンプトが表示されたら、ストレージ アカウントを取得するために、次のコマンドを入力します。`<storageaccountname>` をお使いのストレージ アカウントの一意の名前に変更し、Enter キーを押します。

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. ストレージ アカウントのコンテキストを取得するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $context = $storageaccount.Context
    ```

1. 新しいキューを作成するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a>メッセージをキューに追加する

これでストレージ アカウントにキューが作成されたので、それにメッセージを追加できます。

1. 新しいメッセージを作成するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. 新しいメッセージを新しいキューに追加するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. Azure Portal の左側のナビゲーションで **[すべてのリソース]** をクリックします。
1. リソースの一覧で、先に作成したストレージ アカウントをクリックします。
1. [ストレージ アカウント] ブレードで、**[ストレージ エクスプローラー (プレビュー)]** をクリックします。
1. ストレージ エクスプローラーの **[キュー]** の下で **[musicsharingmessages]** をクリックします。 ストレージ エクスプローラーには、追加したメッセージが表示されます。

## <a name="retrieve-and-remove-the-message"></a>メッセージの取得と削除

ストレージ キューのメッセージに対する宛先コンポーネントでは、キューの先頭でメッセージを取得し、それを処理したら、他のコンポーネントによって取得されないようにキューから削除する必要があります。

1. Azure Cloud Shell で、キューの先頭にあるメッセージを取得するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. メッセージを表示するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $retrievedMessage.AsString
    ```

1. メッセージのプロパティをすべて表示するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $retrievedMessage
    ```

1. キューからメッセージを削除するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. Azure Portal でキュー ディスプレイを更新するには、[ストレージ アカウント] ブレードで、**[概要]** をクリックし、**[ストレージ エクスプローラー]** をクリックします。
1. **[キュー]** の下で **[musicsharingmessages]** をクリックします。 メッセージのみを削除したため、ストレージ エクスプローラーにはキューが空であることが表示されます。

## <a name="cleanup"></a>クリーンアップ

この演習中に作成したリソースをすべて削除するには、Azure Cloud Shell に次のコマンドを入力します 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a>まとめ

今回、Azure サブスクリプションでストレージ アカウントを作成し、その中で新しいキューを作成しました。 また、PowerShell を使用し、キューにメッセージを追加したらそれを取得し、削除することで、分散アプリケーション コンポーネントのアクションをシミュレーションしました。

Azure Storage アカウントのキューは、分散アプリケーションのコンポーネント間でメッセージを渡す場合に最適なソリューションです。 イベントを発行するときはストレージ キューを選択しないでください。