<span data-ttu-id="76014-101">BLOB には、クラウド内のファイル ストレージと、データにアクセスするためのアプリを構築できる API が用意されています。</span><span class="sxs-lookup"><span data-stu-id="76014-101">Blobs give you file storage in the cloud and an API that lets you build apps to access the data.</span></span>

<span data-ttu-id="76014-102">ここでは、拡張現実ゲーム企業で働いている場合を例にして説明します。</span><span class="sxs-lookup"><span data-stu-id="76014-102">Suppose you work at an augmented-reality gaming company.</span></span> <span data-ttu-id="76014-103">すべてのモバイル プラットフォームで動作するゲームを構築しています。</span><span class="sxs-lookup"><span data-stu-id="76014-103">Your game runs on every mobile platform.</span></span> <span data-ttu-id="76014-104">ユーザーがゲームプレイの動画を録画し、サーバーにアップロードできる新機能をこのゲームに追加したいと考えています。</span><span class="sxs-lookup"><span data-stu-id="76014-104">You want to add a new feature to let users record video clips of their gameplay and upload the clips to your servers.</span></span> <span data-ttu-id="76014-105">ユーザーはゲーム内または Web サイトから動画を直接視聴することができます。</span><span class="sxs-lookup"><span data-stu-id="76014-105">Users will be able to watch clips directly in the game or through your website.</span></span> <span data-ttu-id="76014-106">分析とトレーサービリティのためにアップロードと閲覧をすべて記録する機能を搭載する予定です。</span><span class="sxs-lookup"><span data-stu-id="76014-106">You plan to log every upload and viewing for use in analytics and for traceability.</span></span>

<span data-ttu-id="76014-107">数千単位の同時アップロード、大量の動画データ、常に増大し続けるログ ファイルを処理できるストレージ ソリューションが必要です。</span><span class="sxs-lookup"><span data-stu-id="76014-107">You need a storage solution that can handle thousands of simultaneous uploads, massive amounts of video data, and constantly growing log files.</span></span> <span data-ttu-id="76014-108">また、複数のプラットフォームや言語から API にアクセスできるように、すべてのモバイル アプリと Web サイトに閲覧機能を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="76014-108">You also need to add the viewing functionality to all your mobile apps and your website, so you want API access from multiple platforms and languages.</span></span>

<span data-ttu-id="76014-109">ここでは、Azure Blob Storage がこのようなアプリケーションに適している可能性について説明します。</span><span class="sxs-lookup"><span data-stu-id="76014-109">Here, you will see how Azure Blob storage could be appropriate for this application.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="76014-110">学習の目的</span><span class="sxs-lookup"><span data-stu-id="76014-110">Learning objectives</span></span>

<span data-ttu-id="76014-111">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="76014-111">In this module, you will:</span></span>

- <span data-ttu-id="76014-112">Azure Blob Storage でデータを整理する</span><span class="sxs-lookup"><span data-stu-id="76014-112">Organize your data with Azure Blob storage</span></span>
- <span data-ttu-id="76014-113">BLOB を保持するストレージ リソースを作成する</span><span class="sxs-lookup"><span data-stu-id="76014-113">Create storage resources to hold blobs</span></span>
- <span data-ttu-id="76014-114">Azure Blob Storage でデータの保存と取得を行う</span><span class="sxs-lookup"><span data-stu-id="76014-114">Store and retrieve data from Azure Blob storage</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76014-115">前提条件</span><span class="sxs-lookup"><span data-stu-id="76014-115">Prerequisites</span></span>  

<span data-ttu-id="76014-116">なし</span><span class="sxs-lookup"><span data-stu-id="76014-116">None</span></span>