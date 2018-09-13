## <a name="intro-to-deep-learning"></a>ディープ ラーニングの概要

Machine Learning ML の目的は、入力データ (画像、タイム シリーズ、オーディオなど) を指定された出力 (たとえばキャプション、価格値、トランスクリプション) に変換するモデルをトレーニングする機能を検索することです。 従来のデータ サイエンスでは、機能は多くの場合、慎重に選択されます。

![フィード フォワード ディープ ニューラル ネットワークの Canonical 例です。](../media/2-image1.PNG)

ディープ ラーニング (DL) では、入力をベクトルとして表し、これを一連の適切な線形代数操作によって指定された出力に変換することで機能の抽出プロセスを学習します。  

次にモデル出力が、損失関数と呼ばれる式を使用して、想定される出力と比較されます。 各トレーニング入力の損失関数によって返される値は、次のパス上で下位の損失となる機能を抽出するようにモデルをガイドするために使用されます。  
 
線形代数コンポーネントの一部として計算する一連の行列演算は、計算上コストの高い傾向があり、並列処理が極めて高いことが多く、効率的に計算するにはグラフィックス処理ユニット (GPU) などの特殊な計算を必要とします。

## <a name="data-science-virtual-machine"></a>Data Science Virtual Machine

![Data Science Virtual Machine のオプション](../media/2-image2.PNG)

データ サイエンス VM は、データ分析、機械学習、ディープ ラーニング トレーニングでよく使用される一般的なツールを使用して事前インストール、構成、テストされた Azure 仮想マシン イメージです。

次を提供します。

- チーム全体で一貫した設定で共有とコラボレーション、Azure スケールと管理、ほとんど不要なセットアップ、データ サイエンス用の完全クラウド ベースのデスクトップを推進。
- オンデマンドでのエラスティックな容量で、垂直方向および水平方向のスケーリングによってすべての Azure ハードウェア構成の分析を実行する機能。 料金は使用した量、使用したときのみ課金。
- ディープ ラーニング ツールが既に事前構成されているすぐに利用可能な GPU クラスターによるディープ ラーニング。 

データ サイエンス VM には、Microsoft R Server Developer Edition、Anaconda Python、Python および R 用 Jupyter ノートブック、Python および R 用 IDE、SQL データベース、その他多くのデータ サイエンスと ML ツールなどの、ディープ ラーニング フレームワークとツールの一般的な GPU エディションを含む AI 用の複数のツールが含まれています。

Data Science Virtual Machine は、Azure の GPU の NC シリーズ VM インスタンスで実行できます。 これらの GPU では個別のデバイスの割り当てが使用され、ベア メタルに近いパフォーマンスが実現され、ディープ ラーニングの問題に適しています。

<!--### Quiz? 

What is the goal of machine learning? 
How is traditional machine learning different from deep learning? 
Why are GPU's often used for deep learning? 
What does the DSVM provide? -->
