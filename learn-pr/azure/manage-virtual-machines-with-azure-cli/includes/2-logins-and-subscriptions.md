始める前に、Azure CLI ツールの構文を確認しましょう。 「**Control Azure services with the Azure CLI**」(Azure CLI を使用した Azure サービスの制御) モジュールをご覧になった場合は、Azure CLI コマンドが次のような形式であることはご存じのはずです。

```azurecli
az [command] [subcommand] [--parameter --parameter]
```

`[command]` は、Azure の特定の制御対象領域を示します。 たとえば、`account` コマンドを使うとサブスクリプション情報を管理でき、`sql` コマンドを使うと SQL データベースを管理できます。 `[subcommand]` と `[--parameters]` は、使用しているコマンドに依存します。 

部分的なコマンドを入力することで、コマンド、サブコマンド、パラメーターの一覧を表示できます。 たとえば、コマンド ラインで「`az`」と入力すると最上位レベルのヘルプ画面が表示され、「`az vm`」と入力すると仮想マシンに対するすべてのサブコマンドが表示されます。 この方法は、Azure CLI ツールを調べるのにとても便利な場合があります。

> [!NOTE]
> ここでは、ブラウザーでホストされた Azure Cloud Shell から Azure CLI を使用します。 ローカル コンピューターを使用する方がよい場合は、[Azure CLI をインストールする](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ことにより、ここで説明するすべてのコマンドをコマンド ラインから実行することもできます。 このタスクの説明、 **Azure CLI を使用したコントロールの Azure サービス**モジュール。

## <a name="login-to-azure"></a>Azure にログインする

通常、まず、Azure CLI を使用する場合に行いますが、Azure アカウントにサインインするは。 それには、`az login` コマンドを使用します。 このコマンドにより、ブラウザー ウィンドウが起動されて、使用する Microsoft アカウントを選択できます。 使用している Azure サンド ボックスで、この手順は必要に応じて後は、代わりに、Azure サンド ボックスをアクティブ化する必要があります。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="working-with-subscriptions"></a>サブスクリプションの使用

このモジュールで、サブスクリプションの一時いたしますが、自分のアカウントからサブスクリプションを使用する通常。 複数のサブスクリプションがある場合は、`az account list --output table` ステートメントを使用して、わかりやすく書式設定されたサブスクリプションの一覧を取得できます。

```
Name                                  CloudName    SubscriptionId                        State    IsDefault
------------------------------------  -----------  ------------------------------------  -------  -----------
Contoso Legacy Resources              AzureCloud   abc13b0c-d2c4-64b2-9ac5-2f4cb819b752  Enabled  True
Visual Studio Enterprise              AzureCloud   233aebce-23c2-4572-c056-c029449e93ed  Enabled  False
```

このコマンドでは、すべてのコマンドが適用される "_既定の_" サブスクリプションも示されることに注意してください。 別のサブスクリプションで作業する場合は、`az account set --subscription "[name]"` コマンドを使用できます。 たとえば、現在のサブスクリプションを上記のリストの `Visual Studio Enterprise` に設定するには、次のコマンドを使用します。

```azurecli
az account set --subscription "Visual Studio Enterprise"
```