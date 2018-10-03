多くの場合、最初から始めることが最適です。 まず "サイト リライアビリティ エンジニアリングとは" という基本的な問題から始めましょう。
この問題の周辺にはいくつかの答えがあります。たとえば、この用語を作った人物 (Google の Ben Treynor Sloss) の[よく引用される言葉](https://landing.google.com/sre/book/chapters/introduction.html)などがありますが、私たちが提供できる最も実用的な答えは次のとおりです。

> サイト リライアビリティ エンジニアリングとは、組織がシステム、サービス、および製品で適切なレベルの信頼性を達成するために役立つ専用のエンジニアリング手法です。

今後、この概念に他の定義を持ち込むかもしれませんが、まずはここから始めましょう。 この定義には、明らかにする必要がある 2 つの重要な部分があり、"なぜ重要なのか" という問いにそのまま導かれます。

## <a name="reliability"></a>信頼性

核となる部分 (そして "SRE" という名前にも含まれているもの) は信頼性という言葉です。 定義には、"適切なレベルのパフォーマンス"、"適切なレベルの効率性"、"適切なレベルの安定性"、さらには "適切なレベルの収入を達成する" という言葉は含まれていません。 "適切なレベルの信頼性" という言葉があります。 なぜでしょうか?

簡単なデモを見てみましょう。 スクリーンショットを次に示します。 これは何を示していると思いますか? 考えが浮かぶか諦めるまで次に進まないでください。 注: 下図の詳細な部分がわかりづらくても問題ありません。ブラウザーで適切に表示されます。

   ![スクリーンショット 1](../media/02_blank-screenshot.png)

この図は、PHP アプリでエラーが発生したときにどのようになるかを示すスクリーンショットです (他のデバッグのサポートは追加されていません)。 Java アプリでもこれと似た例があります。

   ![スクリーンショット 2](../media/02_java-screenshot.png)

ここでこのような例を紹介しているのはなぜでしょうか? それぞれ、業務に膨大な時間、エネルギー、リソースがかかる可能性があるアプリケーションを表しています。 ただし、アプリケーションが実行されていない場合 (顧客がアクセスする必要があるときに機能していない場合)、つまり信頼できない場合、だれの得にも (とりわけビジネスの得には) なりません。 実際、信頼性の欠如は、ビジネスに実害 (評判、経済、契約、士気など) を及ぼす可能性があります。

これは信頼性が非常に重要である理由であり、SRE がそれを基本的な特性として、おそらくサービス、システム、または製品の基本的な特性として中心に据えている理由です。 信頼性には多くのことを含まれる可能性があります (後で詳しく説明します) が、定義の 2 つ目の重要な部分に進みましょう。

## <a name="appropriate-levels-of-reliability"></a>適切なレベルの信頼性

初めて定義を読んだときはよくわからなかったかもしれませんが、もう 1 つの重要な言葉を強調してみましょう。

> サイト リライアビリティ エンジニアリングとは、組織がシステム、サービス、および製品で*適切な*レベルの信頼性を達成するために役立つ専用のエンジニアリング手法です。

この言葉が重要な理由は何でしょうか?

SRE の世界で行われている重要な観察は、100% 信頼できるシステムやサービスはほとんど存在しないことです。 航空機、医療機器などの生死の状況は、注目すべき例外です。

実際、望ましい場合でもほとんどありません。 高い信頼性を求めるほど、高い信頼性を達成するために必要な労力とリソースが (それに従ってコストも) 急増します。 言い方を変えると、必要としていない信頼性を求めることは時間とお金の無駄です。 _システム、サービス、製品に適切なレベルの信頼性を達成することを考えます。_ 

レベルは、ビジネス ニーズに合致し、実用的である必要があります。 たとえば、顧客が信頼性が 100% ではない (たとえば、時間の最大 90% の) ネットワークを介して自社と接続している場合、自社のサービスが 95% の信頼性にするように労力とお金をかけることは、定義が示すとおり、時間とお金の無駄です。 _システム、サービス、製品に適切なレベルの信頼性を達成することを考えます。_

SRE はこの実用主義をもう一歩進めます。 望ましいレベルの信頼性があると考えることができたら、会議で成功した場合やそのレベルを上回った場合に、何かすべきことはあるでしょうか? 同様に、達成しなかった場合はどうでしょうか?

チャンネルはそのままで。いろいろな点について後で質疑応答コーナーを設けます。