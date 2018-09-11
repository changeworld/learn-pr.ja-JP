<span data-ttu-id="d4eb2-101">ここでは、新しい Azure 仮想マシンを作成することを目標にします。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-101">Our goal is to create a new Azure virtual machine.</span></span> <span data-ttu-id="d4eb2-102">リソースの場所、使用する OS、VM に対して必要なハードウェア構成を識別するために、いくつかの情報を指定することが必要になります。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-102">We'll need to supply several pieces of information to identify the resource location, OS to use, and the hardware configuration needed for the VM.</span></span> <span data-ttu-id="d4eb2-103">それでは、**リソース グループ**から始めましょう。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-103">Let's start with the **resource group**.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="d4eb2-104">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="d4eb2-104">Create a resource group</span></span>

<span data-ttu-id="d4eb2-105">Azure では_リソース グループ_を使用して、仮想マシンやデータベースなどの関連リソースがまとめてグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-105">Azure uses _resource groups_ to group related resources such as virtual machines and databases together.</span></span> <span data-ttu-id="d4eb2-106">また、リソース グループでは特定の場所 ("リージョン" と呼ばれる) が識別され、これによって、リソースが配置されるデータ センターが決定します。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-106">The resource group also identifies a specific location (called a "region") which will decide what data center the resource is placed into.</span></span>

<span data-ttu-id="d4eb2-107">実験ですので、`ExerciseResources` という名前の新しいリソース グループを作成し、それを `eastus` リージョンに配置することから始めます。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-107">Since we're experimenting, start by creating a new resource group named `ExerciseResources` and place it into the `eastus` region.</span></span>

<!-- TODO: replace with free ed-tier -->

<span data-ttu-id="d4eb2-108">Azure Cloud Shell で次の Azure CLI コマンドを入力して、ご利用のサブスクリプション内にリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-108">Type the following Azure CLI command in Azure Cloud Shell to create the resource group in your subscription.</span></span>

```azurecli
az group create --name ExerciseResources --location eastus
```

<span data-ttu-id="d4eb2-109">これにより、リソース グループが作成されたことを示す JSON ブロックが返されます。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-109">This will return a JSON block indicating the resource group has been created.</span></span> <span data-ttu-id="d4eb2-110">その内容は次のようなものとなります。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-110">It should look something like:</span></span>

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

<span data-ttu-id="d4eb2-111">応答の一部として、サブスクリプションの一意の識別子、場所、および名前が返されていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-111">Notice that it returns the subscription unique identifier, location, and name as part of the response.</span></span> <span data-ttu-id="d4eb2-112">これらを使用して、リソース グループが適切なサブスクリプションおよび場所で作成されているかどうかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-112">You can use these to verify it got created in the proper subscription and location.</span></span>

<span data-ttu-id="d4eb2-113">これでリソース グループの準備ができたので、次に新しい仮想マシンを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="d4eb2-113">Now that we have a resource group, let's create a new virtual machine.</span></span>