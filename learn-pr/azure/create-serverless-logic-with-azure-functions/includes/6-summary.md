Azure Functions を使用して、クラウドでビジネス ロジック サービスをホストする方法について学習しました。 この方法はホストされたサービスを、ビジネスを拡大および成長させることができるご自分のソリューションに追加するのに最適です。 好みの言語を使用してコードに集中することができ、インフラストラクチャは Azure によって管理されます。 Functions を、Event Grid、GitHub、Twilio、Microsoft Graph、Logic Apps などの他のサービスと統合することで、複雑で堅牢なサーバーレス ワークフローを迅速かつ簡単に作成することができます。

## <a name="clean-up-your-resources"></a>リソースをクリーンアップする
<!---TODO: Do we need to include cleanup for the free education tier?---> Azure 関数はトリガーされたときにのみ実行されますが、演習で作成したリソースを削除したい場合があります。

1. ご自分の Azure アカウントを使用して [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。

1. 左側のメニューで **[すべてのリソース]** を選んで、**[escalator-functions-group]** を選択して、最初の演習で作成したリソース グループにアクセスします。

1. ツールバーで、**[リソース グループの削除]** ボタンを押します。 削除するリソース グループの名前を入力するように求められます。 完了したら、**[削除]** ボタンを押します。  

![リソース グループの削除](../media-draft/6-cleanup.png)