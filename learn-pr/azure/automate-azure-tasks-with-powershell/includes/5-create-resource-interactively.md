PowerShell では、コマンドを記述し、すぐに実行できます。 これは**対話モード**と呼ばれています。

顧客関係管理 (CRM) の例の全体的な目標は VM を含む 3 つのテスト環境を作成することであることを思い出してください。 リソース グループを利用して VM を別々の環境に整理します。単体テスト用に 1 つ、統合テスト用に 1 つ、受け入れテスト用に 1 つとなります。 リソース グループは一度だけ作成すれば十分です。その場合、PowerShell の対話モードを使用することをお勧めします。

PowerShell にコマンドを入力すると、_コマンドレット_と照合され、その後、要求されたアクションが実行されます。 使用できる一般的なコマンドをいくつか見てから、PowerShell 用の Azure サポートのインストールについて確認します。

## <a name="what-are-powershell-cmdlets"></a>PowerShell コマンドレットとは
PowerShell コマンドは**コマンドレット**と呼ばれています。 コマンドレットは 1 つの機能を操作するコマンドです。 **コマンドレット**と用語は "小さなコマンド" を意味します。 慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。

基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。 他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。 たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。

コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。 動詞の選択にも規則があります。たとえば、データの取得は "get"、データの挿入または更新は "set"、データの書式設定は "format"、宛先への直接出力は "out" になります。

コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。 **Get-Help** コマンドレットを実行すると、コマンドレットのヘルプ ファイルが表示されます。 たとえば、次のステートメントで `Get-ChildItem` コマンドレットに関するヘルプを取得することができます。

```powershell
Get-Help Get-ChildItem -detailed
```

## <a name="what-is-a-powershell-module"></a>PowerShell モジュールとは

コマンドレットは_モジュール_に付属しています。 PowerShell モジュールは、利用可能な各コマンドレットを処理するコードを含む DLL です。 PowerShell にコマンドレットを読み込む場合は、そのコマンドレットが含まれるモジュールを読み込みます。 `Get-Module` コマンドを使用して、読み込まれたモジュールのリストを取得することができます。

```powershell
Get-Module
```

その出力は次のようになります。

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
**AzureRM** は、Azure 機能で使用できるコマンドレットを含む Azure PowerShell コマンドレットの正式名称です (名前の中の **RM** は **Resource Manager** という意味です)。 数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。 リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。 このモジュールは、[GitHub で入手できる](https://github.com/Azure/azure-powershell)オープン ソースのコンポーネントです。

> [!NOTE]
> Azure PowerShell モジュールのインストールは任意です。モジュールをインポートするまで、コマンドレットは使用できません。

### <a name="install-the-azurerm-module"></a>AzureRM モジュールをインストールする

AzureRM モジュールは、PowerShell ギャラリーと呼ばれるグローバル リポジトリから入手できます。 `Install-Module` コマンドを使用して、ローカル コンピューター上にモジュールをインストールすることができます。 PowerShell ギャラリーからモジュールをインストールするには、管理者特権の PowerShell シェルが必要です。 

::: zone pivot="windows"

最新の Azure PowerShell モジュールをインストールするには、次のコマンドを実行します。

1. **[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。

1. **[Windows PowerShell]** アイコンを右クリックし、**[管理者として実行]** を選択します。

1. **[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。

1. 次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Install-Module -Name AzureRM
    ```

これにより、既定ですべてのユーザーに対してモジュールがインストールされます (スコープ パラメーターで制御)。 

コマンドでは NuGet を利用してコンポーネントを取得します。インストールした NuGet のバージョンによっては、最新バージョンの NuGet をダウンロードしてインストールするように求められる場合があります。

```output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files (x86)\PackageManagement\ProviderAssemblies' or
'C:\Users\<username>\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
 the NuGet provider now?
```

既定では、PowerShell ギャラリーは、PowerShellGet 用の信頼できるリポジトリとしては構成されません。 PSGallery の初回使用時には、次のプロンプトが表示されます。

```output
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
```

#### <a name="script-execution-failed"></a>スクリプトの実行に失敗しました
セキュリティの構成によっては、`Import-Module` が失敗し、次のように表示される場合があります。

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

これは通常、実行ポリシーが "restricted" であることを示しており、PowerShell ギャラリーを含む、外部ソースからダウンロードするモジュールを実行できないことを意味します。 これに該当するかどうかは、`Get-ExecutionPolicy` というコマンドを実行することで確認できます。 "Restricted" が返された場合は、次のようにします。

1. 管理者特権の PowerShell コマンド プロンプトを開きます。
1. `SetExecutionPolicy` コマンドレットを使用して、ポリシーを "RemoteSigned" に変更します。

```powershell
Set-ExecutionPolicy RemoteSigned
```

これにより、アクセス許可が求められます。

```output
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

その後、`Import-Module` を使用して、コマンドレットを読み込むことができます。

:::zone-end

::: zone pivot="linux,macos"

同じコマンドを使用して、Linux または macOS に Azure PowerShell をインストールします。

1. ターミナルで次のコマンドを入力し、管理者特権で PowerShell Core を起動します。

    ```bash
    sudo pwsh
    ```

1. PowerShell Core プロンプトで次のコマンドを実行して、Azure PowerShell をインストールします。

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. **PSGallery** からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** で答えます。

:::zone-end

### <a name="update-a-module"></a>モジュールを更新する

Azure PowerShell モジュールの何らかのバージョンが既にインストールされていることを示す警告またはエラー メッセージが表示された場合は、次のコマンドを実行して、_最新_のバージョンに更新できます。

:::zone pivot="windows"

```powershell
Update-Module -Name AzureRM
```

:::zone-end

::: zone pivot="linux,macos"

```powershell
Update-Module -Name AzureRM.NetCore
```

:::zone-end

`Install-Module` コマンドと同じように、モジュールの信頼の確認を求められたら **[はい]** または **[すべてはい]** で答えます。 問題がある場合は、`Update-Module` コマンドを使用してモジュールを再インストールすることもできます。

## <a name="example-how-to-create-a-resource-group-with-azure-powershell"></a>例: Azure PowerShell を使用してリソース グループを作成する方法
Azure モジュールを読み込んだら、Azure の操作を開始できます。 ここでは一般的なタスク (リソース グループの作成) を行います。 ご存知のように、関連リソースをまとめて管理するにはリソース グループを使用します。 新しいリソース グループの作成は、新しい Azure ソリューションを開始するときに行う最初のタスクの 1 つです。

行う必要がある手順は次の 4 つです。

1. Azure コマンドレットをインポートします。

1. Azure サブスクリプションに接続します。

1. リソース グループを作成します。

1. 正常に作成できたことを確認します (下記参照)。

次の図では、この手順の概要を示します。

![リソース グループを作成する手順を示す図。](../media/5-create-resource-overview.png)

各手順は異なるコマンドレットに対応しています。

### <a name="import-the-azure-cmdlets"></a>Azure コマンドレットをインポートする
PowerShell では、起動時に中心的なコマンドレットのみが既定で読み込まれます。 つまり、Azure で使用する必要があるコマンドレットは読み込まれません。 必要なコマンドレットを最も確実に読み込む方法は、PowerShell セッションの開始時にコマンドレットを手動でインポートすることです。

モジュールの読み込みには **Import-Module** コマンドレットを使用します。 このコマンドレットには、さまざまな状況に対処するためのパラメーターがたくさんあります。 たとえば、複数のモジュール、特定のバージョンのモジュール、モジュールの一部などを読み込むことができます。

たとえば、**管理者特権の PowerShell セッションで**は、次のコマンドで AzureRM 用のすべてのコマンドレットを読み込むことができます。

:::zone pivot="windows"

```powershell
Import-Module AzureRM
```

:::zone-end

::: zone pivot="linux,macos"

```powershell
Import-Module AzureRM.NetCore
```

:::zone-end

> [!TIP]
> Azure PowerShell を頻繁に操作するようであれば、2 とおりの方法でモジュール読み込みプロセスを自動化できます。 PowerShell プロファイルにエントリを追加し、起動時に Azure モジュールをインポートするか、PowerShell の最新版を使用し、コマンドレットの使用時にコマンドレットを含むモジュールを自動的に読み込むことができます。

### <a name="connect"></a>接続する
Azure PowerShell のローカル インストールを使用している場合、Azure コマンドを実行する前に認証する必要があります。 **Connect-AzureRmAccount** コマンドレットを実行すると、Azure 資格情報が求められ、その後、Azure サブスクリプションに接続されます。 さまざまなオプション パラメーターがありますが、対話型プロンプトのみが必要な場合、パラメーターは必要ありません。

```powershell
Connect-AzureRmAccount
```

このモジュールはコア セットの一部ではないため、新しい PowerShell セッションを開始するたびに、この手順を繰り返す必要があります。


### <a name="working-with-subscriptions"></a>サブスクリプションの使用
Azure の利用が初めての場合、所有しているサブスクリプションは 1 つのみと思われます。 しかし、Azure をしばらく利用している場合は、複数の Azure サブスクリプションを作成した可能性があります。 特定のサブスクリプションに対してコマンドを実行するように Azure PowerShell を構成することができます。

これは一度に 1 つのサブスクリプションでのみ行うことができます。 アクティブなサブスクリプションを判別するには、`Get-AzureRmContext` コマンドレットを使用します。 これが適切でない場合は、変更できます。

1. `Get-AzureRmSubscription` コマンドで、アカウントのすべてのサブスクリプション名のリストを取得します。 

2. 選択するサブスクリプションの名前を渡して、そのサブスクリプションを変更します。

```powershell
Select-AzureRmSubscription -Subscription "Visual Studio Enterprise"
```

### <a name="get-a-list-of-all-resource-groups"></a>すべてのリソース グループのリストを取得する

アクティブなサブスクリプションのすべてのリソース グループのリストを取得できます。

```powershell
Get-AzureRmResourceGroup
```

より簡潔なリストを表示するには、パイプ '|' を使用して `Get-AzureRmResourceGroup` から `Format-Table` コマンドレットに出力を送信できます。

```powershell
Get-AzureRmResourceGroup | Format-Table
```

その出力は次のようになります。

```output
ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
-----------------                  --------       ----------------- ---- --------- ----------
cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
ExerciseResources                  eastus         Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
```

### <a name="create-a-resource-group"></a>リソース グループを作成する

ご存知のように、Azure でリソースを作成する場合は、常に管理目的でリソース グループにリソースを配置します。 リソース グループは、多くの場合、新しいアプリケーションを開始するときに作成する最初のものの 1 つです。

`New-AzureRmResourceGroup` コマンドレットを使用して、リソース グループを作成することができます。 名前と場所を指定する必要があります。 この名前はサブスクリプション内で一意である必要があります。 この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます (コンプライアンス上の理由から重要となる場合があります)。 "West US"、"North Europe"、"West India" などの文字列を使用して場所を指定します。 ほとんどの Azure コマンドレットと同様に、`New-AzureRmResourceGroup` には多くのオプション パラメーターがありますが、中心的な構文は次のようになります。

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

> [!NOTE]
> リソース グループが自動的に作成される Azure サンドボックスで作業を行うことに注意してください。 上記のコマンドは、ご自身のサブスクリプションで作業を行う場合に使用されます。

### <a name="verify-the-resources"></a>リソースを確認する
`Get-AzureRmResource` では Azure リソースがリストされます。 リソース グループが正常に作成されたことを確認するためにここで役に立ちます。

```powershell
Get-AzureRmResource
```

`Get-AzureRmResourceGroup` コマンドと同様に、`Format-Table` コマンドレットを使用してより簡潔なリストを表示できます。 ここでは、短縮版の `ft` を使用します。

```powershell
Get-AzureRmResource | ft
```

これで特定のリソース グループに絞り込み、そのグループに関連付けられているリソースのみをリストすることもできます。

```powershell
Get-AzureRmResource -ResourceGroup ExerciseResources
```

### <a name="creating-an-azure-virtual-machine"></a>Azure 仮想マシンの作成

PowerShell で行うことができる一般的なタスクには、VM の作成もあります。

Azure PowerShell では、仮想マシンを作成するための `New-AzureRmVm` コマンドレットが提供されます。 コマンドレットには多くのパラメーターがあり、それを使用して数多くの VM 構成設定を処理することができます。 ほとんどのパラメーターに適切な既定値があるため、指定する必要があるのは次の 5 つのみです。

- **ResourceGroupName**: 新しい VM が配置されるリソース グループ。
- **Name**: Azure での VM の名前。
- **Location**: VM がプロビジョニングされる地理的な場所。
- **Credential**: VM 管理者アカウント用のユーザー名とパスワードが含まれているオブジェクト。 ここでは `Get-Credential` コマンドレットを使用します。 このコマンドレットでは、ユーザー名とパスワードが求められ、それが資格情報オブジェクトにパッケージ化されます。
- **Image**: VM で使用するオペレーティング システム イメージです。 これは多くの場合、Linux ディストリビューション、または Windows Server となります。

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

上記のように、これらのパラメーターをコマンドレットに直接指定することができます。 あるいは、`Set-AzureRmVMOperatingSystem`、`Set-AzureRmVMSourceImage`、`Add-AzureRmVMNetworkInterface`、`Set-AzureRmVMOSDisk` などの他のコマンドレットを使用して、仮想マシンを構成することもできます。

`Get-Credential` コマンドレットと `-Credential` パラメーターを一緒に使用する例を以下に示します。

```powershell
New-AzureRmVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

`AzureRmVM` サフィックスは、PowerShell の VM ベースのコマンドに固有のものです。 使用できるものは他にもいくつかあります。

| コマンド | 説明 |
|---------|-------------|
| `Remove-AzureRmVM` | Azure VM が削除されます。 |
| `Start-AzureRmVM` | 停止した VM を起動します。 |
| `Stop-AzureRmVM` | 実行中の VM を停止します。 |
| `Restart-AzureRmVM` | VM を再起動します。 |
| `Update-AzureRmVM` | VM の構成が更新されます。 |

#### <a name="example-getting-the-information-for-a-vm"></a>例: VM の情報の取得

`Get-AzureRmVM -Status` コマンドを使用して、サブスクリプション内の VM をリストすることができます。 `-Name` プロパティで VM を指定することもできます。 ここでは、PowerShell 変数に割り当てます。

```powershell
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName ExerciseResources
```

興味深いことは、これがやりとりできる_オブジェクト_であることです。 たとえば、そのオブジェクトを取得し、変更を加えてから、`Update-AzureRmVM` コマンドを使用してその変更を Azure にプッシュして戻すことができます。

```powershell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzureRmVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

PowerShell の対話モードは 1 回限りのタスクに適しています。 この例では、プロジェクトのライフタイムに対して同じリソース グループを使用することが考えられます。つまり、対話形式で作成するのが妥当です。 このタスクでは多くの場合、スクリプトを記述し、そのスクリプトを 1 回だけ実行するよりも、対話モードの方が早くて簡単です。