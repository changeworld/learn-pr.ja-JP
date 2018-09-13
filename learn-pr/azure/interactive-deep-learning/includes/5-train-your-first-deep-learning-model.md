## <a name="train-your-first-deep-learning-model-using-pytorch-and-jupyter"></a><span data-ttu-id="b3d4d-101">PyTorch と Jupyter を使用して、最初のディープ ラーニング モデルをトレーニングします</span><span class="sxs-lookup"><span data-stu-id="b3d4d-101">Train your first deep learning model using PyTorch and Jupyter</span></span>

![PyTorch ロゴ](../media/5-image1.PNG) 

<span data-ttu-id="b3d4d-103">通常ディープ ラーニングのエンジニアはマトリックス代数をすべて手動でハード コード化することはありません。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-103">Typically deep learning engineers do not hard code matrix algebra operations all by hand.</span></span> <span data-ttu-id="b3d4d-104">PyTorch や TensorFlow などのフレームワークを代用します。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-104">They instead use frameworks such as PyTorch or TensorFlow.</span></span>  

<span data-ttu-id="b3d4d-105">PyTorch は、ディープ ラーニングの開発プラットフォームとしての柔軟性を提供する python ベースのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-105">PyTorch is a python-based framework that provides flexibility as a deep learning development platform.</span></span> <span data-ttu-id="b3d4d-106">PyTorch のワークフローは、python の科学技術コンピューティング ライブラリ NumPy 上に構築されます。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-106">PyTorch's workflow is built on top of python scientific computing library numpy.</span></span> 

<span data-ttu-id="b3d4d-107">PyTorch をディープ ラーニング モデルの構築に使用するのはなぜでしょうか。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-107">Now you might ask, why would we use PyTorch to build deep learning models?</span></span>  

- <span data-ttu-id="b3d4d-108">API が簡単に使えます – python と同じほど簡単です。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-108">Easy to use API – It's as simple as python can be.</span></span>
- <span data-ttu-id="b3d4d-109">Python のサポート – PyTorch が科学技術コンピューティング スタックとスムーズに統合されます。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-109">Python support – PyTorch smoothly integrates with the scientific computing stack.</span></span>
- <span data-ttu-id="b3d4d-110">動的計算グラフ – 特定機能付きで、事前に定義されたグラフではなく、PyTorch が実行時間内に変更可能な計算グラフを動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-110">Dynamic computation graphs – Instead of predefined graphs with specific functionalities, PyTorch build computational graphs dynamically that can be modified during runtime.</span></span> <span data-ttu-id="b3d4d-111">動的計算グラフは、入れ子になったバッチ処理に有効で、特定のネットワークに必要とされるメモリの量が不明の場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-111">Dynamic computation graphs are valuable for nested batching and when we do not know how much memory will be needed for creating a given network.</span></span>

## <a name="run-your-first-pytorch-model"></a><span data-ttu-id="b3d4d-112">最初の PyTorch モデルを実行します</span><span class="sxs-lookup"><span data-stu-id="b3d4d-112">Run your first PyTorch model</span></span>

<span data-ttu-id="b3d4d-113">最後の章でセットアップした、Jupyter Notebook に移動します。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-113">Navigate to the Jupyter Notebook that you set up in the last chapter.</span></span>

- <span data-ttu-id="b3d4d-114">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="b3d4d-114">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span></span>

<span data-ttu-id="b3d4d-115">First_pytorch_classifier.ipynb ノートブックを選択します</span><span class="sxs-lookup"><span data-stu-id="b3d4d-115">Select the first_pytorch_classifier.ipynb notebook</span></span>

![first_pytorch_classifier.ipynb を選択します](../media/5-image2.PNG)

<span data-ttu-id="b3d4d-117">ノートブックの指示に従って、最初の PyTorch クラシッファーをトレーニングします。</span><span class="sxs-lookup"><span data-stu-id="b3d4d-117">Follow the instructions in the notebook to train your first PyTorch classifer.</span></span>

![ノートブック のスクリーン ショット](../media/5-image3.PNG)
