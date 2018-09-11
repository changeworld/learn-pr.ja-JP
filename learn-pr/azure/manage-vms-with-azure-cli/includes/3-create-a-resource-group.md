ここでは、新しい Azure 仮想マシンを作成することを目標にします。 リソースの場所、使用する OS、VM に対して必要なハードウェア構成を識別するために、いくつかの情報を指定することが必要になります。 それでは、**リソース グループ**から始めましょう。

## <a name="create-a-resource-group"></a>リソース グループの作成

Azure では_リソース グループ_を使用して、仮想マシンやデータベースなどの関連リソースがまとめてグループ化されます。 また、リソース グループでは特定の場所 ("リージョン" と呼ばれる) が識別され、これによって、リソースが配置されるデータ センターが決定します。

実験ですので、`ExerciseResources` という名前の新しいリソース グループを作成し、それを `eastus` リージョンに配置することから始めます。

<!-- TODO: replace with free ed-tier -->

Azure Cloud Shell で次の Azure CLI コマンドを入力して、ご利用のサブスクリプション内にリソース グループを作成します。

```azurecli
az group create --name ExerciseResources --location eastus
```

これにより、リソース グループが作成されたことを示す JSON ブロックが返されます。 その内容は次のようなものとなります。

```json
{
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources",
  "location": "eastus",
  "managedBy": null,
  "name": "ExerciseResources",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

応答の一部として、サブスクリプションの一意の識別子、場所、および名前が返されていることに注目してください。 これらを使用して、リソース グループが適切なサブスクリプションおよび場所で作成されているかどうかを確認することができます。

これでリソース グループの準備ができたので、次に新しい仮想マシンを作成しましょう。