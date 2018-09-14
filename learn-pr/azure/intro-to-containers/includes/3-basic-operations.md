機能しているコンテナーの開発環境を作成したようになりましたが、実行、リスト、およびコンテナーを削除するためにコマンドを見てみましょう。

## <a name="create-and-run-a-basic-container"></a>作成し、基本的なコンテナーの実行

次のコマンドを使用して最初のコンテナーを作成します。

```bash
docker run alpine echo "Hello World"
```

次のような出力結果が表示されます。

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

`docker run` コマンドによって、コンテナーのインスタンスが作成されます。 この例では、コンテナーはローカル システムにダウンロードされた `alpine` という名前のコンテナー イメージから作成されています。 コンテナーが開始されると、コンテナー内で `echo "Hello World"` コマンドが実行され、出力がターミナルにエコーされます。

## <a name="get-container-images"></a>コンテナー イメージを取得する

コンテナー イメージには、基本のオペレーティング システムと、追加のプロセス、アプリケーション、および構成が含まれています。 コンテナー イメージは、コンテナー イメージ レジストリに格納されています。 "Hello World"の例で、 *alpine*イメージを Docker Hub にパブリック コンテナー レジストリからプルされました。

次のコマンドを実行して、ご自分のシステムにダウンロードされているイメージの一覧を確認します。

```bash
docker images
```

実行した場合、`docker run alpine`コマンドの前に、alpine イメージの一覧を表示する必要があります。

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

コンテナー イメージを検索するには、`docker search` コマンドを使用します。 たとえば、次の例を使用して、名前に `nginx` が含まれるすべてのコンテナー イメージを列挙します。

```bash
docker search nginx
```

出力は次のようになります。

```output
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

```output
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

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a>コンテナーを実行する

*nginx* を識別してダウンロードできたので、イメージからコンテナーを実行します。 コンテナー イメージの実行に Docker CLI を使用する場合、`docker run` コマンドを使用します。

`-d`引数は、コンテナーはデタッチ モードで実行されていることを指定します。 この構成では、コンテナーは指定されたプロセスを実行します。 そのプロセスが停止したり、クラッシュした場合には、コンテナー自体が停止されます。 `-p 8080:80`引数は、コンテナー ホストで、この場合は、開発システム、ポート 8080 に到着したネットワーク トラフィックが、コンテナーのポート 80 に転送されることを指定します。 最後に、`ngingx` 引数は実行するコンテナー イメージの名前です。

`docker run` の引数の完全な一覧については、「[docker run reference](https://docs.docker.com/engine/reference/run/)」 (Docker run リファレンス) を参照してください。

```bash
docker run -d -p 8080:80 nginx
```

この操作によって、完全なコンテナー ID が返されます。

```output
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

`docker ps` コマンドを使用して、システムで実行されているコンテナーを一覧表示します。

```bash
docker ps
```

最後の手順で実行した NGINX コンテナーが、唯一実行されているコンテナーとして表示されます。 コンテナーに ID と名前の両方があることに注意します。 コンテナーの管理には、これらの値のいずれかを使用できます。 コンテナー ID を書き留めます。 この値は、後で使用されます。

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

コンテナーをテストするブラウザーを開き、および入力`http://localhost:8080`アドレス。 完了した後、ようこそメッセージを含む NGINX の既定 web ページが表示されます。

## <a name="delete-containers"></a>コンテナーを削除する

その名前または ID を渡すことによって、コンテナーを削除する、`docker rm`コマンド。 NGINX を実行しているコンテナーのコンテナー id は、この操作をやり直してください。

```bash
docker rm bd2424bfe7a5
```

コンテナーは実行中なので、削除できません。

```output
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

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

停止状態のコンテナーを含むすべてのコンテナーが一覧表示されるようにするには、`-a` 引数を `docker ps` コマンドに追加します。

```bash
docker ps -a
```

出力は次のようになります。

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

削除操作を再実行します。 この例の ID をご自分の環境の ID に置き換えます。

```bash
docker rm bd2424bfe7a5
```

この操作により、コンテナーの ID が返されます。

```output
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a>コンテナー イメージを削除する

使用してコンテナー イメージを削除する、`docker rmi`コマンド。 (実行および停止中の) 任意のコンテナーがコンテナー イメージから起動されている場合、そのイメージは削除できません。 最初にコンテナーを削除する必要があります。 追加する、`-f`への引数、`docker rmi`コマンドが強制的に関連付けられているすべてのコンテナーの削除とし、コンテナー イメージが削除されます。

次のコマンドを使用して、NGINX コンテナー イメージを削除します。

```bash
docker rmi nginx
```

出力は次のようになります。

```output
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```