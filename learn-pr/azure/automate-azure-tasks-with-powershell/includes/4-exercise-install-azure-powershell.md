このユニットでは、インストール**PowerShell**ローカル コンピューターにします。

> [!NOTE]
> この演習では、PowerShell ツールをローカルにインストールする手順に従ってできます。 モジュールの残りの部分では Azure Cloud Shell を使用して、Microsoft Learn で無料のサブスクリプション サポートを利用できるようにします。 この演習は省略可能なアクティビティと考え、必要に応じて手順を確認できます。

::: zone pivot="linux"

## <a name="linux"></a>Linux

Linux 用 PowerShell をインストールすると、パッケージ マネージャーを使用する必要があります。 この例では **Ubuntu 18.04** を使用しますが、[他のバージョンやディストリビューションに関する詳細な説明についてはこちらのドキュメント](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux)をご覧ください。

Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、Ubuntu Linux 上に PowerShell Core をインストールします。 

1. Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。 これにより、インストールする PowerShell Core パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. パッケージ マネージャーで PowerShell Core パッケージを検索することができるように、**Microsoft Ubuntu リポジトリ**を登録します。

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. パッケージの一覧を更新します。

    ```bash
    sudo apt-get update
    ```

1. PowerShell Core をインストールします。

    ```bash
    sudo apt-get install -y powershell
    ```

1. PowerShell を起動して、正常にインストールされたことを確認します。

    ```bash
    pwsh
    ```
::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>MacOS

Macos でインストールには、まず**PowerShell Core**します。 これは、Homebrew パッケージ マネージャーを使用します。

> [!IMPORTANT]
> **brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。 詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。

1. Homebrew-Cask をインストールして、PowerShell Core パッケージなどの他のパッケージを取得します。

    ```bash
    brew tap caskroom/cask
    ```

1. PowerShell Core をインストールします。

    ```bash
    brew cask installs powershell
    ```

1. PowerShell Core を起動して、正常にインストールされたことを確認します。

    ```bash
    pwsh
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows
PowerShell は、ただしできる場合があります、更新プログラム、コンピューターの Windows に含まれています。 Azure のサポートを使用して、ここでは、PowerShell バージョン 5.0 以降が必要です。 次の手順を使用してインストールするバージョンを確認できます。

1. **[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。 複数のショートカット リンクを使用可能なあります。
    - Windows PowerShell - これは、64 ビット版と一般に選択する必要があります。
    - Windows PowerShell (x86)-これは、64 ビット Windows にインストールされている 32 ビット バージョンです。
    - Windows PowerShell ISE - Integrated Scripting Environment (ISE) は、PowerShell でスクリプトを作成するために使用されます。 
    - Windows PowerShell ISE (x86)-ISE の 32 ビット バージョン。

1. Windows PowerShell アイコンを選択します。

1. インストールされている PowerShell のバージョンを確認するには、次のコマンドを入力します。

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
バージョン番号が 5.0 よりも小さい場合は、次の手順を[既存の Windows PowerShell をアップグレード](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)します。

::: zone-end

## <a name="summary"></a>まとめ
PowerShell をサポートするために、ローカル マシンのセットアップがあります。 次に、Azure モジュールなどを追加するコマンドについてご説明します。