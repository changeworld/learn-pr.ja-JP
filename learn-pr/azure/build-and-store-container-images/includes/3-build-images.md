会社ではコンテナー イメージを使用して、コンピューティング ワークロードを管理しています。 ローカルの Docker ツールを使用して、ご自分のコンテナー イメージをビルドします。

Azure Container Registry Tasks を使用することで、これらのコンテナーをビルドできるようになりました。 また、Container Registry Tasks では、ソース コードのコミット時に DevOps プロセスと自動化されたビルドを統合できます。

Azure Container Registry Tasks を使用してコンテナー イメージの作成を自動化してみましょう。

## <a name="create-a-container-image-with-azure-container-registry-tasks"></a>Azure Container Registry Tasks を使用してコンテナー イメージを作成する

標準の Dockerfile にはビルド手順が提供されています。 Azure Container Registry Tasks では、ご利用の環境に現在ある任意の Dockerfile を再利用することができます。

この例では、新しい Dockerfile を使用します。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

最初の手順として、`Dockerfile` という名前の新しいファイルを作成します。 任意のテキスト エディターを使用して、ファイルを編集することができます。 この例では、Cloud Shell エディターを使用します。 次のコマンドを Cloud Shell ウィンドウに貼り付けてエディターを開きます。

```bash
code
```

次のコンテンツを新しい Dockerfile にコピーします。 必ずファイルを保存します。

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<kbd>Ctrl+S</kbd> キーの組み合わせを使用して保存します。 メッセージが表示されたら、ファイルに `Dockerfile` という名前を付けます。

この構成では、Node.js アプリケーションが `node:9-alpine` イメージに追加されます。 次に、ポート 80 上で *EXPOSE* 命令を介してアプリケーションを提供できるようにコンテナーを構成します。

次に、Azure CLI コマンド `az acr build` を実行して、Dockerfile からコンテナー イメージをビルドします。

```azurecli
az acr build --registry <acrName> --image helloacrtasks:v1 .
```

このコマンドを実行すると、ビルドされてご利用の Container Registry にプッシュされるイメージが表示されます。

## <a name="verify-the-image"></a>イメージを確認する

次のコマンドを実行して、イメージが作成されてレジストリに格納されたことを確認します。

```azurecli
az acr repository list --name <acrName> --output table
```

出力は次のようになります。

```console
Result
-------------
helloacrtasks
```

これで、`helloacrbuild` イメージが使用できる状態になりました。
