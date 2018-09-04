これで機能しているコンテナーの開発環境ができたので、いくつかの基本的なコンテナー操作をすばやく見てみましょう。 このユニットには、Docker のすべての機能一覧は含まれていません (すべてにはほど遠いです)。 このユニットでは、コンテナーの実行、列挙、および削除を準備できます。 このモジュールの残りの部分では、コンテナーの操作についてさらに説明します。

## <a name="run-a-basic-container"></a>基本のコンテナーを実行する

コンテナーの実行と管理の詳細を掘り下げる前に、コンテナーを実行するのがいかに簡単であるかをすばやく見てみましょう。

次のコマンドを使用して最初のコンテナーを作成します。

```bash
docker run alpine echo "Hello World"
```

次のような出力が表示されます。

```bash
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

`docker run` コマンドによって、コンテナーのインスタンスが作成されます。 この例では、コンテナーはローカル システムにダウンロードされた `alpine` という名前のコンテナー イメージから作成されています。 コンテナーが開始されると、コンテナー内で `echo "Hello World"` コマンドが実行され、出力がターミナルにエコーされます。

この時点では、これらのアクションの技術的な詳細については心配しないでください。 それらについては、このモジュールを通して詳述します。

## <a name="get-container-images"></a>コンテナー イメージを取得する

hello world の例で見たとおり、コンテナーはコンテナー イメージから実行されます。 これらのイメージには、コンテナー ベースのオペレーティング システムと、追加のプロセス、アプリケーションおよび構成が含まれます。 コンテナー イメージは、コンテナー イメージ レジストリに格納されています。 hello world の例では、*alpine* のイメージは、パブリック コンテナーのレジストリである Docker Hub からプルされています。

事前に作成したコンテナー イメージを検索してダウンロードする方法を見てみましょう。

次のコマンドを実行して、ご自分のシステムにダウンロードされているイメージの一覧を確認します。

```bash
docker images
```

手順に従ってきた場合には、alpine のイメージが表示されるはずです。 このイメージは、hello world の例の実行時にダウンロードされています。

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

コンテナー イメージを検索するには、`docker search` コマンドを使用します。 たとえば、次の例を使用して、名前に `nginx` が含まれるすべてのコンテナー イメージを列挙します。

```bash
docker search nginx
```

出力は次のようになります。

```bash
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9071                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1365                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   593                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   207                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   60                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          38                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          10                                      [OK]
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```

イメージを実行する前に事前にダウンロードしたい場合、`docker pull` コマンドを使用します。 次の例では、システムに *nginx* イメージがプルされています。

```bash
docker pull nginx
```

出力は次のようになります。

```bash
Using default tag: latest
latest: Pulling from library/nginx
be8881be8156: Pull complete
f2f27ed9664f: Extracting [===============>                                   ]  6.652MB/22.14MB
54ff137eb1b2: Download complete
```

システム上のすべてのイメージの一覧に対して、`docker images` を再度実行します。 *nginx* イメージがご自分のシステムに追加されたのをご確認いただけます。

```bash
docker images
```

出力は次のようになります。

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a>コンテナーを実行する

*nginx* を識別してダウンロードできたので、イメージからコンテナーを実行します。 コンテナー イメージの実行に Docker CLI を使用する場合、`docker run` コマンドを使用します。

次の例では、`-d` 引数によって、コンテナーがデタッチ モードで実行されていることが指定されています。 この構成では、コンテナーは指定されたプロセスを実行します。 そのプロセスが停止したり、クラッシュした場合には、コンテナー自体が停止されます。 引数 `-p 8080:80` は、コンテナー ホスト (ここではご自分の開発システム) のポート 8080 で受信されるネットワーク トラフィックが、コンテナーのポート 80 に転送されることを指定します。 最後に、`ngingx` 引数は実行するコンテナー イメージの名前です。

`docker run` の引数の完全な一覧については、「[docker run reference](https://docs.docker.com/engine/reference/run/)」 (Docker run リファレンス) を参照してください。

```bash
docker run -d -p 8080:80 nginx
```

この操作によって、完全なコンテナー ID が返されます。

```bash
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

`docker ps` コマンドを使用して、システムで実行されているコンテナーを一覧表示します。

```bash
docker ps
```

最後の手順で実行した NGINX コンテナーが、唯一実行されているコンテナーとして表示されます。 コンテナーに ID と名前の両方があることに注意します。 コンテナーの管理には、これらの値のいずれかを使用できます。 コンテナー ID を書き留めます。 この値は、後のユニットで使用します。

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

コンテナーをテストするには、インターネット ブラウザーを開き、アドレスとして http://localhost:8080 を入力します。 完了すると、NGINX の既定の Web サイトが表示されます。

![NGINX のスプラッシュ スクリーンが表示された Microsoft Edge](../media-draft/3-nginx.png)

## <a name="delete-containers"></a>コンテナーを削除する

コンテナーをもう使用しない場合、`docker rm` コマンドにコンテナー名および ID を入力して削除することができます。 この操作は、NGINX を実行するコンテナーのコンテナー ID を使用して試してください。 この例の ID をご自分の環境の ID に置き換えます。

```bash
docker rm bd2424bfe7a5
```

コンテナーは実行中なので、削除できません。

```bash
Error response from daemon: You cannot remove a running container a31c5a5f2a8d6e420435bfcadbe158fa6a26ed29c005a892171505cc0c2861b2. Stop the container before attempting removal or force remove
```

`docker stop` コマンドを使用して、コンテナーを停止します。

```bash
docker stop bd2424bfe7a5
```

この時点で、すべてのコンテナーを列挙するために `docker ps` を実行しても、nginx コンテナーが表示されないことに注意します。

```bash
docker ps
```

出力は次のようになります。

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

停止状態のコンテナーを含むすべてのコンテナーが一覧表示されるようにするには、`-a` 引数を `docker ps` コマンドに追加します。

```bash
docker ps -a
```

出力は次のようになります。

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

削除操作を再実行します。 この例の ID をご自分の環境の ID に置き換えます。

```bash
docker rm bd2424bfe7a5
```

この操作により、コンテナーの ID が返されます。

```bash
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a>コンテナー イメージを削除する

コンテナー イメージでの作業が終わったら、`docker rmi` を使用して削除することができます。 (実行および停止中の) 任意のコンテナーがコンテナー イメージから起動されている場合、そのイメージは削除できません。 最初にコンテナーを削除する必要があります。 `-f` 引数を `docker rmi` コマンドに追加すると、関連するすべてのコンテナーの削除が強制され、次いでコンテナー イメージが削除されます。

次のコマンドを使用して、NGINX コンテナー イメージを削除します。

```bash
docker rmi nginx
```

出力は次のようになります。

```bash
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```

## <a name="summary"></a>まとめ

このユニットでは、Docker の基本的な操作をいくつか学習します。 次のユニットでは、カスタム コンテナー イメージを作成します。