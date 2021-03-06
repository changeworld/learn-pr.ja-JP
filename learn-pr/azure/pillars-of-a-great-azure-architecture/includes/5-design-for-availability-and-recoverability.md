Lamna Healthcare に属する医師は、ダウンタイムをほとんど許容できません。 常に最高品質の治療を提供できるように、24 時間医療用 IT システムにアクセスできる必要があります。 医師からの 24 時間の要求を満たすために、Lamna Healthcare のアプリケーションでは、ユーザーへの影響を最小限にして障害を処理する必要があります。 局所的なインシデントと大規模災害のどちらが発生した場合でも、アプリケーションの継続稼働を保証する方法はあるのでしょうか。

ここでは、可用性と回復性をアーキテクチャの設計に含める方法について説明します。

## <a name="availability-and-recoverability"></a>可用性と回復性

複雑なアプリケーションでは、いくつもの箇所でさまざまな規模の問題が発生する可能性があります。 個々のサーバーやハード ドライブが故障する可能性があります。 デプロイの問題により、データベースのすべてのテーブルが意図せず削除されてしまう可能性があります。 データセンター全体がアクセス不能になる可能性があります。 ランサムウェアのインシデントでは、すべてのデータが悪意を持って暗号化されてしまう可能性があります。

"*可用性*" のための設計では、小規模なインシデント間、また部分的なネットワーク停止などの一時的な条件下で、稼働を維持することに重点を置きます。 アプリケーションの各コンポーネントに高可用性を統合して単一障害点を排除すれば、局所的な障害をアプリケーションで確実に処理できるようになります。 このような設計により、インフラストラクチャのメンテナンスの影響も最小限にします。 通常、可用性の高い設計では、インシデントの影響を迅速かつ自動的に取り除き、影響がほとんどあるいはまったくない状態で要求を処理し続けることができることを目指しています。

*回復性*のための設計では、データの損失や大規模災害からの復旧に重点を置きます。 自動回復の手順によって復旧に必要な時間を短縮できますが、多くの場合、このような種類のインシデントからの復旧にはアクティブな操作が含まれます。 これらの種類のインシデントによって、結果的に多少のダウンタイムが発生したり、データが永久に失われたりする可能性があります。 ディザスター リカバリーには、実行と同じくらい入念な計画が必要です。

アーキテクチャの設計に高可用性と回復性を含めることで、ダウンタイムやデータ損失の結果として生じる金銭的損失から企業を守ります。 顧客からの信頼を失うことによって企業の評判が失墜するのを防ぎます。

## <a name="architecting-for-availability-and-recoverability"></a>可用性と回復性のための設計

可用性と回復性のための設計は、顧客に対するコミットメントをアプリケーションが果たせることを保証します。

可用性については、コミットしているサービス レベル アグリーメント (SLA) を識別します。 アプリケーションの潜在的な高可用性機能を SLA を基準にして確認し、適切なカバレッジがある箇所と改善が必要な箇所を特定します。 目標は、障害が発生する可能性が下がるよう、アーキテクチャのコンポーネントに冗長性を追加することです。 高可用性設計コンポーネントの例には、クラスタリングや負荷分散があります。 クラスタリングでは、単一の VM を協動する VM のセットに置き換えます。 1 つの VM で障害が発生するか VM がアクセス不能になったら、要求を処理できる別の VM にサービスをフェールオーバーできます。 負荷分散は、サービスの多数のインスタンスに要求を分散すると同時に、障害が発生したインスタンスを検出し、それらのインスタンスに要求がルーティングされないようにします。

回復性については、想定されるデータ損失や大規模なダウンタイムのシナリオを検証するための分析を実行します。 分析には、復旧方法の調査や、方法ごとの費用対効果を含める必要があります。 この演習では、組織の優先事項に関する重要な洞察を提供し、アプリケーションの役割を明確にするために役立ちます。 この結果には、アプリケーションの回復ポイントの目標 (RPO) と回復時刻の目標 (RTO) が含まれます。

* **回復ポイントの目標**: 許容されるデータ損失の最大継続時間。 RPO は "30 分のデータ"、"4 時間のデータ" のように、数量ではなく時間の単位で計測されます。 RPO はデータの*損失*の限定や復旧に関連するものであり、データの*盗難*には関連しません。
* **回復時刻の目標**: 許容されるダウンタイムの最大継続時間。"ダウンタイム" は指定内容に従って定義する必要があります。 たとえば、障害のイベントで許容されるダウンタイムの継続時間が 8 時間の場合、RPO は 8 時間です。

RPO と RTO が定義されたら、バックアップ、復元、レプリケーション、回復機能を、これらの目標を達成するためにアーキテクチャの設計に取り入れることができます。

すべてのクラウド プロバイダーは、アプリケーションの可用性と回復性を向上させるために使用できるサービスおよび機能一式を提供します。 可能であれば、既存のサービスやベスト プラクティスを利用し、独自に作成することはできるだけ避けてください。

ハード ドライブの故障、データセンターのアクセス不能、ハッカーの攻撃はどれも可能性があります。 可用性と回復性を備えることで、顧客からの高い評判を維持することが重要です。 可用性はネットワーク停止などの条件下で稼働状態を維持することに重点を置いており、回復性は障害発生後のデータ取得に重点を置いています。
