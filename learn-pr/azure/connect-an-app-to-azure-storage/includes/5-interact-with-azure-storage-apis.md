<span data-ttu-id="49bc4-101">Azure Storage には、REST API があります。これはコンテナーおよび各アカウントに格納されているデータと連動します。</span><span class="sxs-lookup"><span data-stu-id="49bc4-101">Azure Storage provides a REST API to work with the containers and data stored in each account.</span></span> <span data-ttu-id="49bc4-102">格納できる各種データと連動する独立 API を利用できます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-102">There are independent APIs available to work with each type of data you can store.</span></span> <span data-ttu-id="49bc4-103">データの種類には次の 4 つがあることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="49bc4-103">Recall that we have four specific data types:</span></span>

- <span data-ttu-id="49bc4-104">**BLOB** はバイナリやテキスト ファイルなど、非構造化データに使用されます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-104">**Blobs** for unstructured data such as binary and text files.</span></span>
- <span data-ttu-id="49bc4-105">**キュー**は固定のメッセージングに使用されます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-105">**Queues** for persistent messaging.</span></span>
- <span data-ttu-id="49bc4-106">**テーブル**はキー/値の構造化ストレージに使用されます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-106">**Tables** for structured storage of key/values.</span></span>
- <span data-ttu-id="49bc4-107">**ファイル**は従来の SMB ファイル共有に使用されます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-107">**Files** for traditional SMB file shares.</span></span>

## <a name="using-the-rest-api"></a><span data-ttu-id="49bc4-108">REST API の使用</span><span class="sxs-lookup"><span data-stu-id="49bc4-108">Using the REST API</span></span>

<span data-ttu-id="49bc4-109">Storage REST API には、インターネット上の任意の場所から、HTTP/HTTPS 要求を送信し、HTTP/HTTPS 応答を受信できるあらゆるアプリケーションを使用してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-109">The Storage REST APIs are accessible from anywhere on the Internet, by any application that can send an HTTP/HTTPS request and receive an HTTP/HTTPS response.</span></span>

<span data-ttu-id="49bc4-110">たとえば、あるコンテナーに含まれるすべての BLOB を一覧表示する場合、次のようなものを送信できます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-110">For example, if you wanted to list all the blobs in a container, you would send something like:</span></span>

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

<span data-ttu-id="49bc4-111">この場合、アカウントに固有のデータを含む XML ブロックが返されます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-111">This would return an XML block with data specific to the account:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>  
<EnumerationResults AccountName="https://[url-for-service-account]/">  
  <Containers>  
    <Container>  
      <Name>container1</Name>  
      <Url>https://[url-for-service-account]/container1</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 18:09:03 GMT</Last-Modified>  
        <Etag>0x8CAE7D0C4AF4487</Etag>  
      </Properties>  
      <Metadata>  
        <Color>orange</Color>  
        <ContainerNumber>01</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container2</Name>  
      <Url>https://[url-for-service-account]/container2</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8C24928</Etag>  
      </Properties>  
      <Metadata>  
        <Color>pink</Color>  
        <ContainerNumber>02</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container3</Name>  
      <Url>https://[url-for-service-account]/container3</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8EAC0BB</Etag>  
      </Properties>  
      <Metadata>  
        <Color>brown</Color>  
        <ContainerNumber>03</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
  </Containers>  
  <NextMarker>container4</NextMarker>  
</EnumerationResults>  
```

<span data-ttu-id="49bc4-112">ただし、この手法では、各 API と連動させるために、大量の HTTP パケットを手動で解析し、作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49bc4-112">However, this approach requires a lot of manual parsing and the creation of HTTP packets to work with each API.</span></span> <span data-ttu-id="49bc4-113">このような理由から、Azure には_クライアント ライブラリ_が事前に組み込まれており、一般的な言語やフレームワークでサービスとの連動が簡単になっています。</span><span class="sxs-lookup"><span data-stu-id="49bc4-113">For this reason, Azure provides pre-built _client libraries_ that make working with the service easier for common languages and frameworks.</span></span>

## <a name="using-a-client-library"></a><span data-ttu-id="49bc4-114">クライアント ライブラリを使用する</span><span class="sxs-lookup"><span data-stu-id="49bc4-114">Using a client library</span></span>

<span data-ttu-id="49bc4-115">クライアント ライブラリでは、アプリケーション開発者のために大量の作業を保存できます。これは、API が試験され、API が多くの場合、REST API で送受信されるデータ モデルを効果的にラップするためです。</span><span class="sxs-lookup"><span data-stu-id="49bc4-115">Client libraries can save a significant amount of work for application developers because the API is tested and it often provides nicer wrappers around the data models sent and received by the REST API.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="49bc4-116">Microsoft の Azure クライアント ライブラリは、.NET、Java、Python、Node.js、Go :::column-end::: など、さまざまな言語とフレームワークに対応しています。</span><span class="sxs-lookup"><span data-stu-id="49bc4-116">Microsoft has Azure client libraries that support a number of languages and frameworks including: - .NET - Java - Python - Node.js - Go :::column-end::::</span></span> :::column:::
        <br> <span data-ttu-id="49bc4-117">![サポートされており、Azure で使用できるフレームワークのサンプル ロゴ](../media/4-common-tools.png)</span><span class="sxs-lookup"><span data-stu-id="49bc4-117">![Sample logos of supported frameworks you can use with Azure](../media/4-common-tools.png)</span></span> 
    :::column-end:::
:::row-end:::

<span data-ttu-id="49bc4-118">たとえば、C# で BLOB の同じ一覧を取得するには、次のコード スニペットを使用できます。</span><span class="sxs-lookup"><span data-stu-id="49bc4-118">For example, to retrieve the same list of blobs in C#, we could use the following code snippet:</span></span>

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

<span data-ttu-id="49bc4-119">あるいは、JavaScript で:</span><span class="sxs-lookup"><span data-stu-id="49bc4-119">Or in JavaScript:</span></span>

```javascript
const containerName = "...";
const blobService = storage.createBlobService();

blobService.listBlobsSegmented(containerName, null, function (error, results) {
    if (results) {
        for (var i = 0, blob; blob = results.entries[i]; i++) {
            // Work with blob item .. could be page blob, block blob, etc.
        }
    }
});
```

> [!NOTE]
> <span data-ttu-id="49bc4-120">クライアント ライブラリは、REST API に対する Thin _ラッパー_に過ぎません。</span><span class="sxs-lookup"><span data-stu-id="49bc4-120">The client libraries are just thin _wrappers_ over the REST API.</span></span> <span data-ttu-id="49bc4-121">Web サービスを直接使用するとき、ユーザーの操作どおりのことを行います。</span><span class="sxs-lookup"><span data-stu-id="49bc4-121">They are doing exactly what you would do if you used the web services directly.</span></span> <span data-ttu-id="49bc4-122">これらのライブラリはオープン ソースでもあり、非常に透過的になります。</span><span class="sxs-lookup"><span data-stu-id="49bc4-122">These libraries are also open source, making them very transparent.</span></span> <span data-ttu-id="49bc4-123">GitHub で探してください。</span><span class="sxs-lookup"><span data-stu-id="49bc4-123">Look for them on GitHub.</span></span>

<span data-ttu-id="49bc4-124">それでは、アプリケーションにクライアント ライブラリ サポートを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="49bc4-124">Let's add client library support to our application.</span></span>