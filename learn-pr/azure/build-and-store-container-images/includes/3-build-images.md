会社ではコンテナー イメージを使用して、社内のコンピューティング ワークロードを管理しています。 ローカルの Docker ツールを使用して、ご自分のコンテナー イメージをビルドします。 Azure Container Registry の使用を決断することにより、クラウド内でコンテナー イメージをビルドできるようになりました。 

Azure Container Registry Build を使用することで これらのコンテナーをビルドできるようになりました｡ また、Container Registry Build では、ソース コードのコミット時に DevOps プロセスと自動化されたビルドを統合できます。

Azure Container Registry Build を使用してコンテナー イメージの作成を自動化してみましょう。

## <a name="create-a-container-image-with-azure-container-registry-build"></a>Azure Container Registry Build を使用してコンテナー イメージを作成する

ビルド手順が提供されている標準の Dockerfile を使用します。 Azure Container Registry Build では、ご利用の環境に現在ある任意の Dockerfile を再利用することができます。

この例では、新しい Dockerfile を使用します。 

最初の手順として、`Dockerfile` という名前の新しいファイルを作成します。 任意のテキスト エディターを使用して、ファイルを編集することができます。 この例では、Visual Studio Code を使用します。

```bash
code Dockerfile
```

次のコンテンツを新しい Dockerfile にコピーします。 必ずファイルをセキュリティで保護します。 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

この構成では、Node.js アプリケーションが `node:9-alpine` イメージに追加されます。 次に、ポート 80 上で *EXPOSE* 命令を介してアプリケーションを提供できるようにコンテナーを構成します。

次に、Azure CLI コマンド `az acr build` を実行して、Dockerfile からコンテナー イメージをビルドします。

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

このコマンドを実行すると、ビルドされてご利用のコンテナー レジストリにプッシュされるイメージが表示されます。

## <a name="verify-the-image"></a>イメージを確認する

次のコマンドを実行して、イメージが作成されてレジストリに格納されたことを確認します。

```azurecli
az acr repository list --name <acrName> --output table
```

出力は次のようになります。

```console
Result
-------------
helloacrbuild
```

これで、`helloacrbuild` イメージが使用できる状態になりました。
