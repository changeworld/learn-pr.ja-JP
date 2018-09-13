このユニットでは、Artworks アプリを使用して、分類のために画像を Custom Vision Service に送信します。 このアプリは、Custom Vision Prediction API の [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) メソッドへの呼び出しから返された JSON 情報を使用して、画像がピカソ、レンブラント、ポロック、またはそのいずれでもない人物による絵画を表しているかどうかを示します。 また、イメージに割り当てられた分類が正しい確率も示します。

1. Artworks アプリで **[参照(...)]** ボタンをクリックします。 

    ![Artworks アプリでローカル画像を参照する](../media-draft/6-app-click-browse.png)

1. モジュール リソースの "Quick Tests" フォルダーを参照します。 **PicassoTest_02.jpg** という名前のファイルを選択し、**[開く]** をクリックします。

1. **[予測]** ボタンをクリックして、イメージを Custom Vision Service に送信します。

    ![画像を Custom Vision Service に送信する](../media-draft/6-app-click-predict.png)

1. アプリでその絵がピカソの作品として識別されることを確認します。

    ![画像をピカソとして分類する](../media-draft/6-app-prediction-01.png)

1. **RembrandtTest_01.jpg** と **PollockTest_01.jpg** に対して手順 1. から 3. を繰り返し、アプリでレンブラントおよびポロックの絵画が識別できることを確認します。

    ![画像をレンブラントとして分類する](../media-draft/6-app-prediction-02.png)

1. **VanGoghTest_01.png** と **VanGoghTest_02.png** に対して手順 1. から 3. を繰り返し、アプリでヴァン・ゴッホのこれらの名画がピカソやレンブラント、ポロックの絵画として識別されないことを確認します。

    ![ピカソでもレンブラントでもポロックでもない](../media-draft/6-app-prediction-03.png)

1. ご覧のように、アプリから Prediction API を使用すると、Custom Vision Service ポータルを使用する場合と同じくらいの信頼性が得られ、はるかに楽しく行えます。 さらに、ポータルの予測ページに移動すると、アプリを介してアップロードした各画像もそこに表示されます。

    ![Custom Vision Service に送信された画像](../media-draft/6-portal-all-predictions.png)

自由にご自身の画像を使用してテストしたり、アーティストの識別や、画像がピカソ、レンブラント、またはポロックの作品ではないことの識別についてモデルの熟練度を測定したりしてみましょう。 ゴッホも認識するようにトレーニングする場合は、ゴッホの絵画をいくつかアップロードして、それらに "Van Gogh" (ヴァン ゴッホ) とタグ付け、モデルを再トレーニングします。 トレーニングの手間をいとわなければ、追加できるインテリジェンスに制限はありません。 一般的に、トレーニングするイメージが多いほどモデルはスマートになります。