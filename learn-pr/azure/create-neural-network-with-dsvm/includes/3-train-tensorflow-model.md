### <a name="train-a-tensorflow-model"></a>TensorFlow モデルをトレーニングする

このユニットでは、[TensorFlow](https://www.tensorflow.org/) を使用してビルドされた画像分類モデルを、ホットドッグが入った画像を認識するようにトレーニングします。 モデルをゼロから作成すると、膨大な量のコンピューティング能力と数十万もの画像が必要になるため、[転移学習](https://en.wikipedia.org/wiki/Transfer_learning)として知られている既存のモデルをカスタマイズします。 転移学習を使用すると、一般的なノート パソコンや PC で数十個のイメージだけを使用して、わずか数分間で高レベルの精度を実現できます。

ディープ ラーニングとの関係において、転移学習は、イメージの分類と、問題のドメインのネットワークをカスタマイズするレイヤーの追加 (たとえばイメージにホットドッグが含まれているか含まれていないかで 2 つのグループに分類する) を実行するように事前にトレーニングされたディープ ニューラル ネットワークから始まります。 <https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models.> では、20 を超える事前トレーニング済みの TensorFlow イメージ分類モデルを入手できます。[Inception](https://arxiv.org/abs/1512.00567) と [ResNet](https://towardsdatascience.com/an-overview-of-resnet-and-its-variants-5281e2f56035) モデルが高い精度とそれに応じた高いリソース要件によって特徴付けられているのに対し、MobileNet モデルは精度と引き換えにコンパクトさと電源効率が高くなっており、モバイル デバイスでの使用を考慮して開発されました。 これらのモデルはいずれもディープ ラーニング コミュニティではよく知られており、多くのコンペティションや実際のアプリケーションで使用されています。 ここでは、精度とトレーニング時間の妥当なバランスを取るため、MobileNet モデルのうちの 1 つをニューラル ネットワークの基礎として使用します。

モデルのトレーニングは、基本モデルをダウンロードして、ドメイン固有のイメージとラベルを使ってトレーニングされたレイヤーを追加する Python スクリプトを実行するだけです。 必要なスクリプトは GitHub で入手できます。使用するイメージは、[Kaggle](https://www.kaggle.com) から入手可能なパブリック ドメインの数千の食品イメージから集められました。

1. Data Science VM で、画面の下部にあるターミナル アイコンをクリックして、ターミナル ウィンドウを開きます。

    ![ターミナル ウィンドウを起動する](../media-draft/3-launch-terminal.png)

    _"ターミナル ウィンドウを起動する"_

1. ターミナル ウィンドウで、次のコマンドを実行して、"notebooks" フォルダーに移動します。

    ```bash
    cd notebooks
    ```
    このフォルダーには、DSVM 用に収集されたサンプルの Jupyter ノートブックがあらかじめ入っています。

1. 次のコマンドを使用して GitHub から "TensorFlow for Poets" リポジトリを複製します。

    ```bash
    git clone https://github.com/googlecodelabs/tensorflow-for-poets-2
    ```
    > **ヒント**: クリップボードにこの行をコピーして、**Shift + Ins** を使用してターミナル ウィンドウに貼り付けることができます。

    このリポジトリには、転移学習モデルを作成するためのスクリプトや、イメージを分類するためにトレーニング済みモデルを呼び出すスクリプトなどが含まれています。 これは [Google Codelabs](https://codelabs.developers.google.com/) の一部です。Google Codelabs には、TensorFlow やその他の Google のツールおよび API について関心のあるソフトウェア開発者向けのさまざまなリソースとハンズオン ラボが含まれています。

1. 複製が完了したら、複製したモデルを含むフォルダーに移動します。

    ```bash
    cd tensorflow-for-poets-2
    ```

1. 次のコマンドを使用して、モデルのトレーニングに使用するイメージをダウンロードします。

    ```bash
    wget https://topcs.blob.core.windows.net/public/tensorflow-resources.zip -O temp.zip; unzip temp.zip -d tf_files; rm temp.zip
    ```

    このコマンドにより、数百の食品イメージ (このうちの半分にはホットドッグが入っており、もう半分には入っていません) を含む zip ファイルがダウンロードされ、それらが "tf_files" という名前のサブディレクトリにコピーされます。

1. 画面の下部にある [ファイル マネージャー] アイコンをクリックして、[ファイル マネージャー] ウィンドウを開きます。

    ![ファイル マネージャーを起動する](../media-draft/3-launch-file-manager.png)

    _"ファイル マネージャーを起動する"_

1. ファイル マネージャーで、"notebooks/tensorflow-for-poets-2/tf_files" フォルダーに移動します。 フォルダーに "hot_dog" と "not_hot_dog" という名前のサブディレクトリのペアが含まれていることを確認します。 前者にはホットドッグが入った数百のイメージが格納されており、後者にはホットドッグが**入っていない**同じ数のイメージが格納されています。 "hot_dog" フォルダー内のイメージを参照して、その見た目の感触をつかみます。 "Not_hot_dog" フォルダー内のイメージも確認します。

    > イメージにホットドッグが含まれているかどうかをニューラル ネットワークが判断できるように、ホットドッグが含まれているイメージと含まれていないイメージを使ってトレーニングします。

    !["hot_dog" フォルダー内のイメージ](../media-draft/3-hot-dog-images.png)

    *"hot_dog" フォルダー内の画像*

    また、**retrained_labels_hotdog.txt** という名前のテキスト ファイルがフォルダーに含まれていることを確認します。 このファイルは、トレーニング画像が含まれているサブディレクトリを識別します。 これはモデルをトレーニングする Python スクリプトによって使用されます。 スクリプトでは、テキスト ファイル (テキスト ファイルの名前は、スクリプトに渡されるパラメーター) で識別された各サブディレクトリ内のファイルを列挙し、ネットワークのトレーニングにそれらのファイルを使用します。

1. 2 番目のターミナル ウィンドウを開き、"notebooks/tensorflow-for-poets-2" フォルダー (最初のターミナル ウィンドウで開いたのと同じフォルダー) に移動します。 次に、次のコマンドを使用して [TensorBoard](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard) を起動します。これは、TensorFlow モデルを視覚化し、転移学習プロセスの分析情報を得るためのツール セットです。

     ```bash
     tensorboard --logdir tf_files/training_summaries
     ```

     > TensorBoard のインスタンスが既に実行されている場合には、このコマンドは失敗します。 ポート 6006 が既に使用されていることがわかっている場合には、```pkill -f "tensorboard"``` コマンドを使用して既存のプロセスを強制終了します。 次に、```tensorboard``` コマンドをもう一度実行します。

1. 元のターミナル ウィンドウに戻り、次のコマンドを実行します。

    ```bash
    IMAGE_SIZE=224;
    ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}";
    ```

    これらのコマンドにより、トレーニング イメージの解像度と、ニューラル ネットワークの基礎となる基本モデルを指定する環境変数が初期化されます。 IMAGE_SIZE の有効な値は、128、160、192、224 です。 値が大きいほどトレーニング時間は長くなりますが、分類の精度も高まります。

1. 次に、次のコマンドを実行して、転移学習プロセスを開始します。つまり、ダウンロードしたイメージを使って、モデルをトレーニングします。

    ```bash
    python scripts/retrain.py \
    --bottleneck_dir=tf_files/bottlenecks \
    --how_many_training_steps=500 \
    --model_dir=tf_files/models/ \
    --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
    --output_graph=tf_files/retrained_graph_hotdog.pb \
    --output_labels=tf_files/retrained_labels_hotdog.txt \
    --architecture="${ARCHITECTURE}" \
    --image_dir=tf_files \
    --testing_percentage=15 \
    --validation_percentage=15
    ```

    **retrain.py** は、ダウンロードしたリポジトリ内のスクリプトの 1 つです。 これは複雑で、1,000 行を超えるコードとコメントで構成されています。 そのジョブは、```--architecture``` スイッチで指定されたモデルをダウンロードして、それを ```--image_dir``` スイッチで指定されたディレクトリのサブディレクトリにあるイメージを使用してトレーニングされた新しい層に追加することです。 各イメージは、それぞれが格納されているサブディレクトリの名前 (この場合は、"hot_dog" または "not_hot_dog" のいずれか) でラベル付けされ、変更されたニューラル ネットワークがイメージ入力をホットドッグのイメージ ("hot_dog") またはホットドッグなしのイメージ ("not_hot_dog") に分類できるようにします。 トレーニング セッションからの出力は、**retrained_graph_hotdog.pb** という名前の TensorFlow モデル ファイルです。 名前と場所は、```--output_graph``` スイッチで指定されます。

1. トレーニングが完了するまで待ちます。通常は 5 分未満で完了します。 次に、出力を確認して、モデルの精度を判断します。 トレーニング プロセスには多少の推定が入っているため、実際の結果は以下と多少異なる場合があります。

      ![モデルの精度を計測する](../media-draft/3-running-transfer-learning.png)

1. デスクトップの下部にあるブラウザー アイコンをクリックして、Data Science VM にインストールされているブラウザーを開きます。 次に、<http://0.0.0.0:6006> に移動して Tensorboard に接続します。

    ![Firefox を起動する](../media-draft/3-launch-firefox.png)

1. "accuracy_1" というラベルが付いたグラフを調べます。 青い線は、```how_many_training_steps``` スイッチで指定された 500 のトレーニング ステップを経て達成された精度を示しています。 このメトリックは、トレーニングの進行とともにモデルの精度がどのように進化しているかを示すため、重要です。 同じくらい重要なのは、青の線とオレンジの線の間の距離です。これは、発生したオーバーフィットの量を数値化したもので、常に最小限に抑える必要があります。 [オーバーフィット](https://en.wikipedia.org/wiki/Overfitting)は、モデルが、トレーニングに使用したイメージはうまく分類できるようになったが、示された他のイメージはそれほどうまく分類できないことを意味します。 ここでの結果は、オレンジの線 (トレーニング イメージを使って達成された "トレーニング" 精度) と青の線 (トレーニング セット以外のイメージを使ってテストした場合に達成された "検証" 精度) との差が 10% 未満なので、許容範囲内です。

    ![TensorBoard スカラーの表示](../media-draft/3-tensorboard-scalars.png)

1. TensorBoard のメニューで **[グラフ]** をクリックして、表示されるグラフを調べます。 このグラフの主な目的は、ニューラル ネットワークとそれを構成するレイヤーを示すことです。 この例では、"input_1" が食品イメージを使用してトレーニングされ、ネットワークに追加されたレイヤーです。 "MobilenetV1" は、最初に使用する基本のニューラル ネットワークです。 これには、示されていない多くのレイヤーが含まれています。 ゼロからディープ ニューラル ネットワークを構築した場合は、すべてのレイヤーがここにダイアグラム化されます。 (MobileNet を構成するレイヤーを表示する場合は、ダイアグラム内の "MobilenetV1" ブロックをダブルクリックします。)グラフの表示に関する情報とグラフに表示される情報については、「[TensorBoard: Graph Visualization](https://www.tensorflow.org/programmers_guide/graph_viz)」 (TensorBoard: グラフの視覚化) を参照してください。

    ![TensorBoard グラフの表示](../media-draft/3-tensorboard-graphs.png)

1. ファイル マネージャーに戻って、"notebooks/tensorflow-for-poets-2/tf_files" フォルダーに移動します。 **retrained_graph_hotdog.pb** という名前のファイルが含まれていることを確認します。 *このファイルは、トレーニング プロセス中に作成され、トレーニング済みの TensorFlow モデルが含まれています*。 次の演習では、これを使用して、NotHotDog アプリからモデルを呼び出します。

手順 10 で実行したスクリプトでは、精度とトレーニングに必要な時間とのバランスを取るため、500 のトレーニング ステップを指定しました。 必要に応じて、```how_many_training_steps``` の値を 1000 または 2000 などに増やして、もう一度モデルのトレーニングを試してください。 通常は、ステップ数が多くなるほど精度が高まりますが、引き換えにトレーニング時間が長くなります。 TensorBoard のスカラー表示でオレンジの線と青の線との差で示される、オーバー フィットに注意してください。