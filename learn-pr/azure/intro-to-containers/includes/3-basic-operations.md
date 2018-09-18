<span data-ttu-id="6ef50-101">これで機能しているコンテナーの開発環境ができたので、いくつかの基本的なコンテナー操作をすばやく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6ef50-101">Now that you have a functioning container development environment, let's take a quick spin through some basic container operations.</span></span> <span data-ttu-id="6ef50-102">このユニットには、Docker のすべての機能一覧は含まれていません (すべてにはほど遠いです)。</span><span class="sxs-lookup"><span data-stu-id="6ef50-102">This unit doesn't include a complete list of Docker capabilities (not even close).</span></span> <span data-ttu-id="6ef50-103">このユニットでは、コンテナーの実行、列挙、および削除を準備できます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-103">This unit will prepare you to run, list, and delete containers.</span></span> <span data-ttu-id="6ef50-104">このモジュールの残りの部分では、コンテナーの操作についてさらに説明します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-104">Throughout the remainder of this module, you will gain additional exposure to container operations.</span></span>

## <a name="run-a-basic-container"></a><span data-ttu-id="6ef50-105">基本のコンテナーを実行する</span><span class="sxs-lookup"><span data-stu-id="6ef50-105">Run a basic container</span></span>

<span data-ttu-id="6ef50-106">コンテナーの実行と管理の詳細を掘り下げる前に、コンテナーを実行するのがいかに簡単であるかをすばやく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6ef50-106">Before digging into the details of running and managing containers, let's quickly see just how easy it is to run a container.</span></span>

<span data-ttu-id="6ef50-107">次のコマンドを使用して最初のコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-107">Create your first container with the following command.</span></span>

```bash
docker run alpine echo "Hello World"
```

<span data-ttu-id="6ef50-108">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-108">You should see output similar to the following:</span></span>

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

<span data-ttu-id="6ef50-109">`docker run` コマンドによって、コンテナーのインスタンスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-109">The `docker run` command creates an instance of a container.</span></span> <span data-ttu-id="6ef50-110">この例では、コンテナーはローカル システムにダウンロードされた `alpine` という名前のコンテナー イメージから作成されています。</span><span class="sxs-lookup"><span data-stu-id="6ef50-110">In this case, the container was created from a container image named `alpine`, which was downloaded to your local system.</span></span> <span data-ttu-id="6ef50-111">コンテナーが開始されると、コンテナー内で `echo "Hello World"` コマンドが実行され、出力がターミナルにエコーされます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-111">After the container started, the `echo "Hello World"` command was run inside of the container and the output echoed to your terminal.</span></span>

<span data-ttu-id="6ef50-112">この時点では、これらのアクションの技術的な詳細については心配しないでください。</span><span class="sxs-lookup"><span data-stu-id="6ef50-112">At this point, don't worry about the technical details of each of these actions.</span></span> <span data-ttu-id="6ef50-113">それらについては、このモジュールを通して詳述します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-113">They will be detailed throughout this module.</span></span>

## <a name="get-container-images"></a><span data-ttu-id="6ef50-114">コンテナー イメージを取得する</span><span class="sxs-lookup"><span data-stu-id="6ef50-114">Get container images</span></span>

<span data-ttu-id="6ef50-115">'Hello World' の例で見たとおり、コンテナーはコンテナー イメージから実行されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-115">As you saw in the 'Hello World' example, containers are run from a container image.</span></span> <span data-ttu-id="6ef50-116">これらのイメージには、コンテナー ベースのオペレーティング システムと、追加のプロセス、アプリケーションおよび構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-116">These images include the container base operating system and any additional processes, applications, and configurations.</span></span> <span data-ttu-id="6ef50-117">コンテナー イメージは、コンテナー イメージ レジストリに格納されています。</span><span class="sxs-lookup"><span data-stu-id="6ef50-117">Container images are stored in a container image registry.</span></span> <span data-ttu-id="6ef50-118">'Hello World' の例では、*alpine* のイメージは、パブリック コンテナーのレジストリである Docker Hub からプルされています。</span><span class="sxs-lookup"><span data-stu-id="6ef50-118">In the 'Hello World' example, the *alpine* image was pulled from Docker Hub, which is a public container registry.</span></span>

<span data-ttu-id="6ef50-119">事前に作成したコンテナー イメージを検索してダウンロードする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6ef50-119">Let's see how to search for and download a pre-created container image.</span></span>

<span data-ttu-id="6ef50-120">次のコマンドを実行して、ご自分のシステムにダウンロードされているイメージの一覧を確認します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-120">Run the following command to see a list of images that have been downloaded to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="6ef50-121">手順に従ってきた場合には、alpine のイメージが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="6ef50-121">If you've been following along, you should see the alpine image.</span></span> <span data-ttu-id="6ef50-122">このイメージは、'Hello World' の例の実行時にダウンロードされています。</span><span class="sxs-lookup"><span data-stu-id="6ef50-122">This image was downloaded when the 'Hello World' example was run.</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

<span data-ttu-id="6ef50-123">コンテナー イメージを検索するには、`docker search` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-123">To search for a container image, use the `docker search` command.</span></span> <span data-ttu-id="6ef50-124">たとえば、次の例を使用して、名前に `nginx` が含まれるすべてのコンテナー イメージを列挙します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-124">For instance, use the following example to list all container images that include `nginx` in the name.</span></span>

```bash
docker search nginx
```

<span data-ttu-id="6ef50-125">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-125">The output should look similar to the following:</span></span>

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

<span data-ttu-id="6ef50-126">イメージを実行する前に事前にダウンロードしたい場合、`docker pull` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-126">If you'd like to pre-download an image prior to running it, use the `docker pull` command.</span></span> <span data-ttu-id="6ef50-127">次の例では、システムに *nginx* イメージがプルされています。</span><span class="sxs-lookup"><span data-stu-id="6ef50-127">The following example pulls the *nginx* image to your system.</span></span>

```bash
docker pull nginx
```

<span data-ttu-id="6ef50-128">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-128">The output should look similar to the following:</span></span>

```output
Using default tag: latest
latest: Pulling from library/nginx
be8881be8156: Pull complete
f2f27ed9664f: Extracting [===============>                                   ]  6.652MB/22.14MB
54ff137eb1b2: Download complete
```

<span data-ttu-id="6ef50-129">システム上のすべてのイメージの一覧に対して、`docker images` を再度実行します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-129">Run `docker images` again to list all of the images on your system.</span></span> <span data-ttu-id="6ef50-130">*nginx* イメージがご自分のシステムに追加されたのをご確認いただけます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-130">You'll see that the *nginx* image has been added to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="6ef50-131">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-131">The output should look similar to the following:</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c82521676580        26 hours ago        109MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

## <a name="run-containers"></a><span data-ttu-id="6ef50-132">コンテナーを実行する</span><span class="sxs-lookup"><span data-stu-id="6ef50-132">Run containers</span></span>

<span data-ttu-id="6ef50-133">*nginx* を識別してダウンロードできたので、イメージからコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-133">Now that you've identified and downloaded the *nginx* image, run a container from the image.</span></span> <span data-ttu-id="6ef50-134">コンテナー イメージの実行に Docker CLI を使用する場合、`docker run` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-134">When using the Docker CLI to run a container image, use the `docker run` command.</span></span>

<span data-ttu-id="6ef50-135">次の例では、`-d` 引数によって、コンテナーがデタッチ モードで実行されていることが指定されています。</span><span class="sxs-lookup"><span data-stu-id="6ef50-135">In the following example, the `-d` argument specifies that the container will run in a detached mode.</span></span> <span data-ttu-id="6ef50-136">この構成では、コンテナーは指定されたプロセスを実行します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-136">In this configuration, the container runs a specified process.</span></span> <span data-ttu-id="6ef50-137">そのプロセスが停止またはクラッシュした場合には、コンテナー自体が停止されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-137">If that process stops or crashes, the container itself is stopped.</span></span> <span data-ttu-id="6ef50-138">引数 `-p 8080:80` は、コンテナー ホスト (ここではご自分の開発システム) のポート 8080 で受信されるネットワーク トラフィックが、コンテナーのポート 80 に転送されることを指定します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-138">The `-p 8080:80` argument specifies that network traffic arriving to port 8080 on the container host, your development system in this case, is forwarded to port 80 of the container.</span></span> <span data-ttu-id="6ef50-139">最後に、`ngingx` 引数は実行するコンテナー イメージの名前です。</span><span class="sxs-lookup"><span data-stu-id="6ef50-139">Finally, the `ngingx` argument is the name of the container image to run.</span></span>

<span data-ttu-id="6ef50-140">`docker run` の引数の完全な一覧については、「[docker run reference](https://docs.docker.com/engine/reference/run/)」 (Docker run リファレンス) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ef50-140">For a complete list of `docker run` arguments, see the [docker run reference](https://docs.docker.com/engine/reference/run/).</span></span>

```bash
docker run -d -p 8080:80 nginx
```

<span data-ttu-id="6ef50-141">この操作によって、完全なコンテナー ID が返されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-141">This operation returns the full container ID.</span></span>

```output
bd2424bfe7a5423d7d65efdf0b1622770d59e212db7b82862c3129fb630b5721
```

<span data-ttu-id="6ef50-142">`docker ps` コマンドを使用して、システムで実行されているコンテナーを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-142">List the running containers on your system using the `docker ps` command.</span></span>

```bash
docker ps
```

<span data-ttu-id="6ef50-143">最後の手順で実行した NGINX コンテナーが、唯一実行されているコンテナーとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-143">You should see a single running container, which is the NGINX container run in the last step.</span></span> <span data-ttu-id="6ef50-144">コンテナーに ID と名前の両方があることに注意します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-144">Notice that the container has both an ID and a Name.</span></span> <span data-ttu-id="6ef50-145">コンテナーの管理には、これらの値のいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-145">Either one of these values can be used to manage the container.</span></span> <span data-ttu-id="6ef50-146">コンテナー ID を書き留めます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-146">Take note of the container ID.</span></span> <span data-ttu-id="6ef50-147">この値は、後のユニットで使用します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-147">This value will be used later in the unit.</span></span>

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 37 minutes       0.0.0.0:8080->80/tcp   gallant_engelbart
```

<span data-ttu-id="6ef50-148">コンテナーをテストするには、ブラウザーを開き、アドレスに「 http://localhost:8080」と入力します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-148">To test the container, open a browser and enter http://localhost:8080 for the address.</span></span> <span data-ttu-id="6ef50-149">完了すると、NGINX の既定の Web サイトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-149">After it completes, you should see the NGINX default website.</span></span>

![NGINX のスプラッシュ スクリーンが表示された Microsoft Edge](../media-draft/3-nginx.png)

## <a name="delete-containers"></a><span data-ttu-id="6ef50-151">コンテナーを削除する</span><span class="sxs-lookup"><span data-stu-id="6ef50-151">Delete containers</span></span>

<span data-ttu-id="6ef50-152">コンテナーをもう使用しない場合、`docker rm` コマンドにコンテナー名および ID を入力して削除することができます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-152">When you're done working with a container, it can be deleted by providing the container name or ID to the `docker rm` command.</span></span> <span data-ttu-id="6ef50-153">この操作は、NGINX を実行するコンテナーのコンテナー ID を使用して試してください。</span><span class="sxs-lookup"><span data-stu-id="6ef50-153">Try out this operation with the container ID of the container running NGINX.</span></span> <span data-ttu-id="6ef50-154">この例の ID をご自分の環境の ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-154">Replace the ID in this example with the ID from your environment.</span></span>

```bash
docker rm bd2424bfe7a5
```

<span data-ttu-id="6ef50-155">コンテナーは実行中なので、削除できません。</span><span class="sxs-lookup"><span data-stu-id="6ef50-155">Notice that the container can't be removed because it's in a running state.</span></span>

```output
Error response from daemon: You cannot remove a running container a31c5a5f2a8d6e420435bfcadbe158fa6a26ed29c005a892171505cc0c2861b2. Stop the container before attempting removal or force remove
```

<span data-ttu-id="6ef50-156">`docker stop` コマンドを使用して、コンテナーを停止します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-156">Stop the container with the `docker stop` command.</span></span>

```bash
docker stop bd2424bfe7a5
```

<span data-ttu-id="6ef50-157">この時点で、すべてのコンテナーを列挙するために `docker ps` を実行しても、nginx コンテナーが表示されないことに注意します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-157">Notice at this point, if you run `docker ps` to list all containers, the nginx container is not listed.</span></span>

```bash
docker ps
```

<span data-ttu-id="6ef50-158">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-158">The output should look similar to the following:</span></span>

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

<span data-ttu-id="6ef50-159">停止状態のコンテナーを含むすべてのコンテナーが一覧表示されるようにするには、`-a` 引数を `docker ps` コマンドに追加します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-159">To return a list of all containers, including containers in a stopped state, add the `-a` argument to the `docker ps` command.</span></span>

```bash
docker ps -a
```

<span data-ttu-id="6ef50-160">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-160">The output should look similar to the following:</span></span>

```output
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
bd2424bfe7a5        nginx               "nginx -g 'daemon of…"   13 seconds ago      Exited (0) 3 seconds ago                       focused_spence
```

<span data-ttu-id="6ef50-161">削除操作を再実行します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-161">Try the delete operation again.</span></span> <span data-ttu-id="6ef50-162">この例の ID をご自分の環境の ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-162">Replace the ID in this example with the ID from your environment.</span></span>

```bash
docker rm bd2424bfe7a5
```

<span data-ttu-id="6ef50-163">この操作により、コンテナーの ID が返されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-163">This operation returns the container ID.</span></span>

```output
bd2424bfe7a5
```

## <a name="delete-a-container-image"></a><span data-ttu-id="6ef50-164">コンテナー イメージを削除する</span><span class="sxs-lookup"><span data-stu-id="6ef50-164">Delete a container image</span></span>

<span data-ttu-id="6ef50-165">コンテナー イメージでの作業が終わったら、`docker rmi` を使用して削除することができます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-165">When you're done working with a container image, it can be removed with the `docker rmi` command.</span></span> <span data-ttu-id="6ef50-166">(実行および停止中の) 任意のコンテナーがコンテナー イメージから起動されている場合、そのイメージは削除できません。</span><span class="sxs-lookup"><span data-stu-id="6ef50-166">If any container (running or stopped) has been started from the container image, the image can't be deleted.</span></span> <span data-ttu-id="6ef50-167">最初にコンテナーを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-167">The containers first need to be removed.</span></span> <span data-ttu-id="6ef50-168">`-f` 引数を `docker rmi` コマンドに追加すると、関連するすべてのコンテナーの削除が強制され、次いでコンテナー イメージが削除されます。</span><span class="sxs-lookup"><span data-stu-id="6ef50-168">Adding the `-f` argument to the `docker rmi` command will force the removal of all associated containers, and will then remove the container image.</span></span>

<span data-ttu-id="6ef50-169">次のコマンドを使用して、NGINX コンテナー イメージを削除します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-169">Remove the NGINX container image with the following command.</span></span>

```bash
docker rmi nginx
```

<span data-ttu-id="6ef50-170">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ef50-170">The output should look similar to the following:</span></span>

```output
Untagged: nginx:latest
Untagged: nginx@sha256:4a5573037f358b6cdfa2f3e8a9c33a5cf11bcd1675ca72ca76fbe5bd77d0d682
Deleted: sha256:8b89e48b5f157d9455c963b57c85d21e2337c58b8c983bc06f88476610adc129
Deleted: sha256:119ded3eca5e85ef43ee966e74564c604ccda064d955a8c5ed762e1d5e87f428
Deleted: sha256:6ece91c2763d826487e707f7b8ec063742ad0ee56cc9e605465cce95550c9a7f
Deleted: sha256:cdb3f9544e4c61d45da1ea44f7d92386639a052c620d1550376f22f5b46981af
```

## <a name="summary"></a><span data-ttu-id="6ef50-171">まとめ</span><span class="sxs-lookup"><span data-stu-id="6ef50-171">Summary</span></span>

<span data-ttu-id="6ef50-172">このユニットでは、Docker の基本的な操作をいくつか学習します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-172">In this unit, you learned about some basic Docker operations.</span></span> <span data-ttu-id="6ef50-173">次のユニットでは、カスタム コンテナー イメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ef50-173">In the next unit, you will create a custom container image.</span></span>