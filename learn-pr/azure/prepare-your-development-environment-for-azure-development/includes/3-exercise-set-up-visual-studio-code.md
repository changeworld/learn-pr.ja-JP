この演習では、Visual Studio Code と Azure App Service 拡張機能をインストールします。これで、Microsoft Azure 向けに Web アプリを開発し、デプロイする準備が整います。

## <a name="exercise-steps"></a>演習の手順

まず、使用しているオペレーティング システムを特定し、以下の適切なセクションの手順に従って Visual Studio Code をインストールします。

### <a name="windows"></a>Windows

1. Windows 用 Visual Studio Code インストーラーをダウンロードします。
2. インストーラーを実行します。 この操作に長い時間はかかりません。
3. インストール フォルダーに移動して VS Code を開きます (64 ビット マシンの場合、既定のパスは C:\Program Files\Microsoft VS Code です)。

### <a name="macos"></a>macOS

1. macOS 用 Visual Studio Code をダウンロードします。
2. ダウンロードしたアーカイブをダブルクリックして内容を展開します。
3. Visual Studio Code.app を Applications フォルダーにドラッグし、Launchpad で使用できるようにします。
4. アイコンを右クリックし、[オプション] > [Dock に追加] の順に選択し、Dock に VS Code を追加します。

### <a name="linux--debian-and-ubuntu"></a>Linux - Debian と Ubuntu

1. グラフィカル ソフトウェア センター (使用できる場合) またはコマンド ラインを使用して [.deb package (64 ビット)](https://go.microsoft.com/fwlink/?LinkID=760868) をダウンロードしてインストールします (`<file>` はダウンロードした .deb ファイル名で置き換えます)。

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

### <a name="linux--rhel-fedora-and-centos"></a>Linux - RHEL、Fedora、CentOS

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

### <a name="linux--opensuse-and-sle"></a>Linux - openSUSE と SLE

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
> さまざまな Linux ディストリビューションで VS Code をインストールまたは更新する方法の詳細については、[Linux 上で VS Code を実行する方法に関するドキュメント](https://code.visualstudio.com/docs/setup/linux)を参照してください。

## <a name="install-azure-app-service-extension"></a>Azure App Service 拡張機能をインストールする

VS Code のインストールが完了したら開きます。

1. [拡張機能] タブに移動します。
2. Azure App Service を検索します。
3. [インストール] をクリックします。

    次のスクリーン ショットは、Visual Studio Code 拡張機能の検索結果から選択した Azure App Service 拡張機能を示しています。

    ![[拡張機能] タブが表示され、検索結果で Azure App Service 拡張機能が強調表示されている VS Code のスクリーンショット。](../media/3-install-azure-extension.png)

これで、拡張機能がインストールされます。 Azure サブスクリプションに接続し、Web、モバイル、または API アプリを開発して Azure App Service にデプロイする準備が整います。
