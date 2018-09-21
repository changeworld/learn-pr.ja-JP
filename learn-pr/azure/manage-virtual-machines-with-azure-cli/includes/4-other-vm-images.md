<span data-ttu-id="fcced-101">仮想マシンを作成するためのイメージに `Debian` を使用しました。</span><span class="sxs-lookup"><span data-stu-id="fcced-101">We used `Debian` for the image to create the virtual machine.</span></span> <span data-ttu-id="fcced-102">Azure には、仮想マシンの作成に使用できる標準的な VM イメージがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="fcced-102">Azure has several standard VM images you can use to create a virtual machine.</span></span> 

## <a name="listing-images"></a><span data-ttu-id="fcced-103">イメージの一覧表示</span><span class="sxs-lookup"><span data-stu-id="fcced-103">Listing images</span></span>

<span data-ttu-id="fcced-104">次のコマンドを使用して、使用可能な VM イメージのリストを取得できます。</span><span class="sxs-lookup"><span data-stu-id="fcced-104">You can get a list of the available VM images using the following command.</span></span> 

```azurecli
az vm image list --output table
```

<span data-ttu-id="fcced-105">このコマンドを使用すると、Azure CLI に組み込まれているオフライン リストの一部である最も一般的なイメージが出力されます。</span><span class="sxs-lookup"><span data-stu-id="fcced-105">This will output the most popular images that are part of an offline list built into the Azure CLI.</span></span> <span data-ttu-id="fcced-106">ただし、Azure Marketplace には、_数百_の使用可能なイメージ オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="fcced-106">However, there are _hundreds_ of image options available in the Azure Marketplace.</span></span> 

## <a name="getting-all-images"></a><span data-ttu-id="fcced-107">すべてのイメージの取得</span><span class="sxs-lookup"><span data-stu-id="fcced-107">Getting all images</span></span>

<span data-ttu-id="fcced-108">完全なリストを取得するには、コマンドに `--all` フラグを追加します。</span><span class="sxs-lookup"><span data-stu-id="fcced-108">You can get a full list by adding the `--all` flag to the command.</span></span> <span data-ttu-id="fcced-109">Marketplace のイメージのリストは非常に大きいので、`--publisher`、`--sku`、または `–-offer` のオプション使用して、リストをフィルター処理すると便利です。</span><span class="sxs-lookup"><span data-stu-id="fcced-109">Since the list of images in the Marketplace is very large, it is helpful to filter the list with the `--publisher`, `--sku` or `–-offer` options.</span></span>

<span data-ttu-id="fcced-110">たとえば、次のコマンドを試行して、使用可能な_すべて_の Wordpress イメージを確認します。</span><span class="sxs-lookup"><span data-stu-id="fcced-110">For example, try the following command to see _all_ Wordpress images available:</span></span>

```azurecli
az vm image list --sku Wordpress --output table --all
```

<span data-ttu-id="fcced-111">または、次のコマンドで Microsoft が提供するすべてのイメージを確認します。</span><span class="sxs-lookup"><span data-stu-id="fcced-111">Or this command to see all images provided by Microsoft:</span></span>

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a><span data-ttu-id="fcced-112">場所に固有のイメージ</span><span class="sxs-lookup"><span data-stu-id="fcced-112">Location-specific images</span></span>

<span data-ttu-id="fcced-113">一部のイメージは、特定の場所でのみ使用可能です。</span><span class="sxs-lookup"><span data-stu-id="fcced-113">Some images are only available in certain locations.</span></span> <span data-ttu-id="fcced-114">コマンドに `--location [location]` フラグを追加して、仮想マシンを作成するリージョンで使用可能なものに結果を絞り込んでみましょう。</span><span class="sxs-lookup"><span data-stu-id="fcced-114">Try adding the `--location [location]` flag to the command to scope the results to ones available in the region where you want to create the virtual machine.</span></span> <span data-ttu-id="fcced-115">たとえば、Azure Cloud Shell に次のように入力して、`eastus` リージョンで使用できるイメージのリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="fcced-115">For example, type the following into Azure Cloud Shell to get a list of images available in the `eastus` region.</span></span>

```azurecli
az vm image list --location eastus --output table
```

<span data-ttu-id="fcced-116">その他の使用可能な Azure Sandbox の場所にあるイメージの一部をチェックするには、次を試行します。</span><span class="sxs-lookup"><span data-stu-id="fcced-116">Try checking some of the images in the other Azure Sandbox available locations:</span></span>

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> <span data-ttu-id="fcced-117">これらは、Azure で提供されている標準のイメージです。</span><span class="sxs-lookup"><span data-stu-id="fcced-117">These are the standard images that are provided by Azure.</span></span> <span data-ttu-id="fcced-118">固有の構成やオペレーティング システムのあまり一般的ではないバージョンまたはディストリビューションに基づいて VM を作成するために、[独自のカスタム イメージを作成してアップロード](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images)することもできます。</span><span class="sxs-lookup"><span data-stu-id="fcced-118">Keep in mind that you can also [create and upload your own custom images](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) to create VMs based on unique configurations or less common versions or distributions of an operating system.</span></span>

<span data-ttu-id="fcced-119">おそらく、コマンド ウィンドウの表示はこの時点で限界になります。必要に応じて `clear` を入力して画面をクリアできます。</span><span class="sxs-lookup"><span data-stu-id="fcced-119">Your command window is probably full now, you can type `clear` to clear the screen if you like.</span></span>