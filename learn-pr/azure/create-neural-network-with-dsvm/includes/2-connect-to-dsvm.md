### <a name="exercise-2-connect-to-the-data-science-vm"></a>演習 2: Data Science VM に接続する

この演習では、前の演習で作成した VM にある Ubuntu デスクトップにリモートで接続します。 この操作を行うには、Linux 用のライトウェイト デスクトップ環境である、[Xfce](https://xfce.org/) をサポートするクライアントが必要です。 背景について、および DSVM に接続できるさまざまな方法の概要については、「[Linux データ サイエンス仮想マシンにアクセスする方法](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro#how-to-access-the-data-science-virtual-machine-for-linux)」を参照してください。

1. まだ Xfce クライアントをインストールしていない場合は、この演習を続ける前に、[X2Go クライアント](https://wiki.x2go.org/doku.php/download:start)をダウンロードしてインストールしてください。 X2Go は無料であり、Windows や OS X などのさまざまなオペレーティング システム上で動作するオープンソースの Xfce ソリューションです。この演習の手順では X2Go を使用することを想定していますが、Xfce をサポートする任意のクライアントを使用できます。

1. Azure portal 内の "data-science-rg" リソース グループに戻ります。 "data-science-vm" リソースをクリックしてポータル内で開きます。

    ![Data Science VM を開く](../images/open-data-science-vm.png)

    "Data Science VM を開く"__

1. VM 用に示された IP アドレスの上にマウス ポインターを合わせて、表示された **[コピー]** ボタンをクリックし、IP アドレスをクリップボードにコピーします。

    ![VM の IP アドレスのコピー](../images/copy-ip-address.png)

    "VM の IP アドレスのコピー"__

1. X2Go クライアントを起動し、クリップボードの IP アドレスと前の演習で指定したユーザー名を使用して Data Science VM に接続します。 ポート **22** (SSH 接続に使用される標準ポート) 経由で接続し、セッションの種類に **[XFCE]** を指定します。 **[OK]** ボタンをクリックして、ユーザー設定を確定します。

    ![X2Go との接続](../images/new-session-1.png)

    "X2Go との接続"__

1. 右側の "新しいセッション" パネルで、リモート デスクトップに使用する解像度を選択します。 次に、パネルの上部で **[新しいセッション]** をクリックします。

    ![新しいセッションの開始](../images/new-session-2.png)

    "新しいセッションの開始"__

1. [演習 1](#Exercise1)で指定したパスワードを入力して、**[OK]** ボタンをクリックします。 ホスト キーを信頼するかどうかの確認が表示された場合は、**[はい]** を選択してください。 また、SSH デーモンが起動しないことを示すエラー メッセージは無視します。

    ![VM へのログイン](../images/new-session-3.png)

    "VM へのログイン"__

1. リモート デスクトップが表示されるのを待って、以下のように表示されていることを確認します。

    > デスクトップ上のテキストとアイコンが大きすぎる場合は、セッションを終了します。 "新しいセッション" パネルの右下隅にあるアイコンをクリックして、メニューから **[Session preferences...]\(セッションのユーザー設定\)** を選択します。 "新しいセッション" ダイアログ内の "入力/出力" タブに移動して、ディスプレイ DPI を調整し、新しいセッションを開始します。 96 DPI から始めて、必要に応じて調整します。

    ![接続完了](../images/ubuntu-desktop.png)

    _接続完了_

これで接続されました。少し時間を取ってデスクトップ上のショートカットの詳細を確認しましょう。 これらは、VM に事前にインストールされている多くのデータ サイエンス ツールへのショートカットです。これには、[Jupyter](http://jupyter.org/)、[R Studio](https://www.rstudio.com/)、[Microsoft Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) などが含まれます。