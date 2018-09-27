ここでは、コンテナー ログ、コンテナー イベントをプルし、それをコンテナー インスタンスにアタッチするなどの、いくつかの基本的なトラブルシューティング操作を実行します。 このモジュールの最後には、コンテナー インスタンスをトラブルシューティングするための基本的な機能を理解できるようになります。

## <a name="create-a-container"></a>コンテナーを作成する

まず次のコンテナーを作成します。 

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/sample-aks-helloworld \
    --ports 80 \
    --ip-address Public \
    --location eastus
```

## <a name="get-logs-from-a-container-instance"></a>コンテナー インスタンスからログを取得する

アプリケーション コードからコンテナー内のログを表示するために、`az container logs` コマンドを使用できます。

```azurecli
az container logs \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

出力例:

```output
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
Running inside /app/prestart.sh, you could add migrations to this file, e.g.:

#! /usr/bin/env bash

# Let the DB start
sleep 10;
# Run migrations
alembic upgrade head
```

## <a name="get-container-events"></a>コンテナー イベントを取得する

`az container attach` コマンドでは、コンテナーの起動中の診断情報が示されます。 コンテナーが起動すると、ローカル コンソールに STDOUT と STDERR もストリーミングされます。

```azurecli
az container attach \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer
```

出力例:

```output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-09-21 23:48:14+00:00) pulling image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:09+00:00) Successfully pulled image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:12+00:00) Created container
(count: 1) (last timestamp: 2018-09-21 23:49:13+00:00) Started container

Start streaming logs:
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
```

> [!TIP]
> 接続されているコンテナーから切断するには、<kbd>Ctrl キーを押しながら C キー</kbd>を押します。

## <a name="execute-a-command-in-a-container"></a>コンテナーでコマンドを実行する

Azure Container Instances は、実行中のコンテナーでのコマンドの実行をサポートします。 既に開始されているコンテナー内でのコマンドの実行は、アプリケーションの開発とトラブルシューティング時に特に役立ちます。 この機能の最も一般的な用途は、対話型シェルを起動して、実行中のコンテナーで発生した問題をデバッグできるようにすることです。

この例では、実行中のコンテナーで対話型のターミナル セッションを開始します。

```azurecli
az container exec \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --exec-command /bin/sh
```

コマンドが完了すると、実質的にコンテナー内で作業していることになります。 この例では、作業ディレクトリの内容を表示する `ls` コマンドを実行しました。

```output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

`exit` を実行してリモート セッションを停止します。

## <a name="monitor-container-cpu-and-memory"></a>コンテナーの CPU とメモリを監視する

CPU とメモリの使用量のメトリックをプルしたい場合があります。 これを行うには、まず、Azure コンテナー インスタンスの ID を取得します。 この例では、ID は `CONTAINER_ID` という名前の変数に配置されています。

```azurecli
CONTAINER_ID=$(az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name mycontainer --query id --output tsv)
```

ここで、`az monitor metrics list` コマンドを使用して、CPU の使用量の情報を戻します。

```azurecli
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric CPUUsage \
    --output table
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
az monitor metrics list \
    --resource $CONTAINER_ID \
    --metric MemoryUsage \
    --output table
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

この情報は、Azure portal でも見つけることができます。 CPU とメモリ使用量の情報を図で確認する場合は、コンテナー インスタンスの Azure portal の概要ページを参照してください。

![Azure Container Instances の CPU とメモリ使用量の情報を示す Azure portal のビュー](../media/6-cpu-memory.png)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
