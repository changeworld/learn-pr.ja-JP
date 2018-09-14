この演習では、ローカル マシンに Eclipse をインストールし、次に Azure Toolkit をインストールし、Azure と統合された Java アプリケーションを開発する準備を整えます。 インストールはすばやくシンプルに実行できます。 この演習を終了すると、Azure の機能とサービスを利用して、最初の Java アプリケーションを起動するために必要なすべての設定が完了します。

## <a name="install-eclipse-ide"></a>Eclipse IDE をインストールする

1. http://www.eclipse.org/downloads/packages/installer からお使いのオペレーティング システムに適した Eclipse バージョンをダウンロードします。

1. ダウンロードした Eclipse インストーラーを起動します。

    1. Windows の場合は、ダウンロードしたファイルをダブルクリックします。

    1. macOS と Linux の場合は、ダウンロードしたファイルからインストーラーを展開します。 展開が完了したら、インストーラーを起動します。

        > [!NOTE]
        > Java Development Kit がインストールされていない場合、インストールするように求められることがあります。

1. インストールするパッケージを選択します。 Java 開発者の場合は、Java または Java EE Eclipse IDE オプションを選択します。

1. マシン上のインストール先を選択します。

1. Eclipse を起動して、正しくインストールされていることを確認します。

## <a name="install-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse をインストールする

Azure Toolkit のインストールは、Windows、macOS、 Linux で同じです。

1. Eclipse を起動します。

1. **[Help]\(ヘルプ\)** > **[Install New Software]\(新しいソフトウェアのインストール\)** に移動します。

    次のスクリーンショットは、**[Install New Software]\(新しいソフトウェアのインストール\)** 項目のメニューの場所を示しています。

    ![Eclipse の [Help]\(ヘルプ\) メニュー内で強調表示されている [Install New Software]\(新しいソフトウェアのインストール\) オプションのスクリーンショット](../media/7-eclipse-install-new-software.png)

1. **[Available Software]\(使用できるソフトウェア\)** ダイアログが開きます。 **[Work with:]\(操作対象:\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、Enter キーを押します。

1. 結果の **[Azure Toolkit for Java]** オプションをオンにします。 **[Contact all update sites during install to find required software]\(インストール時にすべての更新プログラム サイトに接続して必要なソフトウェアを検索する\)** オプションがまだオフではない場合はオフにします。

    次のスクリーンショットは、前述の **[Available Software]\(使用できるソフトウェア\)** のインストール構成を示しています。

    ![Eclipse の [Available Software]\(使用できるソフトウェア\) ウィンドウのスクリーンショット。Azure Toolkit for Java を検索してインストールするために必要な構成が強調表示されています。](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. **[Next]\(次へ\)** をクリックします。

1. プロンプトが表示されたらライセンス契約を確認して同意し、**[Finish]\(完了\)** をクリックします。

1. Azure Toolkit が Eclipse によってダウンロードされてインストールされます。

1. 必要に応じて Eclipse を再起動します。

1. Eclipse に **[Tools]\(ツール\)** > **[Azure]** メニュー オプションが表示されることを確認して Azure Toolkit のインストールを検証します。

## <a name="summary"></a>まとめ

このユニットでは、Eclipse for Java をインストールし、Azure のサービスと製品との統合を利用する準備をしました。 インストールはすばやく簡単に実行できるので、Eclipse は、クラウド サービスと統合された Java 開発のタスクに最適です。
