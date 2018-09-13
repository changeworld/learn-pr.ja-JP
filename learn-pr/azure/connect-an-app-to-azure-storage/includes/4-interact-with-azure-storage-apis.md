ソフトウェア開発者は、コードの記述に関して、できるだけ時間を節約する必要があります。 Azure Storage を操作するには、その REST エンドポイントを使用できます。 ただし、大量の作業が必要になります。 この問題により、Azure Storage への接続と使用を効率化するために、ライブラリが開発されるようになりました。 通常、これらは*クライアント ライブラリ*と呼ばれ、一般的に Azure Storage を操作する最も簡単な方法です。 

ここでは、ご自分の .NET アプリケーション内に参照として適切なクライアント ライブラリを追加する方法について説明します。

## <a name="application-programming-interface-api"></a>アプリケーション プログラミング インターフェイス (API)

Azure Storage では、REST (Representational State Transfer) プロトコルを使用してインターネット経由で、一連のアプリケーション プログラミング インターフェイス (API) を使用するサービスを公開しています。 これらの API は、「[Azure Storage Services REST API Reference (Azure Storage Services REST API リファレンス)](https://docs.microsoft.com/rest/api/storageservices/)」にすべて記載されています。 これらの API を直接操作できるときに、Azure Storage のアクセスは、一般的にクライアント ライブラリを使用して実行されます。 API は十分にテストされているため、アプリケーション開発者は、クライアント ライブラリによって膨大な作業量を節約することができます。

## <a name="language-support"></a>言語のサポート

複数の言語をサポートする Azure Storage にアクセスするために、複数のクライアント ライブラリが用意されています。 この [SDK Tools ドキュメント ページ](https://docs.microsoft.com/azure/#pivot=sdkstools)には、現在、サポートされている言語のすべてに対し、クライアント ライブラリまたは SDK へのリンクが含まれています。 これには、.NET、Java、Python、Node.js、Go が含まれます。

## <a name="summary"></a>まとめ

Azure Storage では、そのサービスのすべてを操作するために REST エンドポイントを提供します。 ユーザーはそのエンドポイントを使用できますが、クライアント ライブラリを使用する方が一般的です。 Azure Storage では、.NET、Java、Python、Node.js など、複数の言語をサポートしています。
