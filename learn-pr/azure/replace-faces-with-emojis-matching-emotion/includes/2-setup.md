まず最初にローカル複製スタート コードをいくつかのアカウントが設定されていてかどうかを確認する必要があります。

## <a name="create-an-azure-account"></a>Azure アカウントを作成します。

Azure のアカウントが必要です。 既にがないかどうかは、サインアップすることができ、以下のこのリンク サービスの年間無料分を表示。

[Azure アカウントの作成](https://azure.microsoft.com/free)

## <a name="create-a-slack-workspace"></a>Slack のワークスペースを作成します。

Slack のコマンドを作成するには、slack のワークスペースの管理者特権が必要です。

したがって既存のワークスペースの管理者特権を持っているか、まったく新しい slack ワークスペースを作成するかどうかを確認する必要がありますかなど。

[Slack のワークスペースを作成します。](https://slack.com/create)

## <a name="clone-the-starter-code"></a>スタート コードを複製します。

このモジュール最大限にスタート コードを複製し、手順、お勧めします。

アプリケーションを完了する必要があるすべてのコードだけではなく、開始するためのいくつか最初のブートス トラップ コードを提供いたします。

開始するこの複製_stater_コードをローカル コンピューターにします。

```bash
git clone https://github.com/jawache/mojifier-slack.git
```

次を使用して、必要なパッケージをインストールします。

```bash
npm install
```

このモジュールの手順に従うことをお勧めします。 ただし場合のままになる、ヘルプを必要として、検索できますで完成したコード、`completed`ブランチでは、次のようにします。

```bash
git checkout completed
```

使用して、アプリケーションを記述する`TypeScript`します。 NodeJS が把握していない方法**実行**`TypeScript`に変換する必要がありますを開発すると、当社`TypeScript`コードを`JavaScript`します。 変換するためには、上記の手順で変換を実行するすべての必要なツールがインストールされている、`TypeScript`このコマンドを実行します。

```bash
npm run build
```

上記のコマンドが横に正常に実行された場合、`*.ts`ファイルも表示するようになりました`*.js`と`*.map`ファイル。

> **注**
>
> ターミナル シェルで実行されているこのコマンドでこの TypeScript ファイルを変更するを監視し、JavaScript ファイルに変換されます。
>
> ファイルがないいくつかの理由は、コンソール出力ターミナル ウィンドウでのチェック変換を取得し場合があるエラー、TypeScript コードで。

## <a name="install-visual-studio-code--azure-functions-extention"></a>Visual Studio Code と Azure Functions の拡張機能をインストールします。

Azure では、Visual Studio Code と関連付けられている Azure Functions の拡張機能プラグインを使用する Azure とやり取りしますこのモジュールでの対話方法はたくさんあります。

1. ダウンロードしてから visual studio code のインストール https://code.visualstudio.com/

2. 次の手順は、ここでは、Azure Functions コード拡張機能をインストールします。 https://code.visualstudio.com/tutorials/functions-extension/getting-started
