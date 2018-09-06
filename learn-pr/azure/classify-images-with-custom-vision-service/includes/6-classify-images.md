### <a name="exercise-6-use-the-app-to-classify-images"></a>演習 6: アプリを使用してイメージを分類する

この演習では、Artworks アプリを使用して、イメージを分類のために Custom Vision Service に送信します。 このアプリは、Custom Vision Prediction API の [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) メソッドへの呼び出しから返された JSON 情報を使用して、イメージがピカソ、レンブラント、ポロック、またはそのいずれでもない人物による絵画を表しているかどうかを示します。 また、イメージに割り当てられた分類が正しい確率も示します。

1. Artworks アプリで **[参照(...)]** ボタンをクリックします。 

    ![Artworks アプリでローカル イメージを参照する](../images/app-click-browse.png)

    "Artworks アプリでローカル イメージを参照する"__ 

1. ラボ リソースの "Quick Tests" フォルダーを参照します。 **PicassoTest_02.jpg** という名前のファイルを選択し、**[開く]** をクリックします。

1. **[予測]** ボタンをクリックして、イメージを Custom Vision Service に送信します。

    ![イメージを Custom Vision Service に送信する](../images/app-click-predict.png)

    "イメージを Custom Vision Service に送信する"__ 

1. アプリでその絵がピカソの作品として識別されることを確認します。

    ![イメージをピカソとして分類する](../images/app-prediction-01.png)

    "イメージをピカソとして分類する"__ 

1. **RembrandtTest_01.jpg** と **PollockTest_01.jpg** に対して手順 1 から 3 を繰り返して、アプリがレンブラントおよびポロックの絵画を識別できることを確認します。

    ![イメージをレンブラントとして分類する](../images/app-prediction-02.png)

    "イメージをレンブラントとして分類する"__ 

1. **VanGoghTest_01.png** と **VanGoghTest_02.png** に対して手順 1 から 3 を繰り返して、アプリがゴッホのこれらの名画をピカソやレンブラント、ポロックの絵画として識別しないことを確認します。

    ![ピカソでもレンブラントでもポロックでもない](../images/app-prediction-03.png)

    "ピカソでもレンブラントでもポロックでもない"__ 

1. ご覧のように、アプリから Prediction API を使用すると、Custom Vision Service ポータルを使用する場合と同じくらいの信頼性が得られ、より楽しく行えます。 さらに、ポータルの予測ページに移動すると、アプリを使用してアップロードされた各イメージもそこに表示されます。
 
    ![Custom Vision Service に送信されたイメージ](../images/portal-all-predictions.png)

    "Custom Vision Service に送信されたイメージ"__ 

ご自分のイメージを使用して、アーティストを識別したり、イメージがピカソ、レンブラント、またはポロックの作品ではないことの識別についてモデルの熟練度を自由にテストしたり、測定したりしてみましょう。 ゴッホも認識するようにトレーニングする場合は、ゴッホの絵画をいくつかアップロードして、それらに "Van Gogh" (ヴァン ゴッホ) とタグ付けして、モデルを再トレーニングします。 トレーニングの手間をいとわなければ、追加できるインテリジェンスに制限はありません。 一般的に、トレーニングするイメージが多いほどモデルはスマートになります。