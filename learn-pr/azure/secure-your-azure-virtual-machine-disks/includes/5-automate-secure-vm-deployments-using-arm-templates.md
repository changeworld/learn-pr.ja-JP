会社がクラウド移行の一環としていくつかのサーバーをデプロイしているとします。 デプロイ時には VM ディスクを暗号化する必要があるため、ディスクが攻撃を受けやすくなることはありません。 このプロセスを自動化して、暗号化を自動的に有効化するように Azure Resource Manager テンプレートを変更することをお勧めします。

ここでは、Azure Resource Manager テンプレートを使用して、新しい Windows VM の暗号化を自動的に有効にする方法を見ていきます。

## <a name="what-are-azure-resource-manager-templates"></a>Azure Resource Manager テンプレートとは

これらのテンプレートは、仮想マシンなど、Azure にデプロイするリソースを定義するために使用される JSON ファイルです。 新規作成することも、VM など一部の Azure リソースについては、Azure portal を使用して生成できます。 手動の VM デプロイでは必須情報を入力する必要がありますが、VM を Azure にデプロイする代わりに、テンプレートを保存します。

GitHub でサンプル テンプレートを入手できます。

## <a name="using-github-templates"></a>GitHub のテンプレートの使用

GitHub には、**実行中の Windows VM ARM での暗号化の有効化**と呼ばれる、暗号化を有効にするためのテンプレートがあります。 これは、[Azure クイック スタート テンプレート](https://github.com/Azure/azure-quickstart-templates) リポジトリにあります。 テンプレートの readme ページには **[Azure へのデプロイ]** ボタンがあり、これを使用すると Azure portal でテンプレートが開かれます。

テンプレートを使用すると、暗号化が事前に有効化された Windows Server VM がデプロイされます。 テンプレートを使用する前に、暗号化の前提条件がすべて揃っていることを確認する必要があります。 前提条件スクリプトによって提供される構成情報 (Azure Active Directory クライアント ID や Azure Active Directory クライアント シークレットなど) も必要になります。

テンプレートに必要な情報を入力して新しい VM を作成します。 **[購入]** をクリックすると、デプロイが開始します (通常、コストは標準の Azure コンピューティング料金)。

## <a name="deploy-an-encrypted-vm-by-using-a-template"></a>暗号化された VM をテンプレートを使用してデプロイする

暗号化された VM をテンプレートを使用してデプロイする主な手順は次のとおりです。

1. GitHub の**実行中の Windows VM ARM での暗号化の有効化**テンプレートにアクセスして実行します。

1. スクリプトの構成ページに必須の詳細を追加します。

1. **[購入]** をクリックし、スクリプトを使用して新しい VM をデプロイします。

1. Azure Portal を使用してディスク暗号化の状態を確認します。
