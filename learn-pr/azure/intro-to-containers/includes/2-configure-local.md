<span data-ttu-id="6d71d-101">コンテナーまたはコンテナー統合アプリケーションを Azure で実行する前に、多くの場合はご利用のノート PC のようなローカル開発環境で作業します。</span><span class="sxs-lookup"><span data-stu-id="6d71d-101">Before you run a container or container-integrated application in Azure, you'll most likely work in a local development environment like your laptop.</span></span> <span data-ttu-id="6d71d-102">このユニットは、コンテナー開発のためにご利用のシステムを準備するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-102">This unit helps you prepare your system for container development.</span></span>

## <a name="docker-for-windows-and-mac"></a><span data-ttu-id="6d71d-103">Docker for Windows および Docker for Mac</span><span class="sxs-lookup"><span data-stu-id="6d71d-103">Docker for Windows and Mac</span></span>

<span data-ttu-id="6d71d-104">ローカル コンテナー開発環境をインストールして構成する 2 つのアプリケーションが Docker, Inc. から発行されています。</span><span class="sxs-lookup"><span data-stu-id="6d71d-104">Docker, Inc. has published two applications to install and configure a local container development environment.</span></span> <span data-ttu-id="6d71d-105">各アプリケーションでは基本的に、必要な CLI や自動化ツールなど、Docker ツールを使用してご利用のシステムの準備が整えられます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-105">Essentially, each application prepares your system with Docker tooling, such as the necessary CLI and automation tools.</span></span> <span data-ttu-id="6d71d-106">Docker プラットフォームをホストする仮想マシンも作成されます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-106">A virtual machine is also created that hosts the Docker platform.</span></span> <span data-ttu-id="6d71d-107">Docker コマンドが仮想マシンを通過するように、環境は構成されます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-107">The environment is configured such that Docker commands are passed through to the virtual machine.</span></span> <span data-ttu-id="6d71d-108">これらの各アプリケーションは機能的に類似しており、次の機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6d71d-108">Each of these applications is similar in functionality and includes the following features:</span></span>

- <span data-ttu-id="6d71d-109">Docker プラットフォーム - コンテナーを作成し実行するのに必要なコア コンポーネント。</span><span class="sxs-lookup"><span data-stu-id="6d71d-109">Docker platform - The core components necessary to create and run containers.</span></span>
- <span data-ttu-id="6d71d-110">Docker CLI - Docker コンテナーとやり取りするためのコマンド ライン インターフェイス</span><span class="sxs-lookup"><span data-stu-id="6d71d-110">Docker CLI - The command-line interface for interacting with Docker containers</span></span>
- <span data-ttu-id="6d71d-111">Docker Compose - 複数コンテナー アプリケーションを定義および実行するための自動化ツール。</span><span class="sxs-lookup"><span data-stu-id="6d71d-111">Docker Compose - Automation tooling for defining and running multi-container applications.</span></span>

<span data-ttu-id="6d71d-112">ご利用のシステム上に Docker をインストールするには、ご利用のオペレーティング システムに対応するリンクを参照します。</span><span class="sxs-lookup"><span data-stu-id="6d71d-112">To install Docker on your system, follow the link that matches your operating system.</span></span>

- <span data-ttu-id="6d71d-113">Docker for Windows - https://www.docker.com/docker-windows</span><span class="sxs-lookup"><span data-stu-id="6d71d-113">Docker for Windows - https://www.docker.com/docker-windows</span></span>
- <span data-ttu-id="6d71d-114">Docker for Mac - https://www.docker.com/docker-mac</span><span class="sxs-lookup"><span data-stu-id="6d71d-114">Docker for Mac - https://www.docker.com/docker-mac</span></span>

## <a name="docker-for-windows-environments"></a><span data-ttu-id="6d71d-115">Docker for Windows 環境</span><span class="sxs-lookup"><span data-stu-id="6d71d-115">Docker for Windows environments</span></span>

<span data-ttu-id="6d71d-116">Docker for Windows を使用する場合は、Linux と Windows の 2 つの環境が利用できます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-116">When you use Docker for Windows, two environments are available: Linux and Windows.</span></span> <span data-ttu-id="6d71d-117">Linux 環境を使用すると、ご利用の Windows システム上で Linux コンテナーを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-117">Using the Linux environment allows you to run Linux containers on your Windows system.</span></span> <span data-ttu-id="6d71d-118">環境を選択するには、Docker タスク バー アイコンを右クリックし、**[Switch to Linux containers]\(Linux コンテナーに切り替える\)** を選択し、画面の指示に従います。</span><span class="sxs-lookup"><span data-stu-id="6d71d-118">You can select an environment by right-clicking on the Docker task bar icon, selecting **Switch to Linux containers**, and following the on-screen prompts.</span></span>

![Docker for Windows、Linux コンテナーに切り替える](../media-draft/2-docker-linux.png)

> [!NOTE]
> <span data-ttu-id="6d71d-120">このチュートリアルの手順では、ご利用のシステムが Linux コンテナーを使用するように構成されていることを前提とします。</span><span class="sxs-lookup"><span data-stu-id="6d71d-120">The steps in this tutorial assume that your system is configured to work with Linux containers.</span></span>

## <a name="docker-on-linux"></a><span data-ttu-id="6d71d-121">Linux 上の Docker</span><span class="sxs-lookup"><span data-stu-id="6d71d-121">Docker on Linux</span></span>

<span data-ttu-id="6d71d-122">Linux ベースのシステム上で作業している場合、Docker サーバー コンポーネントと CLI ツールは手動でインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="6d71d-122">If you're working on a Linux-based system, the Docker server components and CLI tools can be manually installed.</span></span> <span data-ttu-id="6d71d-123">ご利用の特定の Linux ディストリビューションについては、[Dokcer CE](https://docs.docker.com/install/#server) に関するページにある説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d71d-123">Follow the instructions found on [About Dokcer CE](https://docs.docker.com/install/#server) for your specific Linux distribution.</span></span>

## <a name="validate-configuration"></a><span data-ttu-id="6d71d-124">構成を検証する</span><span class="sxs-lookup"><span data-stu-id="6d71d-124">Validate configuration</span></span>

<span data-ttu-id="6d71d-125">Docker が正常にインストールおよび構成されていることを検証するには、ターミナルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6d71d-125">To validate that Docker has been successfully installed and configured, open a terminal and run the following command:</span></span>

```bash
docker search nginx
```

<span data-ttu-id="6d71d-126">次のような出力が表示された場合、ご利用のシステムでは次のユニットのための準備ができています。</span><span class="sxs-lookup"><span data-stu-id="6d71d-126">If you see output similar to the following, your environment is read for the next unit.</span></span>

```bash
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

## <a name="summary"></a><span data-ttu-id="6d71d-127">まとめ</span><span class="sxs-lookup"><span data-stu-id="6d71d-127">Summary</span></span>

<span data-ttu-id="6d71d-128">このユニットでは、コンテナーのローカル開発環境の準備を整えました。</span><span class="sxs-lookup"><span data-stu-id="6d71d-128">In this unit, you prepared a local container development environment.</span></span> <span data-ttu-id="6d71d-129">次のユニットでは、基本的な Docker 操作のいくつかについて説明します。</span><span class="sxs-lookup"><span data-stu-id="6d71d-129">In the next unit, you will learn about some basic Docker operations.</span></span>