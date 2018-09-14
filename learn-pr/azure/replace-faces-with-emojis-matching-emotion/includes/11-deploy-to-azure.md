これまでに、このモジュールでこうして、Slack を取得する_スラッシュ_ngrok を使用して、コンピューター上にトンネリングして localhost 上で実行中にのみ操作コマンド。 これは、開発に適しては、クラウドで実行するコードを解放します。

この講義で行う、Azure 関数をクラウドにデプロイし、コードの新しい運用バージョンを使用する slack の構成を更新します。

この講義の終わりまでには、次の説明は。

- Azure での Azure Function App を作成する方法
- Visual Studio Code を使用して Azure にデプロイする方法
- 反映する方法を運用環境にローカルの設定

## <a name="create-an-azure-function-app-on-azure"></a>Azure で Azure 関数アプリを作成します。

VS Code と Azure 関数の拡張機能を使用して azure 関数を作成するでしょう。

1. コマンド パレットを開いて<kbd>Ctrl</kbd>+<kbd>P</kbd>選択と`Azure Function: Deploy to Function App`します。

   ![関数アプリをデプロイします。](/media-drafts/10.deploy-to-function-app.png)

2. 現在のプロジェクトをアップロードしてください、それを選択することを確認することが表示されます。

   ![配置先のフォルダーを選択します。](/media-drafts/10.select-folder-to-deploy.png)

3. アプリに関連付けるサブスクリプションを選択します。

   Azure に慣れていない場合は、サブスクリプションが 1 つ、その接続を選択する必要があります。

4. 選択して新しい function app の作成

   ![新しい Function App を作成します。](/media-drafts/10.create-new-function-app.png)

5. アプリの名前を選択します。

   名前を選択で、必要な値に表示されます。 私を呼び出している`mojifier-slack-function-app`します。

   ![アプリ名を選択します。](/media-drafts/10.choose-app-name.png)

6. 新しいリソース グループを作成する

   リソース グループは、1 つに、Azure で複数のサービスをどのようにグループ化します_アプリ_します。 新しいまたは既存の 1 つのことを既に作成して使用を作成することができます。

   その 1 つは、自動的に設定します名前は新しいを作成する場合は、を選択することもか、または別を入力することができます。

7. 選択_新しいストレージ アカウント作成_です。

   関数アプリは、ストアのデータ ファイルとファイルをより永続的な場所のストレージ アカウントが必要です。 新しいまたは既存の 1 つのことを既に作成して使用を作成することができます。

   自動的に設定します。 名前選択するかを 1 つ入力できます。

8. リージョンを選択します。

   関数アプリは、どこにします。 またはお客様のユーザーに最も近い場所を選択することをお勧めします。 選択したいます `West Europe`

   ![アプリ名を選択します。](/media-drafts/10.select-region.png)

9. 完了すると、出力ウィンドウに表示 URL を完了すると同様に、アップロードのため待機します。

```output
Deployment to "mojifier-slack-function-app" completed.
HTTP Trigger Urls:
  MojifyImage: https://mojifier-slack-function-app.azurewebsites.net/api/MojifyImage
```

## <a name="try-it-out"></a>試してみる

この時点では、少なくともが予想される、`RespondToSlackCommand`に他のすべての依存関係に依存しないために、作業関数。

参照してください。 `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`

すべてが動作してもする場合はブラウザーのウィンドウで返される JSON 次のように。

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "https://mojifier-slack.azurewebsites.net/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```

## <a name="upload-settings"></a>アップロードの設定

一部の設定があることに注意してください。 `local.settings.json` cognitive services のキーと URL でした。

`local.settings.json` ローカル、そのことはありませんにコピーされます運用展開するときにまったくです。

通常は、ポータルに移動し、おそらく UI または使用して経由での設定で手動で追加する必要がありますには、 `func` CLI で、同じ操作を実行します。

ただし、ため VS Code を使用していること使用できます Func の Azure 拡張機能とコマンド パレットのローカル設定をアップロードします。

1.  コマンド パレットを開いて<kbd>Ctrl</kbd>+<kbd>P</kbd>選択と`Azure Functions: Upload Local Settings`します。

    ![ローカル設定をアップロードします。](/media-drafts/10.upload-local-settings.png)

2.  選択 `local.settings.json`

    ![ローカル設定を選択します。](/media-drafts/10.choose-localsettings.png)

3.  Function app に関連付けられているサブスクリプションを選択します。

4.  アップロードする関数アプリを選択し、私が呼び出されます `mojifier-slack-function-app`

5.  すべての設定を上書きすることが求め場合の選択します。 `No To All`

    これは、アップロードのみを行う新しい設定は必要な運用環境の設定を開発から設定をオーバーライドするリスクがあるそれ以外の場合。

    ![ローカル設定を選択します。](/media-drafts/10.choose-no-to-all.png)

## <a name="try-it-out"></a>試してみる

今すぐすべて予想どおりに動作している場合、ようになりました MojifyImage エンドポイントは動作します。

参照してください。 `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`

問題なく動作はすべてと、ウィンドウの mojified 画像が表示されます。

動作しない場合は、ここの問題のトラブルシューティングから再試行してください。

## <a name="update-slack"></a>Slack を更新します。

指す新しい Slack での設定をアップデートできる関数をクラウドにデプロイされましたので_パブリック_URL。

1. パブリック URL を使用して、Slack の更新、`RespondToSlackCommand`します。

![運用 URL での Slack を更新します。](/media-drafts/10.deploy-update-url.png)

> **注**
>
> 更新する、実稼働に使用するコマンド_パブリック_のバージョン、`RespondToSlackCommand`でホストされています。 `*.azurewebsites.net`

## <a name="try-it-out"></a>試してみる

すべてが正しく構成されていると、Slack がローカルのアプリではなく、デプロイされたアプリからデータを要求することを確認するには、オフにできるように`ngrok`今のところです。

Slack ウィンドウを好きな slack スラッシュ コマンドを入力し、ヘッドします。

`/mojify <image>`

かどうかは、すべての作業が想定どおり slack のウィンドウに表示されるイメージが表示されます、次のようにします。

![Win](/media-drafts/10.publish-success.png)
