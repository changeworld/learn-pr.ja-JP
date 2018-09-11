このユニットでは、ボットとそれに関連付けられているすべてのリソースを含むリソース グループを削除します。 リソース グループを削除すると、グループ内のすべてのものが削除されるため、今後料金が発生することを防ぐことができます。 削除したリソース グループを復旧することはできないため、必ずグループの使用が終了していることを確認してから、削除してください。

<!---TODO: Do we need cleanup instructions for free education access?--->

1. "factbot-rg" リソース グループのブレードに戻ります。 次に、ブレードの上部にある **[リソース グループの削除]** をクリックします。

    ![リソース グループを削除する](../media-draft/9-delete-resource-group.png)

    _リソース グループを削除する_

1. 安全のため、リソース グループの名前を入力する必要があります。 (リソース グループは、削除すると復旧することはできません。)リソース グループの名前を入力します。 次に、**[削除]** ボタンをクリックして、この演習のすべてのトレースを Azure サブスクリプションから削除します。

数分後、リソース グループとそのリソースのすべてが削除されます。 **[削除]** クリックすると課金が停止するため、リソースを削除するために必要な時間に対しては課金されません。 同様に、リソースが完全かつ正常にデプロイされるまで、課金は発生しません。

### <a name="summary"></a>まとめ

[ダイアログ](http://aihelpwebsite.com/Blog/EntryId/9/Introduction-To-Using-Dialogs-With-The-Microsoft-Bot-Framework)、[FormFlow](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/07/14/building-a-microsoft-bot-using-microsoft-bot-framework-using-formflow/)、[Microsoft Language Understanding Intelligence Services (LUIS)](https://docs.botframework.com/node/builder/guides/understanding-natural-language/) を組み込むと、Azure Bot Service の機能を活用するためにできることがさらに増えます。 これらの機能により、ユーザーのクエリとコマンドに応答し、滑らかな会話型の非線形の方法で対話する高度なボットをビルドすることができます。 詳細および開始するためのアイデアについては、「[What is Microsoft Bot Framework Overview](https://blogs.msdn.microsoft.com/uk_faculty_connection/2016/04/05/what-is-microsoft-bot-framework-overview/)」 (Microsoft Bot Framework の概要) を参照してください。