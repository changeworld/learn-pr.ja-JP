Azure CLI をローカル コンピューターにインストールしてから、コマンドを実行してインストールを確認します。 Azure CLI をインストールするために使用する方法は、お使いのコンピューターのオペレーティング システムによって異なります。 お使いのオペレーティング システムの手順を選択してください。

> [!NOTE]
> この演習では、Azure CLI ツールをローカルでインストールする手順を説明します。 モジュールの残りの部分では Azure Cloud Shell を使用して、Microsoft Learn で無料のサブスクリプション サポートを利用できるようにします。 この演習は省略可能なアクティビティと考え、必要に応じて手順を確認できます。

::: zone pivot="linux"

## <a name="linux"></a>Linux

ここでは、Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、**Ubuntu Linux** 上に Azure CLI をインストールします。

> [!TIP]
> 次のコマンドは、Ubuntu バージョン 18.04 用です。 Linux のその他のバージョンやディストリビューションの手順は異なります。 別の Linux バージョンを使用する場合は、[公式ドキュメント](https://docs.microsoft.com/cli/azure/install-azure-cli)を確認してください。

1. Microsoft リポジトリが登録されるように、自分のソース リストを変更すると、パッケージ マネージャーで Azure CLI パッケージを検索できるようになります。

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。 これにより、インストールする Azure CLI パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Azure CLI をインストールします。

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>macOS

ここでは、Homebrew パッケージ マネージャーを使用して macOS に Azure CLI をインストールします。

> [!IMPORTANT]
> **brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。 詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。

1. 最新の Azure CLI パッケージを確実に入手するため、brew リポジトリを更新します。

    ```bash
    brew update
    ```

1. Azure CLI をインストールします。

    ```bash
    brew install azure-cli
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows

ここでは、MSI インストーラーを使用して Windows に Azure CLI をインストールします。

1. [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) に移動して、ブラウザーのセキュリティ ダイアログ ボックスで、**[実行]** をクリックします。
1. インストーラーで、ライセンス条項に同意し、**[インストール]** をクリックします。
1. **[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。

::: zone-end

## <a name="running-the-azure-cli"></a>Azure CLI の実行

Azure CLI を実行するには、bash シェルを開く (Linux と macOS) か、コマンド プロンプトまたは PowerShell (Windows) から実行します。

1. Azure CLI を起動し、バージョン チェックを実行して、インストールを確認します。

    ```azurecli
    az --version
    ```

::: zone pivot="windows"

> [!TIP]
> PowerShell から Azure CLI を実行することには、Windows コマンド プロンプトから Azure CLI を実行することに比べて、いくつかの利点があります。 PowerShell では、コマンド プロンプトよりも多くのタブ補完機能が提供されます。

::: zone-end

Azure CLI を使用して Azure リソースを管理できるように、ローカル コンピューターを設定しました。 Azure CLI をローカルに使用して、コマンドを入力したりスクリプトを実行したりできるようになりました。 入力したコマンドは、Azure CLI によって Azure データセンターに転送され、Azure サブスクリプションの内部で実行されます。