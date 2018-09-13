ボットを作成する最初のステップは、ボットが Azure でホストされる場所を指定することです。 Azure App Service の [Web Apps](https://azure.microsoft.com/services/app-service/web/) 機能はボット アプリケーションをホストするのに最適であり、Azure Bot Service はそれを自動的にプロビジョニングするように設計されています。 この演習では、Azure portal を使用して Azure Web アプリ ボットをプロビジョニングします。

1. ブラウザーで [Azure portal](https://portal.azure.com/?azure-portal=true) を開きます。 サインインを求められたら、Microsoft アカウントを使用してサインインします。

1. **[+ リソースの作成]**、**[AI + Machine Learning]**、**[Web App Bot]** の順にクリックします。
 
    ![新しい Azure Web アプリ ボットの作成](../images/new-bot-service.png)

    "_新しい Azure Web アプリ ボットの作成_"
  
1. **[アプリ名]** ボックスに "qa-factbot" などの名前を入力します。 "*この名前は Azure 内で一意である必要があるため、名前の横に緑色のチェック マークが表示されることを確認してください。*" **[リソース グループ]** の **[新規作成]** を選択し、リソース グループ名として「factbot-rg」 と入力します。 最寄りの場所を選択し、無料の **F0** 価格レベルを選択します。 次に、**[Bot template]\(ボット テンプレート\)** をクリックします。

    ![Azure Web アプリ ボットの構成](../images/portal-start-bot-creation.png)

    "_Azure Web アプリ ボットの構成_"

1. SDK 言語として **[Node.js]** を選択し、テンプレートの種類として **[Question and Answer]\(質問と答え\)** を選択します。 次に、ブレードの下部にある **[選択]** をクリックします。   
  
    ![言語とテンプレートの選択](../images/portal-select-template.png)

    "_言語とテンプレートの選択_"

1. **[App Service プラン/場所]**、**[新規作成]** の順にクリックし、手順 3. で選択したのと同じリージョンに "qa-factbot-service-plan" のような名前で App Service プランを作成します。 終わったら、[Web App Bot] ブレードの下部にある **[作成]** をクリックして展開を開始します。 

1. ポータルの左側のリボンで **[リソース グループ]** をクリックします。 **[factbot-rg]** をクリックして、Azure Web アプリ ボット用に作成したリソース グループを開きます。 ブレードの上部の "デプロイ中" が、Azure Web アプリ ボットが正常にデプロイされたことを示す "成功" に変わるまで待ちます。 通常、デプロイに必要な時間は 2 分以内です。 ブレードの上部にある **[更新]** をときどきクリックして、デプロイの状態を更新します。

    ![デプロイに成功](../images/deployment-succeeded.png)

    "_デプロイに成功_"
  
Azure Web アプリ ボットをデプロイするときは、見えないところで多くのことが行われています。 ボットが作成されて登録され、それをホストする [Azure Web App](https://azure.microsoft.com/services/app-service/web/) が作成されて、[Microsoft QnA Maker](https://www.qnamaker.ai/) で動作するようにボットが構成されました。 次の手順では、QnA Maker を使用して、ボットにインテリジェンスを取り入れるための質問と回答のナレッジ ベースを作成します。