新しいサイトを準備するときにまずやることは、開発環境の準備です。 ASP.NET Web アプリケーションを作成して展開するには、ローカル コンピューターに必要なツールがインストールされている必要があります。 ここでは、必要なツールとそれらをインストールする方法を説明します。

## <a name="prepare-your-development-environment"></a>開発環境を準備する

Visual Studio 2017 では、作成、発行、および、web サイトを Azure にデプロイする必要がある 2 つのワークロードがあります。 これらのワークロードには、ASP.NET サイト用のすべてテンプレートが含まれ、サイトを Azure に接続して展開する機能が備わっています。

次のワークロードがインストールされていることを確認する必要があります。

- ASP.NET と Web 開発

Visual Studio 2017 における web 開発ワークロードは、ASP.NET と HTML および JavaScript のような標準ベースのテクノロジを使用して web アプリケーションの開発で生産性が最大に設計されています。

- Azure の開発

Visual Studio 2017 の Azure の開発ワークロードでは、最新の Azure SDK for .NET と Visual Studio 用のツールがインストールされます。 これらがインストールされると、Cloud Explorer でリソースを表示し、Azure Resource Manager ツールを使用してリソースを作成し、Azure Web サービスや Cloud Services 用のアプリケーションを構築し、Azure Data Lake ツールを使用してビッグ データの操作を実行することができます。

## <a name="how-to-install-the-required-workloads"></a>必要なワークロードをインストールする方法

Visual Studio インストーラーを使用して、Visual Studio の一部としてインストールされるコンポーネントを変更します。

- インストーラーを起動するには、Windows の [スタート] メニューで **V** まで下にスクロールして、**[Visual Studio インストーラー]** をクリックします。 または、[スタート] メニューを開き、「```Visual Studio Installer```」と入力してインストーラーのリンクを探します。 次に、**Enter** キーを押します。

- Visual Studio インストーラー ウィンドウが表示されます。 **[変更]** ボタンをクリックします。 ボタンが表示されない場合は、**[詳細]** ドロップダウン メニューで **[変更]** を選択できます。

    ![Visual Studio を変更する](../media-draft/3-visual-studio-installer-modify.PNG)

- **[ワークロード]** タブの **[Web & Cloud]\(Web とクラウド\)** セクションで **[ASP.NET と Web 開発]** および **[Azure の開発]** ワークロードが選択されていることを確認します。 ![ワークロードをインストールする](../media-draft/2-select-workloads.png)

次に、インストーラーの右下にある **[変更]** ボタンをクリックします。 Visual Studio インストーラーで、必要なコンポーネントがダウンロードされてインストールされます。 これで、ASP.NET Web アプリを作成して Microsoft Azure にアップロードできるようになりました。

> [!IMPORTANT]
> Visual Studio for Mac では、必要なワークロードがプレインストールされている "_はずです_"。 再インストールする必要がある場合は、[Visual Studio for Mac](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio-mac/?sku=communitymac&rel=15_) をダウンロードする必要があります。ダウンロードするとインストーラーが実行されます。 そこから、追加するワークロードを選択することができます。

## <a name="summary"></a>まとめ

Visual Studio 2017 の **ASP.NET と Web 開発**および**Azure の開発**ワークロードから、ASP.NET Web サイトを作成、管理、発行することができます。
