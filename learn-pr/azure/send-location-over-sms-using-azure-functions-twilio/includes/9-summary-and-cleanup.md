Xamarin および Azure 関数と Twilio バインディングを使用してクロスプラットフォーム モバイル アプリを正しく作成できました。

## <a name="clean-up"></a>クリーンアップ
<!---TODO: Update for sandbox?--->

この Azure Functions アプリケーションの作業が終わったら、チュートリアルの間に作成したすべてのリソースを、Azure portal を使用して削除できます。

1. Visual Studio で、*[表示] > [Cloud Explorer]* を選択します。

1. このパネルの上部にあるドロップダウン リストから、*[リソース グループ]* を選択します。

1. リソース グループの作成に使用したサブスクリプションを展開します。 "ImHere" リソース グループを右クリックし、*[ポータルで開く]* を選択します。

    ![Cloud Explorer ウィンドウからポータルでリソース グループを開く](../media-drafts/9-open-resource-group-in-portal.png)

1. 必要な場合は、ブラウザーで Azure portal にログインします。

1. ポータルで "ImHere" リソース グループが開きます。 **[リソース グループの削除]** ボタンをクリックします。

    ![リソース グループを削除します](../media-drafts/9-delete-resource-group.png)

1. リソース グループの名前を入力して削除を確認し、**[削除]** をクリックします。

    ![リソース グループの名前を入力して、削除を確定します。](../media-drafts/9-confirm-delete-resource-group.png)

## <a name="summary"></a>まとめ

このモジュールでは、次の方法を説明しました。

- Xamarin.Essentials を使用するクロスプラットフォームの Xamarin.Forms アプリを作成しました。
- ViewModel のアプリケーション ロジックを含む XAML を使用してクロスプラットフォームの UI を作成し、UI に ViewModel のプロパティをバインドしました。
- ユーザーの場所を検出しました。
- HTTP トリガーを含む Azure 関数を作成し、ローカルで実行しました。
- モバイル アプリから Azure 関数を呼び出し、JSON としてデータを渡しました。
- Azure 関数を Twilio にバインドして SMS メッセージを送信しました。
- Azure 関数を Azure に発行しました。