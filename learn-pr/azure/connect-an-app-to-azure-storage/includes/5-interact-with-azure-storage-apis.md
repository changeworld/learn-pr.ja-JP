Azure Storage には、Representational State Transfer (REST) ベースの Web API があります。これはコンテナーと各アカウントに格納されているデータと連動します。 格納できる各種データと連動する独立 API を利用できます。 データの種類には次の 4 つがあることを思い出してください。

- **BLOB** はバイナリやテキスト ファイルなど、非構造化データに使用されます。
- **キュー**は固定のメッセージングに使用されます。
- **テーブル**はキー/値の構造化ストレージに使用されます。
- **ファイル**は従来の SMB ファイル共有に使用されます。

## <a name="using-the-rest-api"></a>REST API の使用

Storage REST API には、Azure で実行されているサービスから仮想ネットワーク経由でアクセスするか、HTTP/HTTPS 要求を送信し、HTTP/HTTPS 応答を受信できるあらゆるアプリケーションからインターネット経由でアクセスできます。

たとえば、あるコンテナーに含まれるすべての BLOB を一覧表示する場合、次のようなものを送信できます。

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

この場合、アカウントに固有のデータを含む XML ブロックが返されます。

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

ただし、この手法では、各 API と連動させるために、大量の HTTP パケットを手動で解析し、作成する必要があります。 このような理由から、Azure には_クライアント ライブラリ_が事前に組み込まれており、一般的な言語やフレームワークでサービスとの連動が簡単になっています。

## <a name="using-a-client-library"></a>クライアント ライブラリを使用する

クライアント ライブラリでは、アプリケーション開発者のために大量の作業を保存できます。これは、API が試験され、API が多くの場合、REST API で送受信されるデータ モデルを効果的にラップするためです。

:::row:::
    :::column:::
        Microsoft の Azure クライアント ライブラリは、.NET、Java、Python、Node.js、Go :::column-end::: など、さまざまな言語とフレームワークに対応しています。 :::column:::
        <br> ![サポートされており、Azure で使用できるフレームワークのサンプル ロゴ](../media/4-common-tools.png)
    :::column-end:::
:::row-end:::

たとえば、C# で BLOB の同じ一覧を取得するには、次のコード スニペットを使用できます。

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

あるいは、JavaScript で:

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
> クライアント ライブラリは、REST API に対する Thin _ラッパー_に過ぎません。 Web サービスを直接使用するとき、ユーザーの操作どおりのことを行います。 これらのライブラリはオープン ソースでもあり、非常に透過的になります。 GitHub でお探しください。

それでは、アプリケーションにクライアント ライブラリ サポートを追加しましょう。