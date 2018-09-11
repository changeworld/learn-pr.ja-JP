これで、Alpine Ski House の Web アプリが起動および実行されたので、あなたはこれを上司に見せる必要があります。 サイトを更新し、それらの更新を Azure に発行する必要があります。

## <a name="update-your-web-app"></a>Web アプリを更新する

### <a name="replace-the-boilerplate-code"></a>定型のコードを置き換える

1. **[ページ]** フォルダーで **[About.cshtml]** ファイルを開きます。

1. コードの下部で `<p> Use this area to provide additional information. </p>` を検索します。

1. 定型のテキストを `Welcome to the Alpine Ski House!` に置き換えます。

1. ファイルを保存します。

1. **About.cshtml.cs** ファイルを開きます。

1. `Message` の文字列を、**Alpine Ski House is the premier ski hill in Northeast.** などに置き換えます。

1. ファイルを保存します。

### <a name="publish-your-updates"></a>更新を発行する

1. ソリューション エクスプローラーで、プロジェクトを右クリックします。

1. **[発行]** を選択します。 お使いの [Web サイト]-Web 配置を含むオプションがあるはずです。

1. ご自分のサイトを選択すると、Visual Studio によって変更点が Azure に送信されます。

### <a name="view-your-changes"></a>変更を確認する

変更を発行したら、Visual Studio によってサイトがブラウザーで開かれます。 **[About]\([概要]\)** ページに移動し、変更が反映されていることを確認します。

## <a name="congrats"></a>おめでとうございます!

Web アプリが Visual Studio 2017 で正常に更新されました。
