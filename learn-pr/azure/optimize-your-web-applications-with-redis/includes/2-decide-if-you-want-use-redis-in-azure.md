あなたのスポーツ Web サイトの背後にはデータベースがあり、クエリを実行することでデータが返されます。 しかし、特に大規模なスポーツ イベントなど、負荷が高くなるときには、パフォーマンスが低下します。 ホスト環境では、リソース使用量の増加はコストの増加につながります。 データをキャッシュすることで、Web サイトが適切かつ経済的に実行されるようになります。

## <a name="what-is-caching"></a>キャッシュとは

キャッシュは、頻繁にアクセスされるデータを、アプリケーションが使用する非常に近いメモリに格納する機能です。 キャッシュは、パフォーマンスを向上し、サーバーの負荷を減らすために使用されます。 ここでは、Redis を使用して、短い待機時間で、パフォーマンスを大幅に高める可能性があるメモリ内キャッシュを作成します。

## <a name="what-is-a-redis-cache"></a>Redis キャッシュとは

Redis (**RE**mote **DI**ctionary **S**erver) キャッシュは、オープン ソースのメモリ内キー値ペアのストアです。 Redis は高速で、文字列、ハッシュ、セットなどの一般的なデータ型を格納および操作できるため、よく使われています。 また、ASP.NET、Python、C、C++、C#、Java、JavaScript、Node.js などの複数の言語をサポートしているため、開発者フレンドリと見なされています。

## <a name="what-is-azure-redis-cache"></a>Azure Redis Cache とは

Microsoft Azure Redis Cache は、広く支持されているオープン ソースの Redis キャッシュがベースとなっています。 マイクロソフトによって管理されている、セキュリティで保護された専用の Redis キャッシュにアクセスすることができます。 Azure Redis Cache を使用して作成されたキャッシュには、Microsoft Azure 内のあらゆるアプリケーションからアクセスすることができます。 Azure Redis Cache は通常、バックエンドのデータストアに大きく依存するシステムのパフォーマンスを向上させるために使用されます。

キャッシュされたデータは、データベースによってディスクから読み込まれるのではなく、Redis キャッシュを実行している Azure サーバーのメモリ内に配置されます。 キャッシュには高い拡張性もあります。 サイズと価格レベルはいつでも変更できます。

## <a name="how-is-data-stored-in-a-redis-cache"></a>Redis キャッシュでのデータの格納方法

Redis のデータは、_**ノード**_ と_**クラスター**_ に格納されます。

**ノード**は、データが格納されている Redis 内の領域です。

**クラスター**は、データセットが分割される 3 つ以上のノードのセットです。 クラスターは、ノードに障害が発生した場合、またはノードがクラスターの残りの部分と通信できない場合に操作を続行できるため、有用です。

## <a name="what-are-redis-caching-architectures"></a>Redis キャッシュ アーキテクチャとは

Redis キャッシュ アーキテクチャは、データをキャッシュ内に分散させる方法です。 Redis には、データを分散させる 3 つの主要な方法があります。

1. **単一ノード**
1. **複数ノード**
1. **クラスター化**

Redis キャッシュ アーキテクチャは、階層によって Azure 全体に分割されます。

### <a name="basic-cache"></a>Basic キャッシュ

Basic キャッシュでは、_**単一ノード**_ の Redis キャッシュが提供されます。 すべてのデータセットが 1 つのノードに格納されます。 この階層は、開発、テスト、および重要ではないワークロードに最適です。

### <a name="standard-cache"></a>Standard キャッシュ

Standard キャッシュでは、_**複数ノード**_ アーキテクチャが作成されます。 Redis によって、2 ノード プライマリ/セカンダリの構成でキャッシュがレプリケートされます。 2 つのノード間のレプリケーションは、Azure によって管理されます。 これは、マスター/スレーブ レプリケーションで運用可能なキャッシュです。

### <a name="premium-tier"></a>Premium レベル

Premium レベルには、Standard レベルの機能に加え、データの永続化、スナップショットの作成、およびデータのバックアップ機能が含まれます。 このレベルでは、データを複数の Redis ノードにシャード化する Redis クラスターを作成して、使用可能なメモリを増やすことができます。 Premium レベルでは、接続、サブネット、IP アドレス指定、およびネットワークの分離を完全に制御するための Azure Virtual Network もサポートしています。 このレベルには、geo レプリケーションも含まれているため、データをそれを使用するアプリの近くに確実に配置することができます。

## <a name="summary"></a>まとめ

データベースは大量のデータを格納するには適していますが、データの検索時に固有の待機時間があります。 クエリを送信すると、 サーバーはそのクエリを解釈して、データを検索して結果を返します。 サーバーには、要求を処理するための容量制限もあります。 要求が多すぎると、データの取得速度が低下する可能性があります。 キャッシュすると、頻繁に要求されるデータがメモリに格納されます。これにより、データベースのクエリを実行するよりも速くデータを返すことができるため、待機時間が短くなり、パフォーマンスが向上します。 Azure Redis Cache を使用すると、セキュリティで保護され、スケーラブルな専用 Redis Cache にアクセスできます。これは Microsoft が管理し、Azure でホストされています。