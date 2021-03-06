自分の会社が開発した画期的ながん治療法が、第一面のニュースになっているところを想像してみてください。 これはすばらしいマイルストーンであり、会社の Web サイトには間違いなくアクセスが殺到するでしょう。 その Web サイトは、このアクセスの増加に対応できるでしょうか。サイトの反応が低下したり応答しなくなったりするおそれはないでしょうか。

ここでは、スケーリングと最適化の原則を使用して優れたアプリケーション パフォーマンスを確保する基本原理を見ていきます。

## <a name="what-is-scaling-and-performance-optimization"></a>スケーリングとパフォーマンスの最適化とは何ですか。

スケーリングとパフォーマンスの最適化とは、利用可能なリソースを受信要求を持つアプリケーションに適合させることです。 パフォーマンスの最適化には、リソースをスケーリングすること、潜在的なボトルネックを識別して最適化すること、最高のパフォーマンスが得られるようアプリケーション コードを最適化することが含まれています。

### <a name="scaling"></a>スケーリング

コンピューティング リソースは、2 つの方向にスケーリングできます。

* スケーリング "*アップ*" は他のリソースを 1 つのインスタンスに追加する操作です。
* スケーリング "*アウト*" とはインスタンスの追加です。

![パフォーマンス機能を向上するための、仮想マシンのスケーリング アップおよびスケーリング アウトを示す図。](../media/scale-up-scale-out.png)

スケールアップでは CPU やメモリなどの他のリソースを 1 つのインスタンスに追加します。 このインスタンスは、仮想マシンまたは PaaS サービスである場合があります。 インスタンスへより多くの容量を追加する機能により、アプリケーションで使用できるリソースが向上しますが、上限があります。 仮想マシンは実行中のホストの容量までに限られ、ホスト自体に物理的な制限があります。 最終的には、インスタンスのスケール アップを行う時に、これらの上限に達して、インスタンスにさらにリソースを追加する機能は制約を受けます。

スケール アウトは、サービスに追加のインスタンスを付加することが関係しています。 これらの追加のインスタンスは仮想マシンや PaaS サービスでもかまいませんが、1 つのインスタンスを強化することで容量を追加するのではなく、インスタンスの合計数を増やすことで容量を増加させます。 スケール アウトのメリットは、アーキテクチャに複数のマシンを追加すれば、想定上は永久にスケール アウトが可能である点です。 スケール アウトには、何らかの種類の負荷分散が必要です。 利用可能な複数のサーバー間に要求を分散させるロード バランサーや、要求の送信先となるアクティブなサーバーを識別するためのサービス検出メカニズムが考えられます。

どちらの場合も、リソースを削減し、コストの最適化を実現できます。

### <a name="performance-optimization"></a>パフォーマンスの最適化

パフォーマンスを最適化するときは、ネットワークとストレージで許容できるパフォーマンスレベルを確保してください。 どちらもアプリケーションの応答時間に影響を与える可能性があります。 実際のアーキテクチャに合った適切なネットワーク テクノロジとストレージ テクノロジを選択することが、最上級のエクスペリエンスをコンシューマーに提供することにつながります。

パフォーマンスの最適化には、アプリケーション自体がどのように実行されているかを理解することも含まれます。 依存システム内でのエラー、不具合のあるコード、ボトルネックはすべて、アプリケーション パフォーマンス管理ツールで明らかになります。 多くの場合、これらの問題はエンドユーザーや開発者、管理者には見えなかったり、わかりにくかったりします。そして、ご利用のアプリケーションの全体的なパフォーマンスに悪影響を及ぼす可能性があります。

## <a name="scalability-and-performance-patterns-and-practices"></a>スケーラビリティとパフォーマンスのパターンとプラクティス

ご利用のアプリケーションのスケーラビリティとパフォーマンスを強化するために利用できるいくつかのパターンとプラクティスを見てみましょう。

### <a name="data-partitioning"></a>データのパーティション分割

多くの大規模なソリューションでは、個別に管理およびアクセスすることができる複数の独立したパーティションにデータが分割されています。 パーティション分割戦略は、メリットを最大限に活かし、悪影響を最小限に抑えるために、入念に選択する必要があります。 パーティション分割を行うことで、拡張性が向上し、競合が少なくなり、パフォーマンスが最適化されます。

### <a name="caching"></a>キャッシュ

ご利用のアーキテクチャでキャッシュを使用すると、パフォーマンスの向上につながる可能性があります。 キャッシュは、使用頻繁の高いデータや資産 (Web ページ、画像) を、より高速に取得できるように格納するメカニズムです。 アプリケーションのさまざまなレイヤーで、キャッシュを使用できます。 アプリケーション サーバーとデータベース間でキャッシュを使用でき、データ取得時間を短縮することができます。 エンドユーザーと Web サーバーとの間にキャッシュを使用することもできます。静的なコンテンツをユーザーの近くに配置することで、エンドユーザーに Web ページを返すのにかかる時間を短縮することができます。 これには、ご利用のデータベース サーバーや Web サーバーから要求の負荷をオフロードし、他の要求のパフォーマンスを高めるという二次的な効果もあります。

### <a name="autoscaling"></a>自動スケール

自動スケールは、パフォーマンス要件に合わせてリソースを動的に割り当てるプロセスです。 作業量が増加すると、アプリケーションで、サービス レベル アグリーメント (SLA) に準拠し、必要なパフォーマンス レベルを維持するために追加のリソースが必要になる場合があります。 需要が減り、追加のリソースが必要なくなった場合は、コストを最小限に抑えるためにそれらのリソースの割り当てを解除できます。

自動スケールでは、クラウドでホストされている環境の弾力性が活用されると同時に、管理上の負担が軽減されます。 これにより、オペレーターがシステムのパフォーマンスを常時監視したり、リソースの追加や削除を判断したりする必要がなくなります。

### <a name="decouple-resource-intensive-tasks-as-background-jobs"></a>バック グラウンド ジョブとしてリソースを集中的に使用するタスクを切り離す

さまざまな種類のアプリケーションにおいて、ユーザー インターフェイス (UI) とは無関係に実行されるバックグラウンド タスクが必須となっています。 その例として、バッチ ジョブ、多くの処理能力を消費するタスク、長時間実行されるプロセス (ワークフローなど) を挙げることができます。 バックグラウンド ジョブの実行は、ユーザーの介入を必要としません。アプリケーションでジョブを起動した後も、ユーザーから対話式に送られる要求を処理し続けることができます。 アプリケーションの UI に対する負荷が小さくなるので、稼働率が向上し、対話の応答時間が短縮されます。

### <a name="use-a-messaging-layer-between-services"></a>サービス間のメッセージング レイヤーを使用する

サービスとサービスの間にメッセージング レイヤーを追加することで、パフォーマンスとスケーラビリティが向上する場合があります。 メッセージング レイヤーを追加することで、サービス間に要求のバッファーが形成され、アプリケーションの処理が追い付かなくなった場合でも、エラーが生じることなく要求のフローが維持されるようになります。 アプリケーションによって絶えず要求が処理されるので、受信した順序で応答が返されます。

### <a name="implement-scale-units"></a>スケール ユニットを実装する

ユニットとしてスケーリングします。 リソースごとに、スケーリング アクティビティが依存システムにどのような影響を及ぼす可能性があるかを特定します。 これにより、スケール アウト操作を容易に適用できるようになるため、アプリケーションは悪影響を受けにくくなります。 たとえば、x 個の Web ロールおよび worker ロールを追加すると、それらのロールによって生成される追加のワークロードを処理するために、y 個の追加キューおよび z 個のストレージ アカウントが必要になる場合があります。 スケール ユニットは x 個の Web ロールおよび worker ロール、y 個のキュー、z 個のストレージ アカウントで構成することが可能です。 1 つ以上のスケール ユニットを追加することでスケーリングが容易になるように、アプリケーションを設計してください。

### <a name="performance-monitoring"></a>パフォーマンスの監視

クラウドで実行される分散アプリケーションとサービスは、その性質上、数多くの変化する部分で構成される複雑なソフトウェアです。 運用環境では、ユーザーによるシステムの利用方法を追跡し、リソースの使用率をトレースし、システムの正常性とパフォーマンスを総合的に監視できることが重要です。 この情報を診断に使用して、問題の検出と修正に役立てることができます。さらに、潜在的な問題を見つけてその発生を防止するために役立てることもできます。

ご利用のアプリケーションのすべての階層を確認して、そのアプリケーションでのパフォーマンスのボトルネックを識別して修復します。 ご利用のアプリケーションのほか、ご利用のデータベースにインデックスを追加するプロセスでさえ、これらのボトルネックによってパフォーマンスの低いメモリ処理が行われている可能性があります。 1 つのボトルネックを解決すると、今まで気が付かなかった別のボトルネックが発覚するため、これは反復的なプロセスとなる可能性があります。

パフォーマンスの監視に完全な方法を使用することで、ご利用のアーキテクチャにメリットのあるパターンおよびプラクティスの種類を特定することができます。