このユニットでは、お使いのローカル コンピューターに **PowerShell** をインストールします。

> [!NOTE]
> この演習では、PowerShell ツールをローカルでインストールする手順を説明します。 モジュールの残りの部分では Azure Cloud Shell を使用して、Microsoft Learn で無料のサブスクリプション サポートを利用できるようにします。 この演習は省略可能なアクティビティと考え、必要に応じて手順を確認できます。

::: zone pivot="linux"

## <a name="linux"></a>Linux

Linux 用 PowerShell のインストールでは、パッケージ マネージャーを使用します。 この例では **Ubuntu 18.04** を使用しますが、[他のバージョンやディストリビューションに関する詳細な説明についてはこちらのドキュメント](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux)をご覧ください。

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

## <a name="macos"></a>macOS

macOS では、最初に **PowerShell Core** をインストールします。 これは、Homebrew パッケージ マネージャーを使用して行われます。

> [!IMPORTANT]
> **brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。 詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。

1. Homebrew-Cask をインストールして、PowerShell Core パッケージなどの他のパッケージを取得します。

    ```bash
    brew tap caskroom/cask
    ```

1. PowerShell Core をインストールします。

    ```bash
    brew cask install powershell
    ```

1. PowerShell Core を起動して、正常にインストールされたことを確認します。

    ```bash
    pwsh
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows
PowerShell は Windows に付属していますが、お使いのコンピューターに使用可能な更新プログラムがある場合があります。 使用する予定の Azure サポートでは、PowerShell バージョン 5.0 以降が必要です。 インストールされているバージョンを確認するには、次の手順を行います。

1. **[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。 使用可能なショートカット リンクが複数ある場合があります。
    - Windows PowerShell: これは 64 ビット バージョンで、通常はこれを選択します。
    - Windows PowerShell (x86): これは 64 ビット Windows にインストールされている 32 ビット バージョンです。
    - Windows PowerShell ISE: Integrated Scripting Environment (ISE) は、PowerShell でスクリプトを記述するために使用されます。 
    - Windows PowerShell ISE (x86): ISE の 32 ビット バージョンです。

1. Windows PowerShell アイコンを選択します。

1. インストールされている PowerShell のバージョンを確認するには、次のコマンドを入力します。

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
バージョン番号が 5.0 より小さい場合は、[既存の Windows PowerShell のアップグレード](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)の手順に従います。

::: zone-end

PowerShell をサポートするように、ローカル コンピューターをセットアップしました。 次は、Azure モジュールなど、追加できるその他のコマンドについてご説明します。