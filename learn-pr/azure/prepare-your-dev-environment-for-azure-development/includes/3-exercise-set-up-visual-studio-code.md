Azure 開発に Visual Studio Code を使用するには、Visual Studio Code と 1 つまたは複数の Azure 拡張機能をローカルにインストールする必要があります。 この演習では、**Azure App Service** 拡張機能を追加します。

## <a name="install-visual-studio-code"></a>Visual Studio Code のインストール

::: zone pivot="windows"

### <a name="windows"></a>Windows

1. [Windows 用 Visual Studio Code インストーラーをダウンロードします](https://code.visualstudio.com/)。

1. インストーラーを実行します。

1. Windows キーを押すか、タスク バーの Windows アイコンをクリックし、「Visual Studio Code」と入力し、結果の **Visual Studio Code** をクリックして Visual Studio Code を開きます。

::: zone-end

::: zone pivot="macos"

### <a name="macos"></a>macOS

1. [macOS 用 Visual Studio Code をダウンロードします](https://code.visualstudio.com/)。

1. ダウンロードしたアーカイブをダブルクリックして内容を展開します。

1. Visual Studio Code.app を Applications フォルダーにドラッグします。

1. Apps セクションのアイコンをクリックするか、Spotlight で Visual Studio Code を検索して Visual Studio Code を開きます。

::: zone-end

::: zone pivot="linux"

### <a name="linux"></a>Linux 

#### <a name="debian-and-ubuntu"></a>Debian と Ubuntu

1. グラフィカル ソフトウェア センター (使用できる場合) またはコマンド ラインを使用して、[.deb package (64 ビット)](https://go.microsoft.com/fwlink/?LinkID=760868) をダウンロードしてインストールします (`<file>` はダウンロードした .deb ファイル名で置き換えます)。

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

#### <a name="rhel-fedora-and-centos"></a>RHEL、Fedora、および CentOS

1. 次のスクリプトを使用して、キーとリポジトリをインストールします。

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. パッケージ キャッシュを更新し、dnf (Fedora 22 以降) を使用してパッケージをインストールします。

    ```bash
    dnf check-update
    sudo dnf install code
    ```

#### <a name="opensuse-and-sle"></a>openSUSE と SLE

1. yum リポジトリは、openSUSE と SLE ベースのシステムでも機能します。 次のスクリプトで、キーとリポジトリをインストールします。

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. パッケージ キャッシュを更新し、以下を使用してパッケージをインストールします。

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> さまざまな Linux ディストリビューションで Visual Studio Code をインストールまたは更新する方法の詳細については、[Linux 上で Visual Studio Code を実行する方法に関するドキュメント](https://code.visualstudio.com/docs/setup/linux)を参照してください。

::: zone-end

## <a name="install-azure-app-service-extension"></a>Azure App Service 拡張機能をインストールする

1. Visual Studio Code を開いていない場合は開きます。

1. 拡張機能ブラウザーを開きます (左側のメニューからアクセスできます)。

1. **Azure App Service** を検索します。

1. 結果の **Azure App Service** を選択し、**[インストール]** をクリックします。

    次のスクリーン ショットは、Visual Studio Code 拡張機能の検索結果から選択した Azure App Service 拡張機能を示しています。

    ![[拡張機能] タブが表示され、検索結果で Azure App Service 拡張機能が強調表示されている Visual Studio Code のスクリーンショット。](../media/3-install-azure-extension.png)

これで、拡張機能がインストールされます。 Azure サブスクリプションに接続し、Web、モバイル、または API アプリを Azure App Service にデプロイする準備が整いました。
