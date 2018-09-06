Azure サービスを使用して、多彩な機能を備えたサーバーレス アプリケーションを作成できました。

## <a name="clean-up-resources"></a>リソースのクリーンアップ

このアプリケーションの作業が完了したら、次のコマンドを使用して、このチュートリアルで作成したすべてのリソースを削除できます。

```azurecli
az group delete --name first-serverless-app
```

プロンプトが表示されたら 「`y`」と入力します。  

## <a name="next-steps"></a>次の手順

このモジュールでは、次の方法を説明しました。
> [!div class="checklist"]
> * 静的な Web サイトとアップロードされた画像がホストされるように Azure Blob Storage を構成します。
> * Azure Functions を使用して、画像を Azure Blob Storage にアップロードします。
> * Azure Functions を使用して、画像のサイズを変更します。
> * 画像のメタデータを Azure Cosmos DB に格納します。
> * Cognitive Services Vision API を使用して、画像のキャプションを自動生成します。
> * Azure Active Directory を使用してユーザーを認証することにより、Web アプリをセキュリティで保護します。

さらに多くのサービスを Functions に接続する方法については、Logic Apps のチュートリアルに進んでください。 

> [!div class="nextstepaction"]
> [Functions と Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)