ここでは、Azure でコンテナーを作成し、完全修飾ドメイン名 (FQDN) を使用してインターネットに公開します。

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="why-use-azure-container-instances"></a>Azure Container Instances を使用する理由

Azure Container Instances は、シンプルなアプリケーション、タスク自動化、ビルド ジョブなど、分離されたコンテナーで操作できるシナリオに便利です。 Azure Container Instances には次の利点があります。

- **高速起動**: 数秒でコンテナーを起動します。
- **秒単位の課金**: コンテナーが実行されている間だけコストが発生します。
- **ハイパーバイザー レベルのセキュリティ**: アプリケーションを VM 内にあるのと同様に完全に分離します。
- **カスタム サイズ**: CPU コアとメモリの正確な値を指定します。
- **永続的なストレージ**: Azure Files 共有をコンテナーに直接マウントして、状態を取得して保持します。
- **Linux と Windows**: 同じ API を使用して、Windows と Linux の両方のコンテナーでスケジュールを設定します。

複数のコンテナー間でのサービスの検出、自動スケーリング、調整されたアプリケーション アップグレードなど、完全なコンテナー オーケストレーションが必要なシナリオには、Azure Kubernetes Service (AKS) をお勧めします。

## <a name="create-a-container"></a>コンテナーを作成する

名前、Docker イメージ、および Azure リソース グループを **az container create** コマンドに指定することで、コンテナーを作成します。 必要に応じて、DNS 名ラベルを指定してコンテナーをインターネットに公開できます。 この例では、小さな Web アプリをホストするコンテナーをデプロイします。 イメージを配置する場所を選択することもできます。既定では以下のように "米国東部" に設定されますが、次のリストに示されている自分に近い場所に変更することができます。

<!-- TODO: fix region list so it's not hardcoded here --> 無料のサンドボックスを使用すると、Azure のグローバル リージョンのサブセットにリソースを作成できます。 リソースを作成するときは、次のリストからリージョンを選択します。

:::row:::
    :::column:::
        - westus2 - centralus - eastus - westeurope - southeastasia :::column-end:::
:::row-end:::

Cloud Shell で次のコマンドを実行して、コンテナー インスタンスを開始します。 `--dns-name-label` の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。そのため、`[dns-name]` を一意の値に置き換える必要があります。

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label [dns-name] \
    --location eastus
```

数秒で、要求に対する応答が得られます。 最初に、コンテナーは**作成中**の状態ですが、数秒のうちに起動されます。 `az container show` コマンドを使用して状態を確認することができます。

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

コマンドを実行すると、コンテナーの完全修飾ドメイン名 (FQDN) とそのプロビジョニング状態が表示されます。

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

コンテナーが**成功**状態に推移したら、ブラウザーでその FQDN に移動して、成功を確認します。

ここでは、Web サーバーとアプリケーションを実行する Azure コンテナー インスタンスを作成しました。 また、このアプリケーションには、コンテナー インスタンスの FQDN を使用してアクセスしました。