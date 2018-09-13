## <a name="intro-to-deep-learning"></a><span data-ttu-id="af4f3-101">ディープ ラーニングの概要</span><span class="sxs-lookup"><span data-stu-id="af4f3-101">Intro to deep learning</span></span>

<span data-ttu-id="af4f3-102">Machine Learning ML の目的は、入力データ (画像、タイム シリーズ、オーディオなど) を指定された出力 (たとえばキャプション、価格値、トランスクリプション) に変換するモデルをトレーニングする機能を検索することです。</span><span class="sxs-lookup"><span data-stu-id="af4f3-102">The goal of Machine Learning ML is to find features to train a model that transforms input data (such as pictures, time series, or audio) to a given output (for example captions, price values, transcriptions).</span></span> <span data-ttu-id="af4f3-103">従来のデータ サイエンスでは、機能は多くの場合、慎重に選択されます。</span><span class="sxs-lookup"><span data-stu-id="af4f3-103">In traditional data science, features are often hand-picked.</span></span>

![フィード フォワード ディープ ニューラル ネットワークの Canonical 例です。](../media/2-image1.PNG)

<span data-ttu-id="af4f3-105">ディープ ラーニング (DL) では、入力をベクトルとして表し、これを一連の適切な線形代数操作によって指定された出力に変換することで機能の抽出プロセスを学習します。</span><span class="sxs-lookup"><span data-stu-id="af4f3-105">In Deep Learning (DL), the process of feature extraction is learned through representing inputs as vectors and transforming them, with a series of clever linear algebra operations, into a given output.</span></span>  

<span data-ttu-id="af4f3-106">次にモデル出力が、損失関数と呼ばれる式を使用して、想定される出力と比較されます。</span><span class="sxs-lookup"><span data-stu-id="af4f3-106">The model output is then compared against the expected output using an equation called a loss function.</span></span> <span data-ttu-id="af4f3-107">各トレーニング入力の損失関数によって返される値は、次のパス上で下位の損失となる機能を抽出するようにモデルをガイドするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="af4f3-107">The value returned by the loss function of each training input is used to guide the model to extract features that will result in a lower loss value on the next pass.</span></span>  
 
<span data-ttu-id="af4f3-108">線形代数コンポーネントの一部として計算する一連の行列演算は、計算上コストの高い傾向があり、並列処理が極めて高いことが多く、効率的に計算するにはグラフィックス処理ユニット (GPU) などの特殊な計算を必要とします。</span><span class="sxs-lookup"><span data-stu-id="af4f3-108">The series of matrix operations that we computer as part of the linear algebra component tend to be computationally expensive and are often heavily parallelizable, requiring specialized compute such as Graphics Processing Units GPUs to compute efficiently.</span></span>

## <a name="data-science-virtual-machine"></a><span data-ttu-id="af4f3-109">Data Science Virtual Machine</span><span class="sxs-lookup"><span data-stu-id="af4f3-109">Data Science Virtual Machine</span></span>

![Data Science Virtual Machine のオプション](../media/2-image2.PNG)

<span data-ttu-id="af4f3-111">データ サイエンス VM は、データ分析、機械学習、ディープ ラーニング トレーニングでよく使用される一般的なツールを使用して事前インストール、構成、テストされた Azure 仮想マシン イメージです。</span><span class="sxs-lookup"><span data-stu-id="af4f3-111">DSVMs are Azure Virtual Machine images, pre-installed, configured, and tested with several popular tools that are commonly used for data analytics, machine learning, and deep learning training.</span></span>

<span data-ttu-id="af4f3-112">次を提供します。</span><span class="sxs-lookup"><span data-stu-id="af4f3-112">They provide:</span></span>

- <span data-ttu-id="af4f3-113">チーム全体で一貫した設定で共有とコラボレーション、Azure スケールと管理、ほとんど不要なセットアップ、データ サイエンス用の完全クラウド ベースのデスクトップを推進。</span><span class="sxs-lookup"><span data-stu-id="af4f3-113">Consistent setup across team, promote sharing and collaboration, Azure scale and management, Near-Zero Setup, full cloud-based desktop for data science.</span></span>
- <span data-ttu-id="af4f3-114">オンデマンドでのエラスティックな容量で、垂直方向および水平方向のスケーリングによってすべての Azure ハードウェア構成の分析を実行する機能。</span><span class="sxs-lookup"><span data-stu-id="af4f3-114">On-demand elastic capacity Ability to run analytics on all Azure hardware configurations with vertical and horizontal scaling.</span></span> <span data-ttu-id="af4f3-115">料金は使用した量、使用したときのみ課金。</span><span class="sxs-lookup"><span data-stu-id="af4f3-115">Pay only for what you use, when you use it.</span></span>
- <span data-ttu-id="af4f3-116">ディープ ラーニング ツールが既に事前構成されているすぐに利用可能な GPU クラスターによるディープ ラーニング。</span><span class="sxs-lookup"><span data-stu-id="af4f3-116">Deep Learning with GPUs Readily available GPU clusters with Deep Learning tools already pre-configured.</span></span> 

<span data-ttu-id="af4f3-117">データ サイエンス VM には、Microsoft R Server Developer Edition、Anaconda Python、Python および R 用 Jupyter ノートブック、Python および R 用 IDE、SQL データベース、その他多くのデータ サイエンスと ML ツールなどの、ディープ ラーニング フレームワークとツールの一般的な GPU エディションを含む AI 用の複数のツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="af4f3-117">The DSVM contains several tools for AI including popular GPU editions of deep learning frameworks and tools such as Microsoft R Server Developer Edition, Anaconda Python, Jupyter notebooks for Python and R, IDEs for Python and R, SQL database and many other data science and ML tools.</span></span>

<span data-ttu-id="af4f3-118">Data Science Virtual Machine は、Azure の GPU の NC シリーズ VM インスタンスで実行できます。</span><span class="sxs-lookup"><span data-stu-id="af4f3-118">The DSVM can run on Azure GPU NC-series VM instances.</span></span> <span data-ttu-id="af4f3-119">これらの GPU では個別のデバイスの割り当てが使用され、ベア メタルに近いパフォーマンスが実現され、ディープ ラーニングの問題に適しています。</span><span class="sxs-lookup"><span data-stu-id="af4f3-119">These GPUs use discrete device assignment, resulting in performance close to bare-metal, and are well-suited to deep learning problems.</span></span>

<!--### Quiz? 

What is the goal of machine learning? 
How is traditional machine learning different from deep learning? 
Why are GPU's often used for deep learning? 
What does the DSVM provide? -->
