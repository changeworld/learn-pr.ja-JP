これで機能しているコンテナーの開発環境が用意できたので、コンテナーを実行、一覧表示、および削除する一般的な方法をいくつか見てみましょう。

ここでは、次の方法を学習します。

* 基本のコンテナーを実行する。
* コンテナー イメージをダウンロードする。
* コンテナーとそれに関連付けられているイメージを削除する。

## <a name="whats-a-container-image"></a>コンテナー イメージとは?

コンテナー _イメージ_には、ベース オペレーティング システムと、任意の追加プロセス、アプリケーション、および構成が含まれます。 仮想マシンと同様に、_コンテナー_はイメージの実行中のインスタンスです。

## <a name="connect-to-your-linux-vm"></a>ご利用の Linux VM に接続する

前回のパートで作成した SSH セッションから切断した場合、ログインする必要があります。 念のためその方法を次に示します。

1. IP アドレスを取得します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. VM に SSH で接続します。 **ip-address** を、実際に使用する値に置換します。

    ```bash
    ssh azureuser@ip-address
    ```

## <a name="create-and-run-a-basic-container"></a>基本のコンテナーを作成して実行する

ここでは、Alpine Linux に基づくコンテナーを実行します。 Alpine Linux はそのサイズが理由でよく使用されます。コンテナー イメージのサイズを 5 MB 程度の小ささにすることができます。

この `docker run` コマンドを実行して、Alpine Linux に基づくコンテナーを作成します。

```bash
docker run alpine echo "Hello World"
```

次のような結果が出力されます。

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

`docker run` コマンドでは、コンテナーが作成され、指定されたコマンドが実行され、さらにコンテナーが破棄されます。

ここでは、Docker によって、[alpine](https://hub.docker.com/_/alpine?azure-portal=true) イメージが Docker Hub からダウンロードされ、コンテナーが起動され、"Hello World" がコンソールに出力されます。

この場合、`echo` コマンドが一時的に実行されてから終了します。 前のパートでは、Nginx がフォアグラウンドで実行され、これによって、ご利用のコンテナーは、停止されるか、または Nginx サービスが停止されるまでアライブの状態に維持されています。

## <a name="get-container-images"></a>コンテナー イメージを取得する

コンテナー イメージは、コンテナー イメージ レジストリに格納されています。 前の例では、Docker のパブリック コンテナー レジストリである Docker Hub から **alpine** イメージが Docker によってプルされています。

`docker images` を実行して、ご自分のシステムにダウンロードされているイメージの一覧を確認します。

```bash
docker images
```

&mdash; **alpine** と **nginx** という 2 つのイメージが表示されます。

```output
REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
alpine              latest              196d12cf6ab1        8 days ago4.41MB
nginx               latest              06144b287844        2 weeks ago109MB
```

コンテナー イメージを検索するには、`docker search` コマンドを使用します。 たとえば、次を実行すると、名前に `nginx` が含まれるコンテナー イメージがすべて一覧表示されます。

```bash
docker search nginx --limit 5
```

この例では、引数 `--limit` によって、検索操作が最初の 5 つの結果のみに制限されています。

結果は次のようになります。

```output
NAME                                     DESCRIPTION         STARS               OFFICIAL            AUTOMATED
nginx                                    Official build of Nginx.         9672                [OK]
jwilder/nginx-proxy                      Automated Nginx reverse proxy for docker con…   1415                                    [OK]
richarvey/nginx-php-fpm                  Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion   LetsEncrypt container to use with nginx as p…   411                                     [OK]
bitnami/nginx                            Bitnami nginx Docker Image         58                                      [OK]
```

イメージがローカルにない場合は、`docker run` などのコマンドによって、自動的にイメージがプルダウンされます。 しかし、イメージは使用する前にダウンロードするのが一般的です。 これにより、確実に最新バージョンを使用できます。

そのためには、`docker pull` コマンドを使用します。 次を実行すると、Docker Hub から最新の **nginx** イメージがプルされます。

```bash
docker pull nginx
```

この例では、最新バージョンの **nginx** イメージが取得されていることがわかります。

```output
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
Status: Image is up to date for nginx:latest
```

## <a name="run-and-list-a-container"></a>コンテナーを実行して一覧表示する

前のパートでは、`docker run` コマンドを使用して Nginx を起動しました。 そのコマンドをもう一度実行して、何が起こるかを詳しく見てみましょう。

1. ご利用の SSH 接続から、このコマンドを実行して Nginx を実行するコンテナーを起動します。

    ```bash
    docker run -d -p 8080:80 nginx
    ```

    * 引数 `-d` では、コンテナーがデタッチ モードで実行されること、つまり、コンテナーがバックグラウンドで実行されることが指定されます。 Nginx プロセスが停止またはクラッシュした場合には、コンテナー自体も停止されます。
    * 引数 `-p` では、コンテナー ホスト (この場合はご自分の Linux VM) のポート 8080 で受信されるネットワーク トラフィックが、コンテナー上のポート 80 に転送されることが指定されます。 前のパートではこの引数を使用して、ご利用のブラウザーから Web サーバーにアクセスしました。
    * 引数 `nginx` は実行するコンテナー イメージの名前です。

    `docker run` オプションの完全な一覧については、後で「[docker run reference](https://docs.docker.com/engine/reference/run?azure-portal=true)」 (Docker run リファレンス) をご覧ください。

    `docker run` コマンドでは、コンテナーの一意の識別子が表示されます。 例:

    ```output
    9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466
    ```

1. `docker ps` コマンドを実行して、ご利用のシステム上で実行されているコンテナーを一覧表示します。

    ```bash
    docker ps
    ```

    開始したばかりの Nginx コンテナーが表示されます。 コンテナーに ID と名前の両方があることに注意します。 これらの値のいずれかを使用して、コンテナーを管理できます。 コンテナー ID を書き留めます。 これは後で使用します。

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS              PORTS                  NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   4 minutes ago     Up 4 minutes        0.0.0.0:8080->80/tcp   compassionate_allen
    ```

1. 前に行ったように、ブラウザーでご利用の VM の IP アドレスに移動することで、実行中の Web サーバーを確認できます。 URL の一部として、ポート 8080 を指定することを忘れないでください。

    ![ブラウザーで実行されている Web サイト](../media/2-nginx-browser.png)

## <a name="delete-containers"></a>コンテナーを削除する

コンテナーを削除するには、`docker rm` コマンドを使用します。 コンテナーは、その名前と ID で指定することができます。

1. `docker rm` を実行して、Nginx を実行しているご自分のコンテナーを削除してみてください。 ID を検索する必要がある場合は、`docker ps` を実行してください。 次に例を示します。

    ```bash
    docker rm 9d7327e31212
    ```

    コンテナーは実行中なので、削除できないことがわかります。

    ```output
    Error response from daemon: You cannot remove a running container 9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466. Stop the container before attempting removal or force remove
    ```

1. `docker stop` を実行して、コンテナーを停止します。 次に例を示します。

    ```bash
    docker stop 9d7327e31212
    ```
    
1. `docker ps` を実行して、コンテナーがもう実行されていないことを確認します。

    ```bash
    docker ps
    ```

    次のように表示されます。

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATEDSTATUS              PORTS               NAMES
    ```

1. `docker ps` をもう一度実行しますが、今回は `-a` フラグを指定します。 この場合は、停止されていても、すべてのコンテナーが一覧表示されます。

    ```bash
    docker ps -a
    ```

    次のような結果が出力されます。

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS                          PORTS               NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   11 minutes ago     Exited (0) About a minute ago                       compassionate_allen
    df8aeea0320f        alpine              "echo 'Hello World'"     About an hour ago   Exited (0) About an hour ago                        objective_payne
    3a7efb75c288        nginx               "nginx -g 'daemon of…"   About an hour ago   Exited (0) About an hour ago                        nginx
    ```

    出力には、停止したばかりの Nginx コンテナーに加えて、以前実行した他のコンテナーも含まれます。

1. `docker rm` をもう一度実行します。 表示されている ID をご自分の ID のいずれかに置き換えます。

    ```bash
    docker rm 9d7327e31212
    ```

1. 次の `docker rm` コマンドを実行すると、アクティブなコンテナーが_すべて_削除されます。

    ```bash
    docker rm $(docker ps -aq)
    ```

1. `docker ps -a` をもう一度実行します。すると、実行中または停止中であるアクティブなコンテナーが存在しないことがわかります。

    ```bash
    docker ps -a
    ```

## <a name="delete-a-container-image"></a>コンテナー イメージを削除する

コンテナー イメージを削除するには、`docker rmi` コマンドを使用します。

イメージの削除を行うことができるのは、そのイメージから開始されたコンテナー (実行中または停止中) がアクティブでない場合のみです。 ただし、引数 `-f` を指定すると、関連付けられたすべてのコンテナーの削除が強制的に行われ、次いでコンテナー イメージが削除されます。

1. `docker rmi nginx` を実行して、ご利用の Linux VM から Nginx イメージを削除します。

    ```bash
    docker rmi nginx
    ```

    表示される出力は次のようになります。

    ```output
    Untagged: nginx:latest
    Untagged: nginx@sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
    Deleted: sha256:06144b2878448774e55577ae7d66b5f43a87c2e44322b3884e4e6c70d070b262
    Deleted: sha256:824a442ee3c96744d75be3737a22cc6a47aebad1b30be67f3c2f8f29cb0aa879
    Deleted: sha256:8e3d1e9e4945f930c93c30617512998437f6edafd86676770d29b1581f2520bb
    Deleted: sha256:8b15606a9e3e430cb7ba739fde2fbb3734a19f8a59a825ffa877f9be49059817
    ```

1. `docker images` を実行して、ご利用の VM 上のイメージを一覧表示します。 

    ```bash
    docker images
    ```

    **alpine** イメージが表示されます。

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
    alpine              latest              196d12cf6ab1        8 days ago4.41MB
    ```

1. 次の `docker rmi` コマンドを実行して、ご利用の VM から_すべて_のイメージを削除します。

    ```bash
    docker rmi $(docker images -q)
    ```

1. `docker images` をもう一度実行して、VM 上にイメージがないことを確認します。

    ```bash
    docker images
    ```
