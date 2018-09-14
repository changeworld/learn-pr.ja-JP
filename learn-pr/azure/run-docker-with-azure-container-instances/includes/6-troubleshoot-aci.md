このユニットでは、コンテナー ログ、コンテナー イベントをプルし、それをコンテナー インスタンスにアタッチするなどの、いくつかの基本的なトラブルシューティング操作を実行します。 このモジュールの最後には、コンテナー インスタンスをトラブルシューティングする基本的な機能を理解できるようになります。

## <a name="create-a-container"></a>コンテナーの作成

まず、このユニットで使用するコンテナーを作成します。 このモジュールで作成した最初のコンテナーがまだある場合は、この手順はスキップします。

```azurecli
az container create --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer --image microsoft/aci-helloworld --ports 80 --ip-address Public
```

## <a name="get-logs-from-a-container-instance"></a>コンテナー インスタンスからのログの取得

アプリケーション コードからコンテナー内のログを表示するために、`az container logs` コマンドを使用できます。

```azazurecli
az container logs --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer
```

Web アプリが数回アクセスされた後のコンテナー例からのログ出力は次のとおりです。

```output
listening on port 80
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://23.101.136.193/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:25 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:27 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
```

## <a name="get-container-events"></a>コンテナー イベントの取得

`az container attach` コマンドからは、コンテナーの起動中の診断情報が出力されます。 コンテナーが起動すると、ご利用のローカル コンソールに STDOUT と STDERR もストリーミングされます。

```azazurecli
az container attach --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer
```

出力例:


```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-08-20 21:43:26+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:32+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Created container
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Started container

Start streaming logs:
listening on port 80
listening on port 80
```

## <a name="execute-a-command-in-a-container"></a>コンテナーでのコマンドの実行

Azure Container Instances は、実行中のコンテナーでのコマンドの実行をサポートします。 既に開始されているコンテナー内でのコマンドの実行は、アプリケーションの開発とトラブルシューティング時に特に役立ちます。 この機能の最も一般的な用途は、対話型シェルを起動して、実行中のコンテナーで発生した問題をデバッグできるようにすることです。

この例では、実行中のコンテナーで対話型のターミナル セッションを開始します。

```azurecli
az container exec --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer --exec-command /bin/sh
```

コマンドが完了すると、実質的にコンテナー内で作業していることになります。 この例では、作業ディレクトリの内容を表示する `ls` コマンドを実行しました。

```output
usr/src/app # ls
index.html         node_modules       package.json
index.js           package-lock.json
```

「`exit`」と入力し、リモート セッションを停止します。

## <a name="monitor-container-cpu-and-memory"></a>コンテナーの CPU とメモリを監視する

CPU とメモリの使用量に関するメトリックをプルしたい場合があります。 これを行うには、まず、Azure コンテナー インスタンスの ID を取得します。 この例では、ID は `CONTAINER_ID` という名前の変数に配置されています。

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

ここで、`az monitor metrics list` コマンドを使用して、CPU の使用量の情報を戻します。

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric CPUUsage --output table
```

出力例:

```output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:39:00  CPU Usage
2018-08-20 21:40:00  CPU Usage
2018-08-20 21:41:00  CPU Usage
2018-08-20 21:42:00  CPU Usage
2018-08-20 21:43:00  CPU Usage      0.375
2018-08-20 21:44:00  CPU Usage      0.875
2018-08-20 21:45:00  CPU Usage      1
2018-08-20 21:46:00  CPU Usage      3.625
2018-08-20 21:47:00  CPU Usage      1.5
2018-08-20 21:48:00  CPU Usage      2.75
2018-08-20 21:49:00  CPU Usage      1.625
2018-08-20 21:50:00  CPU Usage      0.625
2018-08-20 21:51:00  CPU Usage      0.5
2018-08-20 21:52:00  CPU Usage      0.5
2018-08-20 21:53:00  CPU Usage      0.5
```

メモリ使用量の情報を取得する場合は、次のコマンドを使用できます。

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric MemoryUsage --output table
```

出力例:

```output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:43:00  Memory Usage
2018-08-20 21:44:00  Memory Usage  0.0
2018-08-20 21:45:00  Memory Usage  15917056.0
2018-08-20 21:46:00  Memory Usage  16744448.0
2018-08-20 21:47:00  Memory Usage  16842752.0
2018-08-20 21:48:00  Memory Usage  17190912.0
2018-08-20 21:49:00  Memory Usage  17506304.0
2018-08-20 21:50:00  Memory Usage  17702912.0
2018-08-20 21:51:00  Memory Usage  17965056.0
2018-08-20 21:52:00  Memory Usage  18509824.0
2018-08-20 21:53:00  Memory Usage  18649088.0
2018-08-20 21:54:00  Memory Usage  18845696.0
2018-08-20 21:55:00  Memory Usage  19181568.0
```

この情報は、Azure portal でも見つけることができます。 CPU とメモリ使用量の情報を図で確認するには、コンテナー インスタンスに関する Azure portal の概要ページを参照してください。

![Azure Container Instances の CPU とメモリ使用量の情報を示す Azure portal のビュー](../media-draft/cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="summary"></a>まとめ

このユニットでは、コンテナー ログ、コンテナー イベントをプルし、それをコンテナー インスタンスにアタッチするなどの、いくつかのトラブルシューティング操作を学習しました。