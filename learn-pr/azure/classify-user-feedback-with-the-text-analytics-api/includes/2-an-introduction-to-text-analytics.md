どの企業も自社のブランド、製品、メッセージについて、顧客がどのように考えているかを知りたいと思っています。 顧客の意見は時間とともにどのように変わるのでしょうか。 顧客の書き込みでセンチメントを探ることで、手掛かりを見つけることができます。 感情分析は、"*顧客が本当に求めているものは何か?*" という問いの答えを見つけるのに役立ちます。 これは、ツイートや他のソーシャル メディア コンテンツ、顧客レビュー、電子メールの分析に使用されます。

![テキストから抽出されたセンチメントは、否定的から肯定的の尺度で表示されます。](../media/sentiment-analysis.png)

 感情分析の一般的なアプローチは、センチメントを検出する機械学習モデルをトレーニングすることです。 ただし、そのプロセスは複雑です。 ラベル付けされた良質のトレーニング データを用意し、そのデータから特徴を作成します。さらに、分類子をトレーニングし、その分類子を使用して新しいテキストのセンチメントを予測します。 すべての企業に、AI ソリューションのゼロからの構築に投資するための資金や専門知識があるわけではありません。 ただし、Microsoft などの企業は、これらの分野の最先端の研究に投資することができ、実際に投資しています。 開発者は、これらの企業が出荷する API、SDK、プラットフォームを使用して、その結果からメリットが得られます。 Microsoft Cognitive Services は、このようなサービスの 1 つです。

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Microsoft Cognitive Services は、一連の API、SDK とサービスで構成されています。 その目的は、開発者がユーザーの目を引く、よりインテリジェントで魅力的なアプリを作成できるよう支援することです。

Azure Cognitive Services は、視覚、音声、言語、知識、検索のインテリジェント アルゴリズムを提供します。 提供されているサービスについては、[Cognitive Services のディレクトリ](https://azure.microsoft.com/services/cognitive-services/directory/)をご覧ください。 各サービスは無料で試すことができます。 これらの 1 つ以上のサービスをアプリケーションに統合する場合は、有料のサブスクリプションにサインアップします。 このモジュール全体を通じて使用するサービスは Text Analytics API であるため、このサービスの詳細を確認しましょう。

## <a name="text-analytics-api"></a>Text Analytics API

Text Analytics API は、テキストからの情報の抽出を支援することを目的とした Cognitive Service です。 このサービスを使用して、言語の特定、センチメントの検出、キー フレーズの抽出、テキストからの既知のエンティティの検出を行うことができます。 

このレッスンでは、この API の感情分析の部分について説明します。 サービスでは、内部で機械学習分類アルゴリズムを使用して、0 から 1 のセンチメント スコアを生成します。 1 に近いスコアは肯定的センチメントを示し、0 に近いスコアは否定的センチメントを示します。 0.5 に近いスコアは、センチメントの欠如、つまり中立的な意見を示します。 アルゴリズムの実装の詳細を気にする必要はありません。 アプリからサービスを呼び出して、サービスの利用に注力できます。 この後説明しますが、**POST** 要求を作成して `/sentiment` エンドポイントに送信し、"*センチメント スコア*" を示す JSON 応答を受け取ります。

まず、オンライン API テスト コンソールを使用して、Text Analytics API を試してみます。 API に慣れたら、メッセージを分類してさらに処理できるように、メッセージのセンチメントを検出するシナリオで使用します。