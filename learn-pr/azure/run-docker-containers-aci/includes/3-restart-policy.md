Azure Container Instances ではコンテナー デプロイを簡単にすばやく行えるため、コンテナー インスタンスでのビルド、テスト、イメージ レンダリングなどの一度のみ実行されるタスクの実行に優れたプラットフォームを提供します。

構成可能な再起動ポリシーを使用して、プロセスが完了したらコンテナーが停止するように指定できます。 コンテナーのインスタンスは秒単位で課金されるため、タスクを実行するコンテナーの実行中に使用されるコンピューティング リソースのみが課金されます。

## <a name="container-restart-policies"></a>コンテナー再起動ポリシー

Azure Container Instances でコンテナーを作成する場合、3 つの再起動ポリシー設定のいずれかを指定できます。

| 再起動ポリシー   | 説明 |
| ---------------- | :---------- |
| `Always` | コンテナー グループ内のコンテナーを常に再起動する。 これは**既定**の設定で、コンテナー作成時に再起動ポリシーが指定されていない場合に適用されます。 |
| `Never` | コンテナー グループ内のコンテナーを再起動しない。 コンテナーは最大で 1 回実行されます。 |
| `OnFailure` | コンテナーで実行されたプロセスが失敗 (0 以外の終了コードで終了) した場合にのみ、コンテナー グループ内のコンテナーを再起動する。 コンテナーは少なくとも 1 回実行されます。 |

このモジュールの前のユニットでは、再起動ポリシーを指定せずに、コンテナーを作成しました。 既定で、このコンテナーが *Always*  再起動ポリシーを受け取りました。 コンテナー内のワークロードは長時間実行されているため (Web サーバー)、このポリシーは理にかなっています。

## <a name="run-to-completion"></a>完了まで実行

再起動ポリシーが動作しているのを確認するには、*microsoft/aci-wordcount* イメージからコンテナー インスタンスを作成し、*OnFailure* 再起動ポリシーを指定します。 このコンテナー例では、シェイクスピアのハムレットのテキストを解析し、最もよく使われる単語 10 個を STDOUT に書き込んで終了する Python スクリプトを実行します。

次の `az container create` コマンドを使用してコンテナー例を実行します。

```azureclu
az container create \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances はコンテナーを開始し、そのアプリケーション (ここではスクリプト) が終了すると停止します。 Azure Container Instances が再起動ポリシーが *Never* または *OnFailure* のコンテナーを停止すると、そのコンテナーの状態は終了に設定されます。

`az container show` コマンドで、コンテナーの状態を確認できます。

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer-restart-demo \
    --query containers[0].instanceView.currentState.state
```

コンテナー例の状態が終了と表示されたら、コンテナー ログを表示してタスクの出力を確認できます。 az container logs コマンドを実行して、スクリプトの出力を表示します。

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer-restart-demo
```

出力:

```bash
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

## <a name="summary"></a>まとめ

このユニットでは、*OnFailure* の再起動ポリシーを使用して、コンテナー インスタンスを作成しました。 この構成では、存続期間の短いタスクを実行するコンテナーに適しています。

次のユニットでは、Azure Container Instances で環境変数を設定します。
