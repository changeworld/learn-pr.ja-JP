コンテナーまたはコンテナー統合アプリケーションを Azure で実行する前に、多くの場合はご利用のノート PC のようなローカル開発環境で作業します。 ここには、システムは、Docker でコンテナーの開発用に準備します。

## <a name="why-use-containers"></a>コンテナーを使用する理由

コンテナーとコンテナー イメージは、ディスク領域、メモリ、および CPU などのホスト リソースを効率的に使用します。 このような効率性により、コンテナーはすぐに起動します。 場合によっては、コンテナーの新しいインスタンスはほぼ瞬時に起動します。 これにより、アプリケーションの迅速なプロビジョニングでき、オンデマンドでの処理とスケール操作の新しいモデルです。

このシナリオの想定: 需要の急増が確認される場合があるバッチ処理サービスを実行しています。 コンテナーを使用する新しいインスタンスを迅速にプロビジョニングして需要の増大に対応するシステムを構築できます。 それは強力なシステムですが、従来の仮想マシンを使用して実現するのは容易ではありません。

コンテナーでは、「ハイパー密度」を実現することもできます。 つまり、少ない仮想または物理リソースに複数のアプリケーションとプロセスを実行できます。

コンテナーは Web サーバーのような従来のワークロードを実行するのに適したプラットフォームであると同時に、負荷の急増に対応できるバッチ処理、最新の分散アーキテクチャでビルドされたアプリケーション、オンデマンド スケーリングを必要とする任意のものなど、機会を開くのにも役立ちます。

## <a name="docker-for-windows-and-mac"></a>Docker for Windows および Docker for Mac

Docker, Inc. がインストールしてローカルのコンテナー開発環境を構成する 2 つのアプリケーションを発行します。 アプリケーションは、Docker を使用した、システムを CLI と自動化のために必要なツールなど、ツールを準備します。 Docker プラットフォームをホストする仮想マシンも作成されます。 Docker コマンドが仮想マシンを通過するように、環境は構成されます。 これらの各アプリケーションは機能的に類似しており、次の機能が含まれています。

- **Docker プラットフォーム:** を作成し、コンテナーの実行に必要なコア コンポーネント。
- **Docker CLI:** Docker コンテナーを操作するコマンド ライン インターフェイス。
- **Docker Compose:** オートメーション ツールを定義すると、マルチ コンテナー アプリケーションを実行します。

オペレーティング システムに Docker をインストールする新しいタブで、以下の適切なリンクを開きます。 

- [Docker for Windows](https://www.docker.com/docker-windows)
- [Mac 用の docker](https://www.docker.com/docker-mac)

## <a name="docker-for-windows-environments"></a>Docker for Windows 環境

Docker for Windows を使用する場合は、Linux と Windows の 2 つの環境が利用できます。 Linux 環境を使用すると、ご利用の Windows システム上で Linux コンテナーを実行することができます。 環境を選択するには、Docker タスク バー アイコンを右クリックし、**[Switch to Linux containers]\(Linux コンテナーに切り替える\)** を選択し、画面の指示に従います。

> [!NOTE]
> このチュートリアルの手順では、ご利用のシステムが Linux コンテナーを使用するように構成されていることを前提とします。

## <a name="docker-on-linux"></a>Linux 上の Docker

現在、Linux のセットアップ アプリケーションはありません。 Linux ベースのシステムで作業する場合に Docker サーバー コンポーネントと CLI ツールする必要があります手動でインストールします。 上の手順に従います[Docker CE について](https://docs.docker.com/install/#server)特定の Linux ディストリビューションです。

## <a name="validate-configuration"></a>構成を検証する

Docker が正常にインストールおよび構成されていることを検証するには、ターミナルを開き、次のコマンドを実行します。

```bash
docker search nginx
```

次の単位のお使い環境が次のような出力が表示された場合。

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