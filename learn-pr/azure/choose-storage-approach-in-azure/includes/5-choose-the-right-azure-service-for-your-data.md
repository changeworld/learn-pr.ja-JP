<span data-ttu-id="49ca9-101">正しいストレージ ソリューションを選択すると、パフォーマンスの向上につながります。</span><span class="sxs-lookup"><span data-stu-id="49ca9-101">Choosing the correct storage solution can lead to better performance.</span></span> <span data-ttu-id="49ca9-102">ここでは、オンライン小売りシナリオでのデータについて学習したことを適用して、各データ セットに最適な Azure サービスを見つけます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-102">Here, you'll apply what you've learned about the data in the online retail scenario and find the best Azure service for each data set.</span></span> 

## <a name="product-catalog-data"></a><span data-ttu-id="49ca9-103">商品カタログ データ</span><span class="sxs-lookup"><span data-stu-id="49ca9-103">Product catalog data</span></span>

<span data-ttu-id="49ca9-104">データの分類: 半構造化</span><span class="sxs-lookup"><span data-stu-id="49ca9-104">Data classification: Semi-structured</span></span>

<span data-ttu-id="49ca9-105">操作:</span><span class="sxs-lookup"><span data-stu-id="49ca9-105">Operations:</span></span>

* <span data-ttu-id="49ca9-106">顧客は、多くの読み取り操作と、データベース内の多くのフィールドに対してクエリを実行する機能を、必要とします。</span><span class="sxs-lookup"><span data-stu-id="49ca9-106">Customers require lots of read operations, with the ability to query on many fields within the database.</span></span>
* <span data-ttu-id="49ca9-107">店側では、常に変化する在庫を追跡するために多くの書き込み操作が必要です。</span><span class="sxs-lookup"><span data-stu-id="49ca9-107">The business requires lots of write operations to track the constantly changing inventory.</span></span>

<span data-ttu-id="49ca9-108">待機時間とスループット: 高スループットと低遅延</span><span class="sxs-lookup"><span data-stu-id="49ca9-108">Latency & throughput: High throughput and low latency</span></span>

<span data-ttu-id="49ca9-109">トランザクションのサポート: 必要</span><span class="sxs-lookup"><span data-stu-id="49ca9-109">Transactional support: Required</span></span>

### <a name="recommended-service-azure-cosmos-db"></a><span data-ttu-id="49ca9-110">推奨されるサービス: Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="49ca9-110">Recommended service: Azure Cosmos DB</span></span>

<span data-ttu-id="49ca9-111">Azure Cosmos DB は、設計上、半構造化データつまり NoSQL データをサポートします。</span><span class="sxs-lookup"><span data-stu-id="49ca9-111">Azure Cosmos DB supports semi-structured data, or NoSQL data, by design.</span></span> <span data-ttu-id="49ca9-112">そのため、Bluetooth 対応フィールドや、将来必要になる新しいフィールドなどの新しいフィールドのサポートは、Azure Cosmos DB で実現できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-112">So, supporting new fields, such as the Bluetooth Enabled field, or any new fields you need in the future is a given with Azure Cosmos DB.</span></span>

<span data-ttu-id="49ca9-113">操作に関しては、Azure Cosmos DB は SQL でのクエリをサポートし、すべてのプロパティには既定でインデックスが付けられるので、ほとんどすべてのものをフィルター処理するという顧客のニーズに合わせたクエリの作成がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="49ca9-113">Regarding operations, Azure Cosmos DB supports SQL for queries and every property is indexed by default, so creating queries to match your customers’ needs to filter on almost everything is supported.</span></span>

<span data-ttu-id="49ca9-114">待機時間とスループットに関しては、Azure Cosmos DB ではスループットを構成できるので、ショッピングが増える期間は高い顧客需要を処理するようにスケールアップし、少なくなったらコストを節約するためにスケールダウンすることができます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-114">Regarding latency and throughput, Azure Cosmos DB enables you to configure your throughput, so you can scale up to handle higher customer demand during peak shopping times or scale down during slower times to conserve cost.</span></span> <span data-ttu-id="49ca9-115">また、Azure Cosmos DB では既定ですべてのプロパティにインデックスが付けられるので、顧客は任意のフィールドでクエリを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-115">And because Azure Cosmos DB indexes all properties by default, you'll be able to enable customers to query on any field.</span></span>

<span data-ttu-id="49ca9-116">Azure Cosmos DB は ACID に準拠しているので、トランザクションは厳密な要件に従って完了します。</span><span class="sxs-lookup"><span data-stu-id="49ca9-116">Azure Cosmos DB is also ACID-compliant, so you can be assured that your transactions are completed according to those strict requirements.</span></span>

<span data-ttu-id="49ca9-117">さらに、Azure Cosmos DB ではボタンをクリックするだけで世界中のどこにでもデータをレプリケートできます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-117">As an added plus, Azure Cosmos DB also enables you to replicate your data anywhere in the world with the click of a button.</span></span> <span data-ttu-id="49ca9-118">したがって、eコマース サイトのユーザーが米国、フランス、英国に集中している場合は、これらのデータ センターにデータをレプリケートすると、データがユーザーの近くに物理的に移動するので、待機時間を短縮できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-118">So, if your e-commerce site has concentrated users in the US, France, and England, you can replicate your data to those data centers to reduce latency as you've physically moved the data closer to your users.</span></span> <span data-ttu-id="49ca9-119">そして、世界各地にデータをレプリケートしても、5 つの整合性レベルのいずれかを選択して、整合性、可用性、待機時間、スループットの間のトレードオフを決定できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-119">And even with data replicated around the world, you can choose from one of five consistency levels so that you can determine the tradeoffs to make between consistency, availability, latency, and throughput.</span></span>

### <a name="why-not-other-azure-services"></a><span data-ttu-id="49ca9-120">他の Azure サービスにしない理由</span><span class="sxs-lookup"><span data-stu-id="49ca9-120">Why not other Azure services?</span></span>

<span data-ttu-id="49ca9-121">Azure Table Storage、HDInsight の一部としての Azure HBase、Azure Redis Cache などの他の Azure サービスも、NoSQL データを格納できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-121">Other Azure services such as Azure Table Storage, Azure HBase as a part of HDInsight, and Azure Redis Cache can also store NoSQL data.</span></span> <span data-ttu-id="49ca9-122">このシナリオでは、ユーザーは複数のフィールドでのクエリを望んでいるため、Azure Table Storage、Azure Redis Cache、HDInsight の一部としての Azure HBase より Azure Cosmos DB の方が適しています。Azure Cosmos DB では既定ですべてのフィールドにインデックスが付けられるのに対し、他のサービスではインデックスが付けられるデータが制限されるため、データベースの任意のフィールドに対してクエリを実行する機能が限定されるためです。</span><span class="sxs-lookup"><span data-stu-id="49ca9-122">In this scenario, because users will want to query on multiple fields, Azure Cosmos DB is a better fit than Azure Table Storage, Azure Redis Cache, and Azure HBase as a part of HDInsight because Azure Cosmos DB indexes every field by default, whereas the others are limited in the data they index, so they have limited abilities to query on any field in the database.</span></span>

## <a name="photos-and-videos"></a><span data-ttu-id="49ca9-123">写真とビデオ</span><span class="sxs-lookup"><span data-stu-id="49ca9-123">Photos and videos</span></span>

<span data-ttu-id="49ca9-124">データの分類: 非構造化</span><span class="sxs-lookup"><span data-stu-id="49ca9-124">Data classification: Unstructured</span></span>

<span data-ttu-id="49ca9-125">操作:</span><span class="sxs-lookup"><span data-stu-id="49ca9-125">Operations:</span></span>

* <span data-ttu-id="49ca9-126">写真やビデオでは、ID による取得だけが必要です。</span><span class="sxs-lookup"><span data-stu-id="49ca9-126">Photos and videos only need to be retrieved by ID.</span></span>
* <span data-ttu-id="49ca9-127">作成と更新はあまり頻繁ではなく、読み取り操作より待機時間が長くてもかまいません。</span><span class="sxs-lookup"><span data-stu-id="49ca9-127">Creates and updates will be somewhat infrequent and can have higher latency than read operations.</span></span>

<span data-ttu-id="49ca9-128">待機時間とスループット: ID による検索では、低遅延と高スループットをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="49ca9-128">Latency & throughput: Retrievals by ID need to support low-latency and high-throughput.</span></span> <span data-ttu-id="49ca9-129">作成と更新は、読み取り操作より待機時間が長くてもかまいません。</span><span class="sxs-lookup"><span data-stu-id="49ca9-129">Creates and updates can have higher latency than read operations.</span></span>

<span data-ttu-id="49ca9-130">トランザクションのサポート: 不要</span><span class="sxs-lookup"><span data-stu-id="49ca9-130">Transactional support: Not required</span></span>

## <a name="recommended-service-azure-blob-storage"></a><span data-ttu-id="49ca9-131">推奨されるサービス: Azure BLOB ストレージ</span><span class="sxs-lookup"><span data-stu-id="49ca9-131">Recommended service: Azure BLOB storage</span></span>

<span data-ttu-id="49ca9-132">Azure BLOB ストレージでは、写真やビデオなどのファイルを格納できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-132">Azure BLOB Storage supports storing files such as photos and videos.</span></span> <span data-ttu-id="49ca9-133">また、最もよく使用されるコンテンツをキャッシュしてエッジ サーバーに格納することにより Azure Content Delivery Network (CDN) で機能し、それらの画像をユーザーに提供するときの待機時間が減ります。</span><span class="sxs-lookup"><span data-stu-id="49ca9-133">It also works with Azure Content Delivery Network (CDN) by caching the most used content and storing it on edge servers, which reduces latency in serving up those images to your users.</span></span>

<span data-ttu-id="49ca9-134">Azure BLOB ストレージを使用すると、画像をホット ストレージ層からクール ストレージ層またはアーカイブ ストレージ層に移動することもでき、コストを削減して、最もよく見られる画像やビデオにスループットを集中できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-134">By using Azure BLOB storage, you can also move images from the hot storage tier to the cool or archive storage tier to reduce costs and focus throughput on the most viewed images and videos.</span></span>

### <a name="why-not-other-azure-services"></a><span data-ttu-id="49ca9-135">他の Azure サービスにしない理由</span><span class="sxs-lookup"><span data-stu-id="49ca9-135">Why not other Azure services?</span></span>

<span data-ttu-id="49ca9-136">画像を Azure App Service にアップロードして、アプリを実行しているのと同じサーバーで画像を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-136">You could upload your images to Azure App Services, so that the same server running your app is serving up your images.</span></span> <span data-ttu-id="49ca9-137">画像が多くない場合はそれでもかまいませんが、ファイルが多い場合、およびグローバルなユーザーに対しては、Azure BLOB ストレージと Azure Content Delivery Network を使用した方が、より高いパフォーマンスの結果を得られます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-137">That would work if you didn't have many images, but if you have lots of files, and a global audience you'll get more performant results by using Azure BLOB Storage with Azure Content Delivery Network.</span></span>

## <a name="business-data"></a><span data-ttu-id="49ca9-138">ビジネス データ</span><span class="sxs-lookup"><span data-stu-id="49ca9-138">Business data</span></span>

<span data-ttu-id="49ca9-139">データの分類: 構造化</span><span class="sxs-lookup"><span data-stu-id="49ca9-139">Data classification: Structured</span></span>

<span data-ttu-id="49ca9-140">操作: 複数のデータベースに対する読み取り専用の複雑な分析クエリ</span><span class="sxs-lookup"><span data-stu-id="49ca9-140">Operations: Read-only complex analytical queries across multiple databases</span></span>

<span data-ttu-id="49ca9-141">待機時間とスループット: クエリの複雑さによっては、結果においてある程度の待機時間が予想されます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-141">Latency & throughput: Some latency in the results is expected based on the complex nature of the queries.</span></span>

<span data-ttu-id="49ca9-142">トランザクションのサポート: 必要</span><span class="sxs-lookup"><span data-stu-id="49ca9-142">Transactional support: Required</span></span>

### <a name="recommended-service-azure-sql-database"></a><span data-ttu-id="49ca9-143">推奨されるサービス: Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="49ca9-143">Recommended service: Azure SQL Database</span></span>

<span data-ttu-id="49ca9-144">ほとんどの場合、ビジネス データのクエリはビジネス アナリストによって行われ、アナリストは他のクエリ言語より SQL をよく知っていると思われます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-144">Business data will most likely be queried by business analysts, who are more likely to know SQL than any other query language.</span></span> <span data-ttu-id="49ca9-145">Azure SQL Database だけでもソリューションとして使用できますが、Azure SQL Database と Azure Analysis Services を組み合わせると、Azure SQL Database 内のデータに対するセマンティック モデルを作成してから、それをビジネス ユーザーと共有することにより、行う必要があるすべてのことを BI ツールからのモデルに接続し、データをすぐに調べて分析情報を得ることができるようになります。</span><span class="sxs-lookup"><span data-stu-id="49ca9-145">Azure SQL Database could be used as the solution by itself but pairing Azure SQL Database with Azure Analysis services enables data analysts to create a semantic model over the data in Azure SQL Database and then share it with business users so that all they need to do is connect to the model from any BI tool and immediately explore the data and gain insights.</span></span> 

### <a name="why-not-other-azure-services"></a><span data-ttu-id="49ca9-146">他の Azure サービスにしない理由</span><span class="sxs-lookup"><span data-stu-id="49ca9-146">Why not other Azure services?</span></span>

<span data-ttu-id="49ca9-147">Azure SQL Data Warehouse は OLAP ソリューションと SQL クエリをサポートしますが、ビジネス アナリストはクロス データベース クエリを実行する必要があり、Azure SQL Data Warehouse ではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="49ca9-147">Azure SQL Data Warehouse supports OLAP solutions and SQL queries, however your business analysts will need to do perform cross database queries, which Azure SQL Data Warehouse does not support.</span></span>

<span data-ttu-id="49ca9-148">Azure SQL Database に加えて Azure Analysis Services を使用できますが、ビジネス アナリストは Power BI より SQL の方を熟知しているため、SQL クエリをサポートしているデータベースを好み、Azure Analysis Services はそれをサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="49ca9-148">Azure Analysis Services could be used in addition to Azure SQL Database, however your business analysts are more well versed in SQL than working with Power BI, so they'd like a database that supports SQL queries, which Azure Analysis Services does not support.</span></span> <span data-ttu-id="49ca9-149">さらに、ビジネス データ セットに格納する財務データは、リレーショナルで多次元であるという性質を持っており、Azure Analysis Services はサービス自体に格納された表形式のデータはサポートしますが、多次元データはサポートしません。</span><span class="sxs-lookup"><span data-stu-id="49ca9-149">In addition, the financial data you're storing in your business data set is relational and multidimensional in nature and Azure Analysis services supports Tabular data stored on the service itself, but not multidimensional data.</span></span> <span data-ttu-id="49ca9-150">Azure Analysis Services で多次元データを分析するには、Azure SQL Database への直接クエリを使用できます。</span><span class="sxs-lookup"><span data-stu-id="49ca9-150">To analyze multidimensional data with Azure Analysis Services, you can use direct query to the Azure SQL Database.</span></span>

<span data-ttu-id="49ca9-151">Azure Stream Analytics はデータを分析して、それをアクションにつながる分析情報に変換する優れた手段ですが、重視されているのはストリーミングされるリアルタイム データであり、この例でのビジネス アナリストは履歴データのみを調べています。</span><span class="sxs-lookup"><span data-stu-id="49ca9-151">Azure Stream Analytics is a great way to analyze data and transform it into actionable insights, but its focus is on real-time data that is streaming in, and in this case our business analysts will be looking at historical data only.</span></span>

## <a name="summary"></a><span data-ttu-id="49ca9-152">まとめ</span><span class="sxs-lookup"><span data-stu-id="49ca9-152">Summary</span></span>

<span data-ttu-id="49ca9-153">データの各カテゴリはストレージ要件が異なり、どのソリューションが最適か判断するのはユーザーの仕事です。</span><span class="sxs-lookup"><span data-stu-id="49ca9-153">Each category of data has different storage requirements and it's your job to figure out which solution is best.</span></span> <span data-ttu-id="49ca9-154">データのカテゴリ、必要な操作、待機時間、およびトランザクションのサポートの必要性を、常に考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49ca9-154">You should always consider: the category of data, required operations, latency, and the need for transactional support.</span></span>
