Azure Container Registry Build では、コンテナー イメージをクラウドでビルドできます。ローカルの Docker ツールは必要ありません。 また、Container Registry Build では、ソース コードのコミット時に DevOps プロセスと自動化されたビルドを統合できます。

このユニットでは、Azure Container Registry Build を使用してコンテナー イメージの作成を自動化します。

## <a name="create-a-container-image-with-build"></a>Build を使用してコンテナー イメージを作成する

Azure Container Registry Build の使用時には、標準の Dockerfile を使用してビルド手順が提供されます。 環境で現在使用されている Dockerfile (複数の段階で構成されるビルドを含む) は Azure Container Registry Build と連携します。

`Dockerfile` という名前のファイルを作成し、任意のテキスト エディターで開きます。

```bash
code Dockerfile
```

次の内容をコピーしてファイルを保存し、テキスト エディターを終了します。 この Dockerfile は、Node.js アプリケーションを `node:9-alpine` イメージに追加し、コンテナーをポート 80 上のアプリケーションのサーバーとして構成します。

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

次のコマンドを実行して、Dockerfile からコンテナー イメージをビルドします。

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

この操作を実行すると、イメージがビルドされ、コンテナー レジストリにプッシュされます。

## <a name="verify-the-image"></a>イメージを確認する

ビルド操作が完了したら、次のコマンドを実行して、イメージが作成されてレジストリに格納されたことを確認します。

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

## <a name="summary"></a>まとめ

このユニットでは、Azure Container Registry Build を使用してコンテナー イメージの作成を自動化しました。 続いて、このイメージが Azure コンテナー レジストリに格納されました。 次のユニットでは、新しいコンテナー イメージのインスタンスを Azure Container Instances にデプロイします。