<span data-ttu-id="ff645-101">コンテナーはアプリケーションと計算プロセスを配信する最新の方法です。</span><span class="sxs-lookup"><span data-stu-id="ff645-101">Containers are a modern way of delivering applications and compute processes.</span></span> <span data-ttu-id="ff645-102">コンテナーを使用する場合、アプリケーションとすべての依存関係がいわゆる*コンテナー イメージ*にパッケージ化されます。</span><span class="sxs-lookup"><span data-stu-id="ff645-102">When using containers, applications and all dependencies are packaged into what is known as a *container image*.</span></span> <span data-ttu-id="ff645-103">コンテナー イメージは、コンテナー イメージ レジストリを使用することで、移植性に優れています。</span><span class="sxs-lookup"><span data-stu-id="ff645-103">Container images are super portable using a container image registry.</span></span> <span data-ttu-id="ff645-104">コンテナー イメージをご利用の開発システム上で作成し、次にそのイメージのインスタンスを Azure データ センター内で実行することができます。追加の変更を加えなくてもそれは動作するようになります。</span><span class="sxs-lookup"><span data-stu-id="ff645-104">You can create a container image on your development system, and then run an instance of that image in an Azure datacenter and have confidence that it will work without additional modification.</span></span>

## <a name="container-efficiencies"></a><span data-ttu-id="ff645-105">コンテナーの効率性</span><span class="sxs-lookup"><span data-stu-id="ff645-105">Container efficiencies</span></span>

<span data-ttu-id="ff645-106">コンテナーとコンテナー イメージは、ディスク領域、メモリ、CPU などのホスト リソースが効率的に使用されるように構築されます。</span><span class="sxs-lookup"><span data-stu-id="ff645-106">Containers and container images are built in such a way that they efficiently use host resources, such as disk space, memory, and CPU.</span></span> <span data-ttu-id="ff645-107">このような効率性により、コンテナーはすぐに起動します。</span><span class="sxs-lookup"><span data-stu-id="ff645-107">Due to these efficiencies, containers start quickly.</span></span> <span data-ttu-id="ff645-108">場合によっては、コンテナーの新しいインスタンスはほぼ瞬時に起動します。</span><span class="sxs-lookup"><span data-stu-id="ff645-108">In some cases, starting a new instance of a container is almost instantaneous.</span></span> <span data-ttu-id="ff645-109">これにより、アプリケーションの迅速なプロビジョニングが可能になるだけでなく、オンデマンド処理およびスケーリング操作の新しいモデルも可能になります。</span><span class="sxs-lookup"><span data-stu-id="ff645-109">This not only allows for quick provisioning of applications, but also a new model of on-demand processing and scale operations.</span></span>

<span data-ttu-id="ff645-110">このシナリオの想定: 需要の急増が確認される場合があるバッチ処理サービスを実行しています。</span><span class="sxs-lookup"><span data-stu-id="ff645-110">Envision this scenario: You run a batch processing service that occasionally sees a large spike in demand.</span></span> <span data-ttu-id="ff645-111">コンテナーを使用すれば、需要の増大に対応する新しいコンテナー インスタンスを迅速にプロビジョニングすることによって需要の増大に応答するシステムを構築することができます。</span><span class="sxs-lookup"><span data-stu-id="ff645-111">Using containers, you can build a system that reacts to increased demand by quickly provisioning new container instances to meet the increased demand.</span></span> <span data-ttu-id="ff645-112">それは強力なシステムですが、従来の仮想マシンを使用して実現するのは容易ではありません。</span><span class="sxs-lookup"><span data-stu-id="ff645-112">That's powerful and not easy to achieve with traditional virtual machines.</span></span>

<span data-ttu-id="ff645-113">コンテナーを使用すれば、起動の高速化に加えて、"ハイパー密度" を実現することができます。</span><span class="sxs-lookup"><span data-stu-id="ff645-113">In addition to fast start, with containers you can achieve "hyper density."</span></span> <span data-ttu-id="ff645-114">これは、事実上、より少ない仮想リソースまたは物理リソースによって、より多くのアプリケーションおよびプロセスを実行できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="ff645-114">This effectively means that you can run more applications and processes with less virtual or physical resources.</span></span>

## <a name="use-cases"></a><span data-ttu-id="ff645-115">ユース ケース</span><span class="sxs-lookup"><span data-stu-id="ff645-115">Use cases</span></span>

<span data-ttu-id="ff645-116">コンテナーは Web サーバーのような従来のワークロードを実行するのに適したプラットフォームであると同時に、負荷の急増に対応できるバッチ処理、最新の分散アーキテクチャでビルドされたアプリケーション、オンデマンド スケーリングを必要とする任意のものなど、機会を開くのにも役立ちます。</span><span class="sxs-lookup"><span data-stu-id="ff645-116">While containers are a great platform for running traditional workload like webservers, they also help open opportunities, such as burstable batch processing, applications built with a modern and distributed architecture, and anything that requires on-demand scale.</span></span>