VM などのリソースを初めて作成する場合には、Azure portal が最も簡単です。 ただし、いくつかのリソースをまとめて作成する場合は、Azure を使用する方法が、最も効率的または早いわけではありません。 ここでは、さまざまなタスクを処理する数十の VM を最終的に作成します。 Azure portal でそれらを手動で作成するのは、楽しい作業ではありません。

Azure でリソースを作成および管理する方法には、他に次などもあります。

- Azure Resource Manager
- Azure PowerShell
- Azure CLI
- Azure REST API
- Azure クライアント SDK
- Azure VM 拡張機能
- Azure Automation サービス

## <a name="azure-resource-manager"></a>Azure Resource Manager

VM のコピーを、同じ設定で作成したいとします。 その場合、VM のイメージを作成し、それを Azure にアップロードし、新しい VM の基として参照します。 この手順は、非効率的で時間がかかります。 Azure には、VM の正確なコピーを作成する、テンプレートを作成するためのオプションがあります。

通常、お使いの Azure のインフラストラクチャには、多くのリソースが含まれており、そのほとんどが他のものに何らかの方法で関連しています。 たとえば、作成した VM には、WordPress サイトを実行するために、まとめてすべて作成した、仮想マシン自体、ストレージ、ネットワーク インターフェイス、Web サーバーおよびデータベースがあります。 **Azure Resource Manager** は、これらの関連リソースをより効率的に操作できるようにします。 これでは、リソースが名前付きの**リソース グループに**まとめられるため、ユーザーはすべてのリソースを、まとめてデプロイ、更新または削除できるようになります。 WordPress サイトの作成時、VM を作成する一貫としてリソース グループを作成しました。そして Resource Manager によって関連するリソースが同じグループに入れられました。

Resource Manager では、特定の構成を作成およびデプロイするために使用できる_テンプレート_も作成できます。

### <a name="what-are-resource-manager-templates"></a>Resource Manager テンプレートとは何ですか?

**Resource Manager テンプレート**とは、ソリューションに対してデプロイが必要なリソースを定義した JSON ファイルのことです。

リソース テンプレートは、特定の VM の **[設定]** セクションで [Automation スクリプト] オプションを選択することによって、作成することができます。

![VM 用の Automation スクリプト](../media/4-automation-script.png)

ユーザーには、リソース テンプレートを後で使用するためのオプション、またはこのテンプレートを使用して新しい VM をすぐにデプロイするオプションがあります。 たとえば、テスト環境でテンプレートから作成した VM が、お使いのオンプレミスのマシンと置き換える場合に、うまく機能しないことがわかったとします。 リソース グループを削除し (このとき、すべてのリソースが削除されます)、テンプレートを調整して、再度試すことができます。 デプロイ済みの既存のリソースのみを変更したい場合、それを作成するために使用したテンプレートを変更して、再度デプロイできます。 Resource Manager により、新しいテンプレートに合うようリソースが変更されます。

一度、それが希望どおりに動作するようになると、そのテンプレートを使用し、ステージングや運用などインフラストラクチャの複数のバージョンを簡単に何度も作成することができるようになります。 VM 名、ネットワーク名、ストレージ アカウント名などのフィールドをパラメーター化し、繰り返しテンプレートを読み込み、さまざまなパラメーターを使用し、各環境用にカスタマイズすることができます。

お好きなプログラミング言語を、Azure CLI、Azure PowerShell などの Automation スクリプト ツールや Azure REST API で使用して、リソース テンプレートを処理して、この強力なツールでインフラストラクチャを迅速に起動させることができます。

## <a name="azure-powershell"></a>Azure PowerShell

管理スクリプトを作成することは、ワークフローを最適化する強力な方法です。 共通する反復的なタスクを自動化することができ、スクリプトは一度検証すれば毎回同じように実行されるため、エラーを削減できます。 **Azure PowerShell** は、1 回限りの対話型のタスクや、繰り返し実行するタスクを自動化するのに最適です。

> [!NOTE]
> PowerShell とは、シェル ウィンドウやコマンド解析などのサービスを提供する、クロスプラットフォームのシェルです。 Azure PowerShell は、(**コマンドレット**と呼ばれる) Azure 固有のコマンドを追加する、オプションのアドオン パッケージです。 Azure PowerShell のインストールと使用の詳細は、別のトレーニング モジュールで説明しています。

たとえば、`New-AzureRmVM` コマンドレットを使用すると、Azure の新しい仮想マシンを作成できます。

```powershell
New-AzureRmVm `
    -ResourceGroupName "TestResourceGroup" `
    -Name "test-wp1-eus-vm" `
    -Location "East US" `
    -VirtualNetworkName "test-wp1-eus-network" `
    -SubnetName "default" `
    -SecurityGroupName "test-wp1-eus-nsg" `
    -PublicIpAddressName "test-wp1-eus-pubip" `
    -OpenPorts 80,3389
```

ここで示すさまざまなパラメーターを提供し、利用できる多数の VM の構成設定を行えます。 ほとんどのパラメーターには妥当な値があるため、必須パラメーターのみを指定する必要があります。 Azure PowerShell で VM を作成および管理する方法の詳細については、「**Automate Azure tasks using scripts with PowerShell**」(PowerShell でスクリプトを使用して Azure タスクを自動化する) のモジュールを参照してください。

## <a name="azure-cli"></a>Azure CLI

スクリプトとコマンド ラインで Azure とやり取りする別のオプションに、**Azure CLI** があります。

Azure CLI は、コマンド ラインから仮想マシンやディスクなどの Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。 このツールは、macOS、Linux、および Windows で使用できます。またはブラウザーで Cloud Shell を使用して使用できます。 Azure PowerShell のように、Azure CLI は、管理ワークフローを効率化する強力な方法です。 Azure CLI では、Azure PowerShell とは異なり、機能に PowerShell は不要です。

たとえば、`az vm create` コマンドを使用して Azure VM を作成することができます。

```azurecli
az vm create \
    --resource-group TestResourceGroup \
    --name test-wp1-eus-vm \
    --image win2016datacenter \
    --admin-username jonc \
    --admin-password aReallyGoodPasswordHere
```

Azure CLI は、Ruby や Python などのその他のスクリプト言語でも使用できます。 いずれの言語も、管理者が PowerShell に精通していない場合に、Windows 以外のコンピューターでよく使用されます。

VM を作成および管理する方法の詳細については、「**Manage virtual machines with the Azure CLI tool**」(Azure CLI ツールを使用して仮想マシンを管理する) モジュールを参照してください。

## <a name="programmatic-apis"></a>プログラム (API)

一般的に、実行するスクリプトがシンプルであり、コマンドライン ツールを使用したい場合、Azure PowerShell と Azure CLI はいずれも適した選択肢です。 複雑なロジックの大規模なアプリケーションの一部として VM を作成および管理する場合のより複雑なシナリオでは、別のアプローチをとる必要があります。

Azure では、プログラムを使用して、さまざまな種類のリソースとやりとりすることができます。

### <a name="azure-rest-api"></a>Azure REST API

開発者は、Azure REST API により、リソースでカテゴリ化された操作を実行できるようになり、また VM を作成および管理できるようになります。 操作は、対応する HTTP メソッド (`GET`、`PUT`、`POST``DELETE`、および `PATCH`) と対応する応答と共に、URI として公開されます。

Azure コンピューティング API によって、仮想マシンおよび仮想マシンでサポートされるリソースに、プログラムからアクセスできるようになります。 この API を使用すると、次の操作を実行できます。

- 可用性セットの作成および管理
- 仮想マシンの拡張機能の追加と管理
- マネージド ディスク、スナップショットおよびイメージの作成と管理
- Azure で利用できるプラットフォーム イメージへのアクセス
- リソースの使用量情報の取得
- 仮想マシンの作成と管理
- 仮想マシン スケールセットの作成と管理

### <a name="azure-client-sdk"></a>Azure クライアント SDK

REST API はプラットフォームと言語に依存しませんが、開発者はしばしばより高いレベルの抽象化を求めます。 Azure クライアント SDK では、Azure REST API をカプセル化できるため、開発者は Azure とより簡単にやりとりすることができるようになります。

Azure クライアント SDK は、C#、Java、Node.js、PHP、Python、Ruby、および Go など .NET ベースの言語を含む、さまざまな言語やフレームワークで使用できます。

`Microsoft.Azure.Management.Fluent` NuGet パッケージが使用された Azure VM を作成する C# コードのスニペット例を次に示します。

```csharp
var azure = Azure
    .Configure()
    .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
    .Authenticate(credentials)
    .WithDefaultSubscription();
// ...
var vmName = "test-wp1-eus-vm";

azure.VirtualMachines.Define(vmName)
    .WithRegion(Region.USEast)
    .WithExistingResourceGroup("TestResourceGroup")
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("jonc")
    .WithAdminPassword("aReallyGoodPasswordHere")
    .WithComputerName(vmName)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

**Azure Java SDK** が使用された Java の同じスニペットは、次のとおりです。

```java
String vmName = "test-wp1-eus-vm";
// ...
VirtualMachine virtualMachine = azure.virtualMachines()
    .define(vmName)
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("TestResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("jonc")
    .withAdminPassword("aReallyGoodPasswordHere")
    .withComputerName(vmName)
    .withSize("Standard_DS1")
    .create();
```

## <a name="azure-vm-extensions"></a>Azure VM 拡張機能

仮想マシンの最初のデプロイ後に、ソフトウェアを追加で構成およびインストールしたいとします。 このタスクで、自動的に監視および実行される特定の構成を使用したいとします。

**Azure VM 拡張機能**は、Azure VM の最初のデプロイ後にタスクを構成および自動化したい場合に使用できる、小規模なアプリケーションです。 **Azure VM 拡張機能**は、Azure CLI、PowerShell、Azure Resource Manager テンプレート、および Azure portal を使って実行できます。

拡張機能は、新しい VM のデプロイにバンドルすることも、既存のシステムに対して実行することもできます。

## <a name="azure-automation-services"></a>Azure Automation サービス

リモート インフラストラクチャを管理する場合の運用上の最大の課題は、時間の短縮、エラーの削減、効率性の向上です。 多くのインフラストラクチャ サービスがある場合、高いレベルから操作できるように、Azure でレベルの高いサービスを使用することを検討したい場合があるでしょう。

**Azure Automation** では、頻繁に発生する、時間のかかる、エラーが発生しやすいタスクを簡単自動化して、サービスを統合できます。 これらのサービスには、**プロセスの自動化**、**構成管理**、および**更新管理**が含まれます。

- **プロセス管理**。 特定のエラー イベントを監視している VM があるとします。 それが報告されたら、できるだけ早くアクションをとり、問題を解決したいと思っています。 プロセス オートメーションを使用すると、データセンターで発生する可能性のあるイベントに対応する監視タスクを設定できます。

- **構成管理**。  ユーザーが、VM で実行しているオペレーティング システムで利用可能なソフトウェアの更新をトラッキングしたいとします。 ユーザーには、インストールしたいまたはしたくない、特定の更新プログラムがあります。 構成管理では、これらの更新プログラムを追跡して、必要に応じてアクションをとることができます。 会社の PC、サーバー、モバイル デバイスの管理には、**System Center Configuration Manager** を使用します。 Configuration Manager を使用すると、このサポートを、Azure VM まで拡張することができます。

- **Update Management**。 これは、ご使用の VM の更新プログラムおよび修正プログラムの管理に使用します。 このサービスでは、利用可能な更新プログラムの状態を評価したり、インストールをスケジュールしたり、デプロイの結果を確認して、更新プログラムが正常に適用されたことを検証したりすることができます。 Update Management には、プロセスおよび構成を管理するサービスが組み込まれています。 VM の Update Management は、**Azure Automation** アカウントから直接有効にできます。 また、ポータルの仮想マシン ブレードから単一の仮想マシンの Update Management を有効にすることもできます。

ご覧のとおり、Azure には、_ご自分に有効な_プロセスに管理操作を統合できる、リソースを作成および管理するツールが多数あります。 インフラストラクチャのリソースがスムーズに実行されるよう、その他の Azure のサービスを検証してみましょう。
