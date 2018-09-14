すべての必要な Azure functions を作成しましたし、それらをローカルで実行できます。

この講義で Slack で、Azure Functions を接続し、Slack 作成_スラッシュ_コマンドを Azure 関数の呼び出しを slack のウィンドウで、結果として得られる mojified イメージを表示します。

この講義の終わりまでには、次の説明は。

- Slack アプリを作成し、スラッシュのコマンドを Azure 関数のエンドポイントに接続する方法。
- をクラウドに展開することがなく、スラッシュのコマンドをローカルをテストする方法

## <a name="using-ngrok-to-develop-with-slash-commands"></a>Ngrok を使用して、スラッシュのコマンドを使用して開発するには

Azure 関数が現在実行中のローカル; だけただし、slack は、インターネットでは、クラウドで実行されます。

設定すると、slack を_スラッシュ_スラッシュ コマンドをユーザーが実行するたびに呼び出しをパブリック URL を要求するこれは、次のセクションでコマンドします。

付与する、`RespondToSlackCommand`ローカル コンピューターですが、URL がのみ存在します。

方法はどれですサービス、クラウドへのアクセスでリソースをローカル コンピューターに開発用?

この難問に対する優れたソリューションが`ngrok`これは、インターネットからローカル コンピューターにセキュリティで保護されたトンネルを作成するツールです。

1. 参照してください。 https://ngrok.com/download
2. ダウンロードしてインストールする手順に従って、`ngrok`ローカル コンピューターにパッケージします。
3. 実行`ngrok http 7071`ターミナルでします。
   ![ngrok](/media-drafts/9.ngrok.png)
4. これで終わるパブリック URL を出力`ngrok.io`など`http://bbade778.ngrok.io`、メモ、これを次のセクションで必要になります。

> **注**
>
> これは、 `http://*.ngrock.io` URL を使用したのと同様に使用することができます`http://localhost:7071`イメージを試すようになります `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`

## <a name="create-a-slack-app"></a>Slack アプリを作成します。

スラッシュ コマンドは、slack、slack アプリの一部として存在_ボット_します。

参照してください[Slack アプリの作成](https://api.slack.com/apps/new)

選択、`App Name`と関連付けること、`Workspace`このチュートリアルを押してからの開始時に作成した`Create App`、次のようにします。

![Slack アプリを作成します。](/media-drafts/9.create-slack-app.png)

作成されると、slack アプリに移動し、スラッシュのコマンドを作成する必要があります。

## <a name="create-a-slash-command"></a>スラッシュ コマンドを作成します。

をクリックして、`Slash Commands`メニュー項目。

![スラッシュ コマンド](/media-drafts/9.slash-commands.png)

[`Create New Command`] をクリックします。

- 任意で、スラッシュ コマンドの名前を入力してください、`command`フィールド。
- Ngrok パブリック URL を入力、`Request URL`フィールド

  > **大事な**
  >
  > 追加する`/api/RespondToSlackCommand`url

- 簡単な説明と使用状況のヒントを追加します。
- キーを押します `Save`

![スラッシュ コマンド](/media-drafts/9.create-slash-command.png)

画面を表示する必要がありますにいったん保存されるようになります。

![スラッシュ コマンドの成功](/media-drafts/9.create-slash-commands-success.png)

## <a name="install-the-slack-app-to-the-workspace"></a>ワークスペースへの slack アプリをインストールします。

Slack アプリ ワークスペースで使用できる、前にインストールする必要があります。

1. 選択、`Basic Information`メニュー項目

2. 展開し、`Install your app to your workspace`オプションとキーを押して、`Install app to workspace`ボタンをクリックします。

   ![ワークスペースにアプリをインストールします。](/media-drafts/9.install-app-to-workspace.png)

3. アプリケーションを認証するように要求が次のようにします。

   ![アプリ ワークスペースへの認証します。](/media-drafts/9.authenticate-slack-app.png)

## <a name="try-it-out"></a>試してみる

これで、スラッシュ コマンドを作成してみましょう ngrok リンク経由で Azure Functions のローカルで実行中インスタンスに接続をテストします。

1. Slack に slack アプリに関連付けられているワークスペースに移動します。
2. すべての型とチャット ウィンドウに移動して`/mojify`(または、アプリを指定した名前)
3. すべてが正しく機能、表示されるはず`mojify`オプションとして

   ![Mojify を確認してください。](/media-drafts/9.slack-check-mojify.png)

4. ここで「`/mojify`し画像 URL を追加し、、enter キーを押して次ように。

   ![型 Mojify](/media-drafts/9.slack-type-mojify.png)
