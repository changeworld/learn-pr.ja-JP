管理スクリプトを作成することは、ワークフローを最適化する強力な方法です。 共通の反復タスクを自動化することができます。 スクリプトを確認すると、実行されます、一貫したエラーが減ります可能性があります。 前の演習で、データ ディスクを追加、および、すべて Azure portal 経由のキャッシュ設定を変更 VM を作成します。 What-if を多くのリージョンで、多数の Vm でこれらのタスクを繰り返す必要でしょうか。 Azure PowerShell を使用してみましょう。

## <a name="what-is-azure-powershell"></a>Azure PowerShell とは

Azure PowerShell は、Azure サブスクリプションに接続してリソースを管理できるようにする Windows PowerShell に追加するモジュールです。 Azure PowerShell では、関数を Windows PowerShell が必要です。 Windows PowerShell では、シェル ウィンドウで、コマンドの解析、およびなどのようなサービスを提供します。 Azure PowerShell で、Azure 固有のコマンドが追加されます。

たとえば、Azure PowerShell には、 **New-azurermvm** Azure サブスクリプション内で仮想マシンを作成するコマンド。 これを使用するには、PowerShell アプリケーションを起動し、この例のように、コマンドを発行するは。

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell は使用可能な 2 つの方法: Linux、Mac、または Windows 上のローカル インストールと Azure Cloud Shell を使用して、ブラウザー内で。

## <a name="what-are-powershell-cmdlets"></a>PowerShell コマンドレットとは

PowerShell コマンドは**コマンドレット**と呼ばれています。 コマンドレットは 1 つの機能を操作するコマンドです。 用語**コマンドレット**「小さなコマンド」を意味するためのものでは、。 慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。

基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。 他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。 たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。

コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。 動詞の選択肢の規則もあります。"get"にデータを取得、挿入または"out"形式のデータを書式設定、データを更新するには、"set"を変換先に直接出力します。

コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。 **Get-Help** コマンドレットを実行すると、コマンドレットのヘルプ ファイルが表示されます。

```powershell
Get-Help <cmdlet-name> -detailed
```

## <a name="what-is-azurerm"></a>AzureRM とは

**AzureRM**を Azure の機能を使用するコマンドレットのセットを持つ Azure PowerShell モジュールの正式な名前を指定 (、 **RM**名には**Resource Manager**)。 数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。 リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。

## <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Azure ディスク キャッシュを管理するための PowerShell コマンドレット

Azure PowerShell には、Vm とディスクを管理するために特定のコマンドレットがあります。

次の表に、一部のコマンドレットでは、次の手順を使用します。

|コマンド  |説明  |
|---------|---------|
|`Get-AzureRmVM`     |  仮想マシンのプロパティを取得します。       |
|`Update-AzureRmVM`     |  Azure の仮想マシンの状態を更新します。       |
|`New-AzureRmDiskConfig`     |  構成可能なディスク オブジェクトを作成します。       |
|`Add-AzureRmVMDataDisk`     |  仮想マシンにデータ ディスクを追加します。   |

VM でキャッシュを管理するのに、次の手順でこれらのコマンドレットを使用しましょう。
