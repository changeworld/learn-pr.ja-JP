PowerShell は、コマンドを記述し、すぐに実行できます。 これは**対話モード**と呼ばれています。

顧客関係管理 (CRM) の例の全体的な目標は VM を含む 3 つのテスト環境を作成することであることを思い出してください。 リソース グループを利用して VM を別々の環境に整理します。単体テスト用に 1 つ、統合テスト用に 1 つ、受け入れテスト用に 1 つとなります。 リソース グループは一度だけ作成すれば十分です。そこで、PowerShell の対話モードを使用することが良い選択肢となります。

一致を PowerShell コマンドを入力すると、_コマンドレット_し、要求されたアクションを実行します。 を使用して PowerShell の Azure サポートのインストールを確認し、一般的なコマンドのいくつか見ていきます。

## <a name="what-are-powershell-cmdlets"></a>PowerShell コマンドレットとは
PowerShell コマンドは**コマンドレット**と呼ばれています。 コマンドレットは 1 つの機能を操作するコマンドです。 **コマンドレット**と用語は "小さなコマンド" を意味します。 慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。

基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。 他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。 たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。

コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。 動詞の選択にも規則があります。たとえば、データの取得は "get"、データの挿入または更新は "set"、データの書式設定は "format"、宛先への直接出力は "out" になります。

コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。 **Get-help**コマンドレットは、いずれかのコマンドレットのヘルプ ファイルを表示します。 などヘルプを取得でした、`Get-ChildItem`コマンドレットは、次のステートメントを使用します。

```powershell
Get-Help Get-ChildItem -detailed
```

## <a name="what-is-a-powershell-module"></a>PowerShell モジュールとは何ですか。

コマンドレットが付属して_モジュール_します。 PowerShell モジュールは、使用可能な各コマンドレットのプロセスにコードを含む DLL です。 内のモジュールを読み込むには、PowerShell にコマンドレットを読み込みます。 使用して読み込まれたモジュールの一覧を取得することができます、`Get-Module`コマンド。

```powershell
Get-Module
```

これは、以下のような出力します。

```output
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Find-PackageProvider, Get-Package, Get-Pack...
Script     1.0.0.1    PowerShellGet                       {Find-Command, Find-DscResource, Find-Module, Find-RoleCap...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

## <a name="what-is-azurerm"></a>AzureRM とは
**AzureRM** は、Azure 機能で使用できるコマンドレットを含む Azure PowerShell コマンドレットの正式名称です (名前の中の **RM** は **Resource Manager** という意味です)。 数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。 リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。 このモジュールは、オープン ソース コンポーネントで[GitHub で入手できます](https://github.com/Azure/azure-powershell)します。

> [!NOTE]
> Azure PowerShell モジュールは、オプションのインストール - コマンドレットは、モジュールをインポートするまで使用できません。

### <a name="install-the-azurerm-module"></a>AzureRM モジュールをインストールします。

AzureRM モジュールは、PowerShell ギャラリーと呼ばれるグローバル リポジトリから使用できます。 使ってローカル コンピューター上に、モジュールをインストールすることができます、`Install-Module`コマンド。 管理者特権で PowerShell シェルを PowerShell ギャラリーからモジュールをインストールする必要があります。 

::: zone pivot="windows"

最新の Azure PowerShell モジュールをインストールするには、次のコマンドを実行します。

1. **[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。

1. **[Windows PowerShell]** アイコンを右クリックし、**[管理者として実行]** を選択します。

1. **[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。

1. 次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Install-Module -Name AzureRM
    ```

これにより、既定で (スコープのパラメーターによって制御されます) すべてのユーザーに対してモジュールがインストールされます。 

コマンドは、コンポーネントを取得する NuGet に依存しています、インストールされている NuGet のバージョンに応じてをダウンロードして、最新バージョンの NuGet のインストールを求めるメッセージを取得する可能性があります。

```output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files (x86)\PackageManagement\ProviderAssemblies' or
'C:\Users\<username>\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
 the NuGet provider now?
```

既定では、PowerShell ギャラリーは、PowerShellGet の信頼できるリポジトリとしては構成されません。 PSGallery の初回使用時には、次のプロンプトが表示されます。

```output
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
```

#### <a name="script-execution-failed"></a>スクリプトの実行に失敗しました
セキュリティ構成によって`Import-Module`で次のような失敗する可能性があります。

```output
import-module : File C:\Program Files (x86)\WindowsPowerShell\Modules\azurerm\6.8.1\AzureRM.psm1 cannot be loaded
because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ import-module azurerm
+ ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [Import-Module], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.PowerShell.Commands.ImportModuleCommand
```

通常、このエラーは、"restricted"、つまりモジュールが PowerShell ギャラリーなどの外部ソースからダウンロードすることを実行することはできません、実行ポリシーであることを示します。 コマンドを実行して、ケースになるかどうかをチェックする`Get-ExecutionPolicy`します。 "Restricted"が返された場合、次を方法します。

1. 管理者特権の PowerShell コマンド プロンプトを開きます。
1. 使用して、 `SetExecutionPolicy` "RemoteSigned"にポリシーを変更するコマンドレット。

```powershell
Set-ExecutionPolicy RemoteSigned
```

これで、アクセス許可が要求されます。

```output
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

使用できる必要がありますし、`Import-Module`コマンドレットを読み込めません。

ゾーン終了

ゾーン ピボット"linux、macos"を =

私たちは、同じコマンドを使用して、Linux または macOS で Azure PowerShell をインストールします。

1. ターミナルで次のコマンドを入力し、管理者特権で PowerShell Core を起動します。

    ```bash
    sudo pwsh
    ```

1. PowerShell Core プロンプトで次のコマンドを実行して、Azure PowerShell をインストールします。

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. **PSGallery** からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** を選択します。

ゾーン終了

### <a name="update-a-module"></a>モジュールを更新します。

警告または Azure PowerShell モジュールのバージョンが既にインストールされていることを示すエラー メッセージを取得する場合を更新することができます、_最新_コマンドを実行してバージョン。

ゾーン pivot ="windows"

```powershell
Update-Module -Name AzureRM
```

ゾーン終了

ゾーン ピボット"linux、macos"を =

```powershell
Update-Module -Name AzureRM.NetCore
```

ゾーン終了

`Install-Module` コマンドと同じように、モジュールの信頼の確認を求められたら **[はい]** または **[すべてはい]** を選択します。 使用することも、`Update-Module`をそれに問題がある場合、モジュールを再インストールするコマンド。

## <a name="example-how-to-create-a-resource-group-with-azure-powershell"></a>例: Azure PowerShell を使用したリソース グループを作成する方法
Azure モジュールをロードしたら、Azure との連携を開始することができます。 一般的なタスク - リソース グループを作成してみましょう。 ご存知のように、関連リソースをまとめて管理するのにリソース グループを使用します。 新しいリソース グループを作成すると、新しい Azure ソリューションを開始するときに行います最初のタスクの 1 つです。

次の 4 つの手順を実行する必要がありますがあります。

1. Azure コマンドレットをインポートします。

1. Azure サブスクリプションに接続します。

1. リソース グループを作成します。

1. 正常に作成できたことを確認します (下記参照)。

次の図では、この手順の概要を示します。

![リソース グループを作成する手順を示す図。](../media/5-create-resource-overview.png)

各手順は異なるコマンドレットに対応しています。

### <a name="import-the-azure-cmdlets"></a>Azure のコマンドレットをインポートします。
起動時、PowerShell は既定で中心的なコマンドレットのみを読み込みます。 つまり、Azure で使用する必要があるコマンドレットは読み込まれません。 必要なコマンドレットを最も確実に読み込む方法は、PowerShell セッションの開始時にコマンドレットを手動でインポートすることです。

モジュールの読み込みには **Import-Module** コマンドレットを使用します。 このコマンドレットには、さまざまな状況に対処するためのパラメーターがたくさんあります。 たとえば、複数のモジュールを読み込んだり、特定のモジュール バージョンを読み込んだり、モジュールの一部を読み込んだりできます。

次のコマンドで AzureRM のすべてのコマンドレットを読み込むたとえば、**管理者特権の PowerShell セッションで**:

ゾーン pivot ="windows"

```powershell
Import-Module AzureRM
```

ゾーン終了

ゾーン ピボット"linux、macos"を =

```powershell
Import-Module AzureRM.NetCore
```

ゾーン終了

> [!TIP]
> Azure PowerShell を頻繁に使用するようであれば、2 とおりの方法でモジュール読み込みプロセスを自動化できます。 PowerShell プロファイルにエントリを追加し、起動時に Azure モジュールをインポートするか、PowerShell の最新版を使用し、コマンドレットの使用時にコマンドレットを含むモジュールを自動的に読み込むことができます。

### <a name="connect"></a>接続
Azure PowerShell のローカル インストールを使用する場合は、Azure コマンドを実行する前に認証する必要があります。 **Connect-AzureRmAccount** コマンドレットを実行すると、Azure 資格情報が求められ、Azure サブスクリプションに接続されます。 さまざまなオプション パラメーターがありますが、対話的プロンプトだけが必要であれば、パラメーターは必要ありません。

```powershell
Connect-AzureRmAccount
```

このモジュールは、コア セットの一部ではないために、開始するすべての新しい PowerShell セッションにこれらの手順を繰り返す必要があります。


### <a name="working-with-subscriptions"></a>サブスクリプションの使用
Azure に慣れていない場合をおそらく 1 つのサブスクリプションがあるだけです。 一方、しばらく前から Azure を使用している場合は、複数の Azure サブスクリプションを作成済みであることも考えられます。 その場合は、特定のサブスクリプションに対してコマンドを実行するように Azure PowerShell を構成することができます。

一度に 1 つのサブスクリプションでのみできます。 使用して、`Get-AzureRmContext`コマンドレットをどのサブスクリプションがアクティブかを判断します。 しいものではない場合は、変更を適用することができます。

1. 使用してアカウントのすべてのサブスクリプション名の一覧を取得、`Get-AzureRmSubscription`コマンド。 

2. 選択する 1 つの名前を渡すことによって、サブスクリプションを変更します。

```powershell
Select-AzureRmSubscription -Subscription "Visual Studio Enterprise"
```

### <a name="get-a-list-of-all-resource-groups"></a>すべてのリソース グループの一覧を取得します。

アクティブなサブスクリプション内のすべてのリソース グループの一覧を取得できます。

```powershell
Get-AzureRmResourceGroup
```

簡潔なビューを取得するからの出力を送信することができます、`Get-AzureRmResourceGroup`を`Format-Table`コマンドレットにパイプを使用して ' |'。

```powershell
Get-AzureRmResourceGroup | Format-Table
```

これは、以下のような出力します。

```output
ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
-----------------                  --------       ----------------- ---- --------- ----------
cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
ExerciseResources                  eastus         Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
```

### <a name="create-a-resource-group"></a>リソース グループを作成します

Azure では、リソースを作成する場合、ご存知のように管理目的でリソース グループに配置するが常に。 リソース グループは、多くの場合は、まず、新しいアプリケーションを開始するときに作成のいずれかです。

持つリソース グループを作成することができます、`New-AzureRmResourceGroup`コマンドレット。 名前と場所を指定する必要があります。 この名前はサブスクリプション内で一意である必要があります。 この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます (コンプライアンス上の理由から重要となる場合があります)。 "West US"、"North Europe"、"West India" などの文字列を使用して場所を指定します。 Azure のコマンドレットのほとんどと同様に`New-AzureRmResourceGroup`は多くの省略可能なパラメーターがあります。 ただし、でも core 構文は。

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

> [!NOTE]
> リソース グループを作成する Azure のサンド ボックス内で取り組む予定に注意してください。 上記のコマンドは、自分のサブスクリプションで作業する場合に使用されます。

### <a name="verify-the-resources"></a>リソースを確認します
`Get-AzureRmResource` Azure リソースを一覧表示されます。 リソース グループが正常に作成されたことを確認するためにここで役に立ちます。

```powershell
Get-AzureRmResource
```

ように、`Get-AzureRmResourceGroup`コマンドをより簡潔なビュー取得できます、`Format-Table`コマンドレット。 ここでは、短縮形バージョンを使用して、 `ft`:

```powershell
Get-AzureRmResource | ft
```

そのグループに関連付けられているリソースのみを一覧表示する特定のリソース グループにフィルターできます。

```powershell
Get-AzureRmResource -ResourceGroup ExerciseResources
```

### <a name="creating-an-azure-virtual-machine"></a>Azure の仮想マシンを作成します。

PowerShell で行うことがもう 1 つの一般的なタスクでは、Vm を作成します。

Azure PowerShell では、`New-AzureRmVm`コマンドレットで仮想マシンを作成します。 コマンドレットには多くのパラメーターがあり、それを使用して数多くの VM 構成設定を処理することができます。 ほとんどのパラメーターに適切な既定値があるため、指定する必要があるのは次の 5 つのみです。

- **ResourceGroupName**: 新しい VM が配置されるリソース グループ。
- **Name**: Azure での VM の名前。
- **Location**: VM がプロビジョニングされる地理的な場所。
- **Credential**: VM 管理者アカウント用のユーザー名とパスワードが含まれているオブジェクト。 使用して、`Get-Credential`コマンドレット。 このコマンドレットはユーザー名とパスワードを確認し、資格情報オブジェクトにパッケージ化します。
- **イメージ**: VM に使用するオペレーティング システム イメージ。 これは多くの場合、Linux ディストリビューション、または Windows Server です。

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

上記のように、これらのパラメーターをコマンドレットに直接を指定できます。 他のコマンドレットを使用してなど、仮想マシンを構成する代わりに、 `Set-AzureRmVMOperatingSystem`、 `Set-AzureRmVMSourceImage`、 `Add-AzureRmVMNetworkInterface`、および`Set-AzureRmVMOSDisk`します。

文字列の例を次に示します、`Get-Credential`コマンドレットと共に、`-Credential`パラメーター。

```powershell
New-AzureRmVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

`AzureRmVM`サフィックスは、PowerShell で VM ベースのコマンドを特定します。 使用できるその他いくつかあります。

| コマンド | 説明 |
|---------|-------------|
| `Remove-AzureRmVM` | Azure VM を削除します。 |
| `Start-AzureRmVM` | 停止した VM を起動します。 |
| `Stop-AzureRmVM` | 実行中の VM を停止します。 |
| `Restart-AzureRmVM` | VM を再起動します。 |
| `Update-AzureRmVM` | VM の構成を更新します。 |

#### <a name="example-getting-the-information-for-a-vm"></a>例: VM の情報を取得します。

サブスクリプション内の Vm を一覧表示、`Get-AzureRmVM -Status`コマンド。 これを使用して VM を指定することも、`-Name`プロパティ。 ここで、PowerShell 変数に割り当てます。

```powershell
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName ExerciseResources
```

これは興味深いは、_オブジェクト_と対話できます。 たとえば、そのオブジェクトを取得、変更を加えるして Azure にバックアップの変更をプッシュ、、`Update-AzureRmVM`コマンド。

```powershell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzureRmVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

## <a name="summary"></a>まとめ
PowerShell の対話モードは 1 回限りのタスクに適しています。 例では、可能性があります、同じリソース グループ、つまり妥当な対話形式で作成することは、プロジェクトの有効期間にわたって使用します。 このタスクでは多くの場合、スクリプトを記述してそのスクリプトを正確に 1 回実行するより、対話モードの方が簡単で速くなります。