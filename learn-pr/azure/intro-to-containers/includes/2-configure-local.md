コンテナーまたはコンテナー統合アプリケーションを Azure で実行する前に、多くの場合はご利用のノート PC のようなローカル開発環境で作業します。 このユニットは、Docker を使用したコンテナー開発のためにシステムを準備するのに役立ちます。

## <a name="docker-for-windows-and-mac"></a>Docker for Windows と Docker for Mac

ローカル コンテナー開発環境をインストールして構成する 2 つのアプリケーションが Docker, Inc. から発行されています。 各アプリケーションでは基本的に、必要な CLI や自動化ツールなど、Docker ツールを使用してご利用のシステムの準備が整えられます。 Docker プラットフォームをホストする仮想マシンも作成されます。 Docker コマンドが仮想マシンを通過するように、環境は構成されます。 これらの各アプリケーションは機能的に類似しており、次の機能が含まれています。

- **Docker プラットフォーム:** コンテナーを作成し実行するのに必要なコア コンポーネント。
- **Docker CLI:** Docker コンテナーとやり取りするためのコマンド ライン インターフェイス。
- **Docker Compose:** 複数コンテナー アプリケーションを定義および実行するための自動化ツール。

以下の適切なリンクを新しいタブで開き、オペレーティング システムに Docker をインストールします。 

- [Docker for Windows](https://www.docker.com/docker-windows)
- [Docker for Mac](https://www.docker.com/docker-mac)

## <a name="docker-for-windows-environments"></a>Docker for Windows 環境

Docker for Windows を使用する場合は、Linux と Windows の 2 つの環境が利用できます。 Linux 環境を使用すると、ご利用の Windows システム上で Linux コンテナーを実行することができます。 環境を選択するには、Docker タスク バー アイコンを右クリックし、**[Switch to Linux containers]\(Linux コンテナーに切り替える\)** を選択し、画面の指示に従います。

![Docker for Windows、Linux コンテナーに切り替える](../media-draft/2-docker-linux.png)

> [!NOTE]
> このチュートリアルの手順では、ご利用のシステムが Linux コンテナーを使用するように構成されていることを前提とします。

## <a name="docker-on-linux"></a>Linux 上の Docker

Linux ベースのシステム上で作業している場合、Docker サーバー コンポーネントと CLI ツールは手動でインストールすることができます。 ご利用の特定の Linux ディストリビューションについては、[Docker CE](https://docs.docker.com/install/#server) に関するページにある説明を参照してください。

## <a name="validate-configuration"></a>構成を検証する

Docker が正常にインストールおよび構成されていることを検証するには、ターミナルを開き、次のコマンドを実行します。

```bash
docker search nginx
```

次のような出力が表示された場合、ご利用のシステムでは次のユニットのための準備ができています。

```output
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9034                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1362                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   589                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   204                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   59                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          36                                      [OK]
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```

## <a name="summary"></a>まとめ

このユニットでは、コンテナーのローカル開発環境の準備を整えました。 次のユニットでは、基本的な Docker 操作のいくつかについて説明します。