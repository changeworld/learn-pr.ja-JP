会社がクラウド移行の一環としていくつかのサーバーをデプロイしているとします。 デプロイ時には VM ディスクを暗号化する必要があるため、ディスクが攻撃を受けやすくなることはありません。 このプロセスを自動化して、暗号化を自動的に有効化するように Azure Resource Manager テンプレートを変更することをお勧めします。

ここでは、Azure Resource Manager テンプレートを使用して、新しい Windows VM の暗号化を自動的に有効にする方法を見ていきます。

## <a name="what-are-azure-resource-manager-templates"></a>Azure Resource Manager テンプレートとは

Resource Manager テンプレートは、Azure にデプロイするリソース セットを定義するための JSON ファイルです。 新規作成することも、VM など一部の Azure リソースについては、Azure portal を使用して生成することもできます。 手動の VM デプロイでは必須情報を入力する必要がありますが、VM を Azure にデプロイする代わりに、テンプレートを保存します。 テンプレートを_再利用_し、その特定の VM 構成を作成できます。

あらゆる種類の管理タスクを自動化するサンプル テンプレートを[こちら](https://azure.microsoft.com/resources/templates)のドキュメントで紹介しています。 今回は手動で VM を暗号化しましたが、実はここにあるテンプレートを使用することもできました。

![Azure テンプレートのスクリーンショット。](../media/5-browse-templates.png)

## <a name="using-github-templates"></a>GitHub テンプレートの使用

実際のテンプレート ソースは GitHub に保存されています。 GitHub でテンプレートを参照し、そのページから Azure に直接デプロイできます。

![GitHub テンプレートのスクリーンショット。[Azure にデプロイ] ボタンが強調表示されています。](../media/5-deploy-from-github.png)

テンプレートがデプロイされると、Azure では、必須入力フィールドの一覧が表示されます。

![Azure portal のテンプレートのスクリーンショット。](../media/5-fill-in-template.png)

次に、テンプレートを実行してリソースを作成、修正、削除できます。

### <a name="running-templates-in-the-azure-portal"></a>Azure portal でテンプレートを実行する

使用するテンプレートが既にわかっている場合、あるいは自分の Azure アカウントでテンプレートを保存している場合、**[リソースの作成]** の **[テンプレートのデプロイ]** リソースを使用し、ポータルで定義済みのテンプレートを検索し、実行できます。 名前でテンプレートを検索したり、テンプレートを編集してパラメーターや動作を変更したり、GUI から直接テンプレートを実行したりできます。

### <a name="running-templates-from-the-command-line"></a>コマンドラインからテンプレートを実行する

テンプレートに URL を指定することで、Azure PowerShell でテンプレートを実行できます。 たとえば、次の PowerShell コマンドでディスク暗号化テンプレートを実行できます。

```powershell
New-AzureRmResourceGroupDeployment `
    -Name encrypt-disk `
    -ResourceGroupName <resource-group-name> `
    -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

あるいは、Azure CLI を使用する場合、`group deployment create` コマンドで実行できます。

```azurecli
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> \ 
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

