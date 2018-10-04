このモジュールの最後のユニットでは、SRE の探索に関心がある方向けに今後のお勧めの情報について話します。 

## <a name="reading-and-watching"></a>ドキュメントとビデオ

SRE の詳細について最適な情報源は、この主題について書かれた次の 3 冊の書籍です

1. [_SRE サイトリライアビリティエンジニアリング―Googleの信頼性を支えるエンジニアリングチーム_](http://shop.oreilly.com/product/0636920041528.do)(「The SRE Book」(SRE ブック) と呼ばれます)
1. [_The Site Reliability Workbook: Practical Ways to Implement SRE_](http://shop.oreilly.com/product/0636920132448.do) (サイト信頼性ワークブック: 実用的な SRE の実装方法) (「The SRE Workbook」(SRE ワークブック) と呼ばれます)
1. [_Seeking SRE: Conversations About Running Production Systems at Scale_](http://shop.oreilly.com/product/0636920063964.do) (SRE の探求: 大規模な運用システムの稼働に関する会話)

(簡単に説明すると、このモジュールの主要作成者は、この 3 冊目のキュレーター/編集者です)

これらの各書籍には、重要な情報が記載されています。

- SRE ブック: Google が長年かけて SRE をどのように実装してきたかが詳細に説明されています。

- SRE ワークブック - SRE ブックと対になる書。Google や他の場所で SRE が "何" であるかだけでなく、"方法" や "理由" についてもより詳しく説明されています。

- Seeking SRE: 他の環境で実装されている方法など、SRE の原点を超えて SRE の世界についてより広範な視点が提供されています。

3 冊のいずれも必ず批評的な視点で読んでください。 これらの本に書かれたすべての内容が自分と組織に適用されるわけではありません。 ある程度時間をかけて、何らかの評価ができる価値があると確信できる情報を特定してください。 記載されているとおり、SRE の業務をサポートできる組織のカルチャと価値の部分と、より困難になりそうな部分を考えてましょう。

映像の方が好みの場合は、SREcon14 カンファレンスで Ben Treynor が行ったトークの「[Keys to SRE](https://www.usenix.org/conference/srecon14/technical-sessions/presentation/keys-sre)」(SRE の要点) をご覧ください。 Treynor は、(少なくとも Google の文脈で) SRE が何であるかについて自分が考えていることを実に適切に説明しています。 [このカンファレンス シリーズ](https://www.usenix.org/conferences/byname/925)などの SRE について話された他のトークのビデオも本当に参考になります。

## <a name="talk-to-other-interested-people"></a>他の関心を持っている人に話す

SRE について読むことも重要ですが、他の人に話すことの方が重要なことはよくあります。 SRE に関する自分の課題、成功、失敗について話すことは、その主題について微妙なところまで理解するために不可欠です。 

SRE コンテンツを特集している会議やカンファレンスは多数あります。 おそらく最も直接的に関連しているものは、USENIX が世界中で開催している [SREcon カンファレンス](https://www.usenix.org/conferences/byname/925)です (免責事項: このモジュールの主要作成者は SREcon の共同創始者の 1 人です)。

ますます多くの SRE コンテンツが、[Velocity](https://conferences.oreilly.com/velocity)、[LISA](https://www.usenix.org/conferences/byname/5) などのカンファレンスや、[DevOps Days](https://www.devopsdays.org) などの地方の DevOps カンファレンスで取り上げられるようになっています。 どこであっても、このコンテンツやその主題に関心を持っている人を探してみてください。

## <a name="first-steps-at-work"></a>職場での第一歩

環境に SRE を取り込みたいものを探り始めるなら、SRE は "すべてか無か" の命題ではない点を忘れないことが重要です。  SRE の原則とプラクティスは少しずつ採用することから始められます。

Mike Dickerson は、その後 United States Digital Service となる (healthcare.gov を救う業務を請け負っていた) 職場での業務で有名になった SRE です。彼は、マズローの欲求の階層に敬意を示し、信頼性の階層を提案しています。 これについては最初の SRE ブックの[「Practices」(プラクティス) セクション](https://landing.google.com/sre/book/chapters/part3.html)で引用されています。

この階層では、まず環境が機能し、信頼できることを監視する必要があると提案しています。 皆さんの環境の場合にも、これを SRE に向かう第一歩にする必要があります。 対象を測定できる場合、それが信頼できるかどうか (または好転しているか悪化しているか) を判断できます。

信頼できる監視プラットフォームを用意できたら、次に進む段階は、職場でサービスを選択し、SLI や SLO について話し始めることです。 簡単なところから始めましょう。 サービスの SLI と SLO を作成し、監視システムに実装し、SRE のレンズを通して信頼性に注意を向けたときに何が起こるかを確認します。 ここから始めることをお勧めします。
