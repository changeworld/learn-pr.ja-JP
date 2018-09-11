この演習では、ローカル コンピューターに **Azure PowerShell** をインストールします。 お使いのオペレーティング システムに該当するセクションを選択してください。

## <a name="linux-and-mac"></a>Linux および Mac
Linux と macOS では、最初に **PowerShell Core** をインストールします。

### <a name="linux"></a>Linux
最後のユニットで説明されているように、Linux 用 PowerShell のインストールではパッケージ マネージャーを使用します。 この例では **Ubuntu 18.04** を使用しますが、[他のバージョンやディストリビューションに関する詳細な説明についてはこちらのドキュメント](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux)をご覧ください。

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

### <a name="macos"></a>macOS
次に、Homebrew パッケージ マネージャーを使用して macOS に **PowerShell Core** をインストールします。

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

## <a name="install-azure-powershell"></a>Azure PowerShell をインストールする
ベース **PowerShell** 製品をインストールした後、**Azure PowerShell** をインストールして Azure 固有のコマンドを追加します。

### <a name="windows"></a>Windows
`Install-Module` PowerShell コマンドを使用して、Windows に Azure PowerShell をインストールします。

> [!IMPORTANT]
> Azure PowerShell をインストールするには、PowerShell バージョン 5.0 以降が必要です。 PowerShell のバージョンを確認するには、次のコマンドを使用します。 
>
> `$PSVersionTable.PSVersion` 
>
>バージョン番号が 5.0 より小さい場合は、[既存の Windows PowerShell をアップグレードする](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)手順に従ってください。

1. **[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。
2. **[Windows PowerShell]** アイコンを右クリックし、**[管理者として実行]** を選択します。
3. **[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。
4. 次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Install-Module -Name AzureRM
    ```
5. PSGallery からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** を選択します。

> [!NOTE]
> Azure PowerShell モジュールの何らかのバージョンが既にインストールされていることを示すエラー メッセージが表示された場合は、次のコマンドを実行して "_最新_" のバージョンに更新できます。
> 
> `Update-Module -Name AzureRM`
> 
> `Install-Module` コマンドと同じように、モジュールの信頼の確認を求められたら **[はい]** または **[すべてはい]** を選択します。

### <a name="linux-or-macos"></a>Linux または macOS
同じ基本プロセスを使用して、Linux または macOS に Azure PowerShell をインストールします。 手順はどちらのオペレーティング システムでも同じです。 違っているのは、PowerShell Core セッションを管理者特権にする点です。

1. ターミナルで次のコマンドを入力し、管理者特権で PowerShell Core を起動します。

    ```bash
    sudo pwsh
    ```

1. PowerShell Core プロンプトで次のコマンドを実行して、Azure PowerShell をインストールします。

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. **PSGallery** からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** を選択します。

## <a name="summary"></a>まとめ
Azure PowerShell を使用して Azure のリソースを管理できるようにローカル コンピューターをセットアップしました。 Azure PowerShell をローカルで使用して、コマンドを入力したりスクリプトを実行したりできるようになりました。 入力したコマンドは、Azure PowerShell によって Azure データセンターに転送され、Azure サブスクリプションの内部で実行されます。
