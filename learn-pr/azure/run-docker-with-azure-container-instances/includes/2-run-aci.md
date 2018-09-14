ここでは、Azure でコンテナーを作成して、完全修飾ドメイン名 (FQDN) を使用してインターネットに公開します。

## <a name="why-use-azure-container-instances"></a>Azure Container Instances を使用する理由

Azure Container Instances は、単純なアプリケーション、タスクの自動化などの分離されたコンテナーで処理でき、ジョブを作成する場合に便利です。 Azure Container Instances には次の利点があります。

- **高速スタートアップ**: 数秒でコンテナーを起動します。
- **単位の 2 つ目の課金**: コンテナーの実行中にのみ、コストを発生します。
- **ハイパーバイザー レベルのセキュリティ**: アプリケーションを VM であるかのように完全に分離します。
- **カスタム サイズ**: CPU コアとメモリの正確な値を指定します。
- **永続的なストレージ**: Azure Files のマウントを取得し、状態を維持するためのコンテナーに直接共有します。
- **Linux および Windows**: 同じ API を使用して Windows と Linux の両方のコンテナーのスケジュールを設定します。

複数のコンテナー間でのサービスの検出、自動スケーリング、調整されたアプリケーション アップグレードなど、完全なコンテナー オーケストレーションが必要なシナリオには、Azure Kubernetes Service (AKS) をお勧めします。

## <a name="create-a-container"></a>コンテナーを作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

名前、Docker イメージ、およびする Azure リソース グループを提供することでコンテナーを作成する、 **az コンテナー作成**コマンド。 必要に応じて、DNS 名ラベルを指定してコンテナーをインターネットに公開できます。 この例では、小型の Web アプリをホストするコンテナーをデプロイします。

コンテナー インスタンスを起動する Cloud Shell で次のコマンドを実行します。 *--dns-name-label* の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。そのため、場合によっては一意性を確保するためにこの値を変更する必要があります。

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

数秒のうちに、要求に対する応答が得られます。 最初に、コンテナーは**作成中**の状態ですが、数秒のうちに起動されます。 `az container show` コマンドを使用して状態を確認することができます。

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
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

コンテナーに移動すると、 **Succeeded**状態、成功を確認する、ブラウザーでその FQDN に移動します。

ここでは、web サーバーとアプリケーションを実行する Azure コンテナー インスタンスを作成します。 また、このアプリケーションには、コンテナー インスタンスの FQDN を使用してアクセスしました。