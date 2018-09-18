このユニットでは、Visual Studio を Windows または macOS コンピューターにインストールします。 Windows 上には、Azure 開発ワークロードをインストールする必要があります。 Visual Studio for Mac では、組み込みの接続済みサービス ワークフローを使用して、Azure App Service 用のアプリを構築できます。 最後には、アプリケーションを作成する準備が完了し、それを Azure に公開できるようになります。

## <a name="exercise-steps"></a>演習の手順

Visual Studio を Windows と macOS にインストールする場合には、若干の違いがあります。 次のセクションで、これらの違いを説明します。

### <a name="windows"></a>Windows

1. Visual Studio のインストーラーを、 https://visualstudio.microsoft.com/downloads/ からダウンロードします。

1. インストーラーを実行すると、ワークロード ウィンドウが開きます。

1. **[Azure の開発]** ワークロードを選びます。

    次のスクリーンショットは、Visual Studio で Azure を開発するために Visual Studio インストーラーのワークロードが選択されているところを示しています。

    ![Azure 開発ワークロードが強調表示された Visual Studio インストーラーのスクリーンショット。](../media/5-select-azure-workload.png)

1. (省略可能) Azure で使用する Web アプリケーションを作成するために、ASP.NET および Web 開発ワークロードをインストールします。

1. **[インストール]** をクリックし、Visual Studio がインストールされるのを待機します。

1. インストールが完了したら、Visual Studio を開きます。

1. Visual Studio の [表示] メニューに移動し、**[Cloud Explorer]** オプションがあることを確認します。

    次のスクリーン ショットでは、Azure 開発ワークロードがインストールされている場合に存在する Cloud Explorer のメニュー オプションを示します。

    ![Cloud Explorer のメニュー オプションが強調表示された Visual Studio の [表示] メニューのスクリーンショット。](../media/5-verify-cloud-explorer.png)

### <a name="macos"></a>macOS

1. https://visualstudio.microsoft.com/ に移動し、Visual Studio for Mac インストーラーをダウンロードします。

1. VisualStudioInstaller.dmg ファイルをクリックしてインストーラーをマウントし、ロゴをダブルクリックして実行します。

1. 表示されたプライバシーとライセンス条項に同意します。

1. インストーラーにより、どのコンポーネントをインスト―ルするか尋ねられます。 Azure のコンポーネントは既に Visual Studio for Mac の一部ですが、Azure 用の Web エクスペリエンスを開発するには、**.NET Core** プラットフォームをインストールすることが推奨されます。

    次のスクリーン ショットは、Visual Studio for Mac で Azure を開発する機能を追加するために必要な .NET Core プラットフォームを示しています。

    ![選択されている .NET Core プラットフォーム オプションが強調表示された Visual Studio for Mac インストーラーのスクリーン ショット。](../media/5-vsmac-install-net-core.png)

1. 選択に問題がなければ、**[Install and Update]** \(インストールして更新\) をクリックし、インストーラーが完了するまで待機します。

1. 必要なアクセス許可を昇格するように求められた場合、それの実行に管理者の資格情報を使用します。

1. インストーラーが完了したら、Visual Studio for Mac を開始します。
