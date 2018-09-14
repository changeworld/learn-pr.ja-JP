すべての必要な Azure functions を作成しましたし、それらをローカルで実行できます。

この講義で Slack で、Azure Functions を接続し、Slack 作成_スラッシュ_コマンドを Azure 関数の呼び出しを slack のウィンドウで、結果として得られる mojified イメージを表示します。

この講義の終わりまでには、次の説明は。

- Slack アプリを作成し、スラッシュのコマンドを Azure 関数のエンドポイントに接続する方法。
- をクラウドに展開することがなく、スラッシュのコマンドをローカルをテストする方法

## <a name="using-ngrok-to-develop-with-slash-commands"></a>ngrok を使用してスラッシュ コマンドで開発する

Azure 関数が現在実行中のローカル; だけただし、slack は、インターネットでは、クラウドで実行されます。

設定すると、slack を_スラッシュ_スラッシュ コマンドをユーザーが実行するたびに呼び出しをパブリック URL を要求するこれは、次のセクションでコマンドします。

付与する、`RespondToSlackCommand`ローカル コンピューターですが、URL がのみ存在します。

クラウド内のサービスが開発用ローカル コンピューター上のリソースにアクセスできるようにするには、どうすればよいでしょうか。

この難問に対する優れたソリューションが`ngrok`これは、インターネットからローカル コンピューターにセキュリティで保護されたトンネルを作成するツールです。

1. https://ngrok.com/download を参照してください
2. 指示に従って `ngrok` パッケージをローカル コンピューターにダウンロードしてインストールします。
3. ターミナルで `ngrok http 7071` を実行します。
   ![ngrok](/media-drafts/9.ngrok.png)
4. これで終わるパブリック URL を出力`ngrok.io`など`http://bbade778.ngrok.io`、メモ、これを次のセクションで必要になります。

> **注**
>
> これは、 `http://*.ngrock.io` URL を使用したのと同様に使用することができます`http://localhost:7071`イメージを試すようになります `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`

## <a name="create-a-slack-app"></a>Slack アプリを作成する

スラッシュ コマンドは、Slack アプリである Slack "_ボット_" の一部として存在します。

「[Create Slack App](https://api.slack.com/apps/new)」(Slack アプリの作成) にアクセスします

選択、`App Name`と関連付けること、`Workspace`この演習を押してからの開始時に作成した`Create App`、次のようにします。

![Slack アプリを作成する](/media-drafts/9.create-slack-app.png)

作成されたら、Slack アプリに移動し、スラッシュ コマンドを作成する必要があります。

## <a name="create-a-slash-command"></a>スラッシュ コマンドを作成する

をクリックして、`Slash Commands`メニュー項目。

![スラッシュ コマンド](/media-drafts/9.slash-commands.png)

[`Create New Command`] をクリックします。

- [`command`]\(コマンド\) フィールドに任意のスラッシュ コマンド名を入力します。
- Ngrok パブリック URL を入力、`Request URL`フィールド

  > **大事な**
  >
  > 追加する`/api/RespondToSlackCommand`url

- 簡単な説明と使用のヒントを追加します。
- [`Save`]\(保存\) をクリックします

![スラッシュ コマンド](/media-drafts/9.create-slash-command.png)

保存すると次のような画面が表示されます。

![スラッシュ コマンド成功](/media-drafts/9.create-slash-commands-success.png)

## <a name="install-the-slack-app-to-the-workspace"></a>ワークスペースへの slack アプリをインストールします。

Slack アプリ ワークスペースで使用できる、前にインストールする必要があります。

1. [`Basic Information`]\(基本情報\) メニュー項目を選択します

2. [`Install your app to your workspace`]\(ワークスペースにアプリをインストールする\) オプションを展開し、[`Install app to workspace`]\(アプリをワークスペースにインストールする\) ボタンをクリックします。

   ![アプリをワークスペースにインストールする](/media-drafts/9.install-app-to-workspace.png)

3. 次のようにアプリケーションの認証を求められる場合があります。

   ![ワークスペースに対してアプリを認証する](/media-drafts/9.authenticate-slack-app.png)

## <a name="try-it-out"></a>試してみる

これで、スラッシュ コマンドを作成してみましょう ngrok リンク経由で Azure Functions のローカルで実行中インスタンスに接続をテストします。

1. Slack アプリに関連付けた Slack ワークスペースに移動します。
2. 任意のチャット ウィンドウに移動して「`/mojify`」 (または、アプリに指定した名前) と入力します
3. すべてが正しく機能、表示されるはず`mojify`オプションとして

   ![Mojify を確認する](/media-drafts/9.slack-check-mojify.png)

4. ここで「`/mojify`し画像 URL を追加し、、enter キーを押して次ように。

   ![Mojify と入力する](/media-drafts/9.slack-type-mojify.png)
