Azure Storage では、コンテナーと各アカウントに格納されたデータを操作に基づく Representational State Transfer (REST) web API を提供します。 格納できるデータの各種類で使用できる独立した Api があります。 次の 4 つの特定のデータ型があることを思い出してください。

- **Blob**バイナリ、テキスト ファイルなどの非構造化データ。
- **キュー**の永続的なメッセージングします。
- **テーブル**キー/値の構造化ストレージ用です。
- **ファイル**従来の smb ファイル共有。

## <a name="using-the-rest-api"></a>REST API の使用

ストレージ REST Api では、HTTP または HTTPS 要求を送信したり、HTTP/HTTPS 応答を受信して、インターネット経由で仮想ネットワーク経由で、任意のアプリケーションから Azure で実行するサービスからアクセスできます。

たとえば、コンテナー内のすべての blob を一覧表示する場合は、ような送信は。

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

返されますデータと XML ブロックを特定のアカウントに。

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

ただし、このアプローチは、多くの手動解析し、各 API を使用する HTTP パケットの作成が必要です。 このため、Azure には、事前に構築された_クライアント ライブラリ_サービスの操作を一般的な言語とフレームワークを簡単に構成します。

## <a name="using-a-client-library"></a>クライアント ライブラリを使用します。

クライアント ライブラリは、API がテストされ、REST API によって送受信されたデータ モデルのより良いラッパーを提供する多くの場合、ために、大量のアプリケーションの開発者の作業を節約できます。

:::row:::
    :::column:::
        Microsoft はさまざまな言語となどのフレームワークをサポートする Azure クライアント ライブラリ: .NET、Java、Python、Node.js - Go :::column-end:::: :::column:::
        <br> ![Azure で使用できるサポートされているフレームワークのサンプルのロゴ](../media/4-common-tools.png)
    :::column-end:::
:::row-end:::

たとえば、同じ c# 内の blob の一覧を取得するには、次のコード スニペットを使用しますでした。

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

または、JavaScript で。

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
> クライアント ライブラリは、シンだけ_ラッパー_ REST API 経由でします。 正確にこのような場合は、web サービスを直接使用するように操作します。 これらのライブラリがオープン ソースの非常に透明にすることもできます。 GitHub でそれらを探します。

アプリケーションへのクライアント ライブラリのサポートを追加してみましょう。