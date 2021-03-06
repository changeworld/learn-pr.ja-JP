Azure Container Instances では、コンテナーのデプロイを簡単かつ迅速に行えるため、ビルド、テスト、イメージ レンダリングなどの一度のみ実行されるタスクの実行に最適です。

構成可能な再起動ポリシーを使用して、プロセスが完了したらコンテナーが停止するように指定できます。 コンテナーのインスタンスは秒単位で課金されるため、タスクを実行するコンテナーの実行中に使用されるコンピューティング リソースのみが課金されます。

## <a name="container-restart-policies"></a>コンテナーの再起動ポリシー

Azure Container Instances には、次の 3 つの再起動ポリシー オプションがあります。

| 再起動ポリシー   | 説明 |
| ---------------- | :---------- |
| `Always` | コンテナー グループ内のコンテナーを常に再起動する。 このポリシーは、Web サーバーなどの実行時間の長いタスクに適しています。 これは**既定**の設定で、コンテナー作成時に再起動ポリシーが指定されていない場合に適用されます。 |
| `Never` | コンテナー グループ内のコンテナーを再起動しない。 コンテナーは最大で 1 回実行されます。 |
| `OnFailure` | コンテナーで実行されたプロセスが失敗 (0 以外の終了コードで終了) した場合にのみ、コンテナー グループ内のコンテナーを再起動する。 コンテナーは少なくとも 1 回実行されます。 このポリシーは、存続期間の短いタスクを実行するコンテナーに適しています。 |

## <a name="run-to-completion"></a>完了まで実行

再起動ポリシーが動作しているのを確認するには、*microsoft/aci-wordcount* イメージからコンテナー インスタンスを作成し、*OnFailure* 再起動ポリシーを指定します。 このコンテナー例では、シェイクスピアのハムレットのテキストを解析し、最もよく使われる 10 個の単語を STDOUT に書き込んで終了する Python スクリプトを実行します。

次の `az container create` コマンドでコンテナー例を実行します。

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --location eastus
```

Azure Container Instances によってコンテナーが開始され、そのアプリケーション (ここではスクリプト) の終了時に停止されます。 Azure Container Instances によって、再起動ポリシーが *Never* または *OnFailure* のコンテナーが停止されると、そのコンテナーの状態は **Terminated** に設定されます。

`az container show` コマンドで、コンテナーの状態を確認できます。

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

コンテナー例の状態が **Terminated** と表示されたら、コンテナー ログを表示してタスクの出力を確認できます。 `az container logs` コマンドを実行して、スクリプトの出力を表示します。

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer-restart-demo
```

出力: 

```json
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```