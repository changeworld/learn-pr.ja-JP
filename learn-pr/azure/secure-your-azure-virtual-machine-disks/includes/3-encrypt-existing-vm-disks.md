社内のすべての VM に Azure Disk Encryption (ADE) を実装することに決まったとします。 既存の VM ボリュームに暗号化をロールアウトする方法を評価する必要があります。

ここでは、ADE の要件と、既存の Windows VM のディスクを暗号化するための手順を紹介します。

## <a name="azure-disk-encryption-prerequisites"></a>Azure Disk Encryption の前提条件

最初の VM ディスクを暗号化する前にする必要があります。

1. Key Vault を作成します。
1. Azure Active Directory (Azure AD) アプリケーションとサービス プリンシパルを設定します。
1. Azure AD アプリのキー コンテナー アクセス ポリシーを設定します。
1. キー コンテナーに高度なアクセス ポリシーを設定します。

### <a name="azure-key-vault"></a>Azure Key Vault

ADE によって使用される暗号化キーは、Azure Key Vault に格納されていることができます。 Azure Key Vault は、シークレットを安全に保管し、それにアクセスするためのツールです。 シークレットは、API キー、パスワード、証明書など、アクセスを厳密に制御する必要がある任意のものです。 これにより、セキュリティで保護されたスケーラブルな高可用性の記憶域、連邦情報処理規格 (FIPS) で 140-2 Level 2 認定のハードウェア セキュリティ モジュール (Hsm)。 キー コンテナーを使用して、データの暗号化に使用されるキーを全面的に制御し、キーの使用方法を管理して監査できます。 構成し、Azure portal、Azure PowerShell、および Azure CLI を使用して、key vault を管理できます。

>[!NOTE]
> Azure Disk Encryption では、key vault と Vm が、同じ Azure リージョンでが必要です。これにより、シークレットの暗号化が地域の境界をまたいでことはできません。

### <a name="azure-ad-application-and-service-principal"></a>Azure AD アプリケーションとサービス プリンシパル

スクリプトまたはコードを使用して、VM の暗号化構成などのリソースにアクセスしたり変更したりするには、まず、**Azure Active Directory (AD) アプリケーション** を設定する必要があります。 Azure AD は、マルチ テナントのクラウド ベースのディレクトリおよび id 管理サービスです。 主要なディレクトリ サービス、アプリケーション アクセス管理、および ID 保護を 1 つのソリューションに結合します。

Azure **サービス プリンシパル**も必要になります。 サービス プリンシパルは、スクリプトまたはコードの実行に使用するサービス アカウントです。 特定のアクセス許可と、特定の Azure リソースに対するタスクを実行するために必要なスコープを割り当てることできます。

Azure AD で 2 つの要素がある: アプリケーションのオブジェクトが、 **_定義_** (内容)、アプリケーションとサービスのプリンシパルが、 **_特定のインスタンス_** アプリケーション。

この方法は**最小限の特権**の原則と一致しており、アプリに割り当てられるアクセス許可は、アプリがタスクを実行するための必要最小限に制限されます。

構成し、Azure AD アプリケーションと Azure portal、Azure PowerShell、および Azure CLI を使用してサービス プリンシパルを管理できます。

### <a name="key-vault-access-policies"></a>キー コンテナー アクセス ポリシー

ADE で詳細に必要な key vault に暗号化キーを格納することができます、前に、**クライアント ID**と**クライアント シークレット**の key vault への書き込みが許可されている Azure AD アプリケーション。

キー コンテナー内の暗号化キーへのアクセス権を Azure に付与する必要もあります。これにより、ボリュームをブートして暗号化する際に、それらの情報を VM に提供できるようになります。

## <a name="set-key-vault-advanced-access-policies"></a>キー コンテナーの高度なアクセス ポリシーを設定する

**高度なアクセス ポリシー**を使用すると、キー コンテナーに対するディスク暗号化が可能になります。これを使用しないと、暗号化のデプロイが失敗します。 

次の 3 つのポリシーを有効にする必要があります。

- **Disk encryption の Key Vault**します。 Azure Disk Encryption で必要です。
- **展開用の Key Vault**します。 Key vault からシークレットを取得する、Microsoft.Compute リソース プロバイダーを有効にします。 このポリシーは、VM を作成するときに必要です。
- **テンプレート デプロイメントに必要な場合は、Key Vault**します。 Key vault からシークレットを取得する Azure Resource Manager を有効にします。 VM のデプロイを Azure Resource Manager テンプレートを使用するときに、このポリシーが必要です。

キー コンテナー アクセス ポリシーの構成と管理には、Azure Portal、Azure PowerShell、または Azure CLI を使用します。

### <a name="what-is-the-azure-disk-encryption-prerequisites-configuration-script"></a>Azure Disk Encryption の前提条件の構成スクリプトとは

**Azure Disk Encryption の前提条件の構成スクリプト**すべて (または必要な数だけ)、設定の暗号化の前提条件。 スクリプトでは、key vault が、暗号化する VM と同じリージョンにあるも確認します。 リソース グループと key vault を作成し、key vault アクセス ポリシーを設定します。 また、このスクリプトでは、誤って削除されるのを防ぐため、キー コンテナーに対するリソース ロックも作成されます。

## <a name="encrypting-an-existing-vm-disk"></a>既存の VM ディスクの暗号化

Azure Disk Encryption の前提条件の構成スクリプトを使用しているときに、既存の VM ディスクを暗号化するための 2 つの手順があります。

1. Azure Disk Encryption の前提条件の構成スクリプトを実行します。
1. PowerShell で Azure の仮想マシンを暗号化します。
