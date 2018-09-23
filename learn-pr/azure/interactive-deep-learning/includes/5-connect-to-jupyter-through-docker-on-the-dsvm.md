データ サイエンス仮想マシンが稼働状態になったので、最初のディープ ラーニング モデルをトレーニングすることにします。 サーバー上の他のすべてのものから分離して実験する必要があります。 そのために、Docker コンテナー内で実験を実行します。

## <a name="connect-to-the-vm-with-ssh"></a>SSH を使って VM に接続する

まだ SSH で VM に接続していることを確認します。 接続していない場合は、もう一度次のコマンドを実行して仮想マシンにリモート接続します。

1. Azure Cloud Shell で次のコマンドを実行して VM にサインインします。

    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 
    
    `<USERNAME>` を、VM 作成時に定義した管理者アカウント名に置き換えます。 `<IP>` を、前の演習で保存した VM の IP アドレスに置き換えます。  

1. メッセージが表示されたら、管理者アカウントのパスワードを入力して、サインイン プロセスを完了します。

## <a name="run-a-jupyter-notebook-server-in-a-docker-container-in-the-vm"></a>VM で Docker コンテナー内の Jupyter Notebook サーバーを実行する

> [!NOTE]
> VM の管理者アカウントつまりルート アカウントしか持っていないので、`sudo` を使用してすべての Docker コマンドをルートとして実行する必要があります

1. VM 上に存在する Docker コンテナーを確認するには、コマンド プロンプトで次のコマンドを実行します。

    ```azurecli 
    sudo docker ps
    ```

1. コマンド プロンプトで次のコマンドを実行して、実験用の新しいコンテナーを作成します。

    ```azurecli 
    sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
    'conda install jupyter matplotlib -y &&\
    curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
    ``` 

    このコマンドの実行には時間がかかります。 そのため、しばらく時間があるので、コマンドの機能について説明しましょう。 
    - `docker run` は、新しいコンテナーでコマンドを実行します。 使用されている Docker イメージは、pytorch/pytorch です。 最初に指定されたイメージの上に書き込み可能なコンテナー レイヤーを作成した後、指定されたコマンドを使用してそれを起動します。
    - `--rm` は、終了したコンテナーを削除します。 コンテナーを残しておく必要がある場合は、この引数を削除します。 
    - `--entrypoint 'bin/sh'` は、イメージの既定のエントリ ポイントを Bash シェルに上書きします。
    - `-c` は、コンテナーの起動時に実行するコマンドを定義します。 ここでは、次の 3 つのコマンドを実行しています。
        - Jupyter と matplotlib をインストールします
        - ノートブック (cifar10_tutorial.ipynb) を pytorch.org から `first_pytorch_classifier.ipynb` という名前のコンテナー内のファイルにコピーします
        - 前の演習と同じ方法で、コンテナー内の Notebook サーバーを起動します。  ブラウザーは開始されず、ノートブックにはルートからアクセスでき、ノートブックはすべてのポートでリッスンできます。 
    
    Notebook サーバーは、そのコンテナーのすべてのポートでリッスンします。 しかし、コンテナー外からのトラフィックはどのように送られるのでしょうか。 `-p 8888:8888` 引数は、コンテナーのポート `8888` をホスト マシンの TCP ポート 8888 にバインドします。 そのため、ポート 8888 経由で VM に送信されるトラフィックは、コンテナーによってピックアップされます。 

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a>リモート ブラウザーから Jupyter Notebook サーバーに接続する 

Jupyter Notebook がコンテナーで実行されると、次のようなメッセージが表示されます。 

> *Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken} (初めて接続するときは、この URL をコピーしてブラウザーに貼り付け、トークンを使用してログインします: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken})*

1. Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the URL with `<HOSTNAME>.<REGION>.cloudapp.azure.com` or the IP address of the VM and navigate to the address  in a new a tab of your browser. (URL の http://(5b8783e7911d または 127.0.0.1) の部分を `&lt;HOSTNAME&gt;.&lt;REGION&gt;.cloudapp.azure.com` または VM の IP アドレスに置き換え、ブラウザーの新しいタブでアドレスに移動します。)

    ![Jupyter Notebook のダッシュボードのスクリーンショット。 ](../media/notebook-in-docker.png)
    
    今度は、ノートブックが 1 つだけ表示されます。 今はコンテナー内にいて、このノートブックのみをコピーしたためです。 次の演習では、このノートブックを調べます。 
    
    > [!TIP]
    > まだ Notebook サーバーをシャットダウンしないでください。 次の演習で `first_pytorch_classifier.ipynb` ノートブックを調べます。