<span data-ttu-id="db1bc-101">これで機能しているコンテナーの開発環境が用意できたので、コンテナーを実行、一覧表示、および削除する一般的な方法をいくつか見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="db1bc-101">Now that you have a functioning container development environment, let's look at some common ways to run, list, and delete containers.</span></span>

<span data-ttu-id="db1bc-102">ここでは、次の方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-102">Here, you'll learn how to:</span></span>

* <span data-ttu-id="db1bc-103">基本のコンテナーを実行する。</span><span class="sxs-lookup"><span data-stu-id="db1bc-103">Run basic containers.</span></span>
* <span data-ttu-id="db1bc-104">コンテナー イメージをダウンロードする。</span><span class="sxs-lookup"><span data-stu-id="db1bc-104">Download container images.</span></span>
* <span data-ttu-id="db1bc-105">コンテナーとそれに関連付けられているイメージを削除する。</span><span class="sxs-lookup"><span data-stu-id="db1bc-105">Delete containers and their associated images.</span></span>

## <a name="whats-a-container-image"></a><span data-ttu-id="db1bc-106">コンテナー イメージとは?</span><span class="sxs-lookup"><span data-stu-id="db1bc-106">What's a container image?</span></span>

<span data-ttu-id="db1bc-107">コンテナー _イメージ_には、ベース オペレーティング システムと、任意の追加プロセス、アプリケーション、および構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-107">A container _image_ includes the base operating system and any additional processes, applications, and configurations.</span></span> <span data-ttu-id="db1bc-108">仮想マシンと同様に、_コンテナー_はイメージの実行中のインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="db1bc-108">Much like a virtual machine, a _container_ is a running instance of an image.</span></span>

## <a name="connect-to-your-linux-vm"></a><span data-ttu-id="db1bc-109">ご利用の Linux VM に接続する</span><span class="sxs-lookup"><span data-stu-id="db1bc-109">Connect to your Linux VM</span></span>

<span data-ttu-id="db1bc-110">前回のパートで作成した SSH セッションから切断した場合、ログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="db1bc-110">If you disconnected from the SSH session you created in the last part, you will need to log in.</span></span> <span data-ttu-id="db1bc-111">念のためその方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-111">Here's a refresher.</span></span>

1. <span data-ttu-id="db1bc-112">IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-112">Get the IP address.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. <span data-ttu-id="db1bc-113">VM に SSH で接続します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-113">SSH into the VM.</span></span> <span data-ttu-id="db1bc-114">**ip-address** を、実際に使用する値に置換します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-114">Replace **ip-address** with yours.</span></span>

    ```bash
    ssh azureuser@ip-address
    ```

## <a name="create-and-run-a-basic-container"></a><span data-ttu-id="db1bc-115">基本のコンテナーを作成して実行する</span><span class="sxs-lookup"><span data-stu-id="db1bc-115">Create and run a basic container</span></span>

<span data-ttu-id="db1bc-116">ここでは、Alpine Linux に基づくコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-116">Here you'll run a container that's based on Alpine Linux.</span></span> <span data-ttu-id="db1bc-117">Alpine Linux はそのサイズが理由でよく使用されます。コンテナー イメージのサイズを 5 MB 程度の小ささにすることができます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-117">Alpine Linux is popular because of its size &mdash; container images can be as small as 5 MB.</span></span>

<span data-ttu-id="db1bc-118">この `docker run` コマンドを実行して、Alpine Linux に基づくコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-118">Run this `docker run` command to create a container based on Alpine Linux.</span></span>

```bash
docker run alpine echo "Hello World"
```

<span data-ttu-id="db1bc-119">次のような結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-119">You see output similar to this:</span></span>

```output
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
8e3ba11ec2a2: Pull complete
Digest: sha256:7043076348bf5040220df6ad703798fd8593a0918d06d3ce30c6c93be117e430
Status: Downloaded newer image for alpine:latest
Hello World
```

<span data-ttu-id="db1bc-120">`docker run` コマンドでは、コンテナーが作成され、指定されたコマンドが実行され、さらにコンテナーが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-120">The `docker run` command creates a container, runs the provided command, and then destroys the container.</span></span>

<span data-ttu-id="db1bc-121">ここでは、Docker によって、[alpine](https://hub.docker.com/_/alpine?azure-portal=true) イメージが Docker Hub からダウンロードされ、コンテナーが起動され、"Hello World" がコンソールに出力されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-121">Here, Docker downloads the [alpine](https://hub.docker.com/_/alpine?azure-portal=true) image from Docker Hub, starts the container, and then prints "Hello World" to the console.</span></span>

<span data-ttu-id="db1bc-122">この場合、`echo` コマンドが一時的に実行されてから終了します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-122">In this case, the `echo` command runs briefly and then exits.</span></span> <span data-ttu-id="db1bc-123">前のパートでは、Nginx がフォアグラウンドで実行され、これによって、ご利用のコンテナーは、停止されるか、または Nginx サービスが停止されるまでアライブの状態に維持されています。</span><span class="sxs-lookup"><span data-stu-id="db1bc-123">In the previous part, Nginx runs in the foreground and therefore keeps your container alive until you stop the container or stop the Nginx service.</span></span>

## <a name="get-container-images"></a><span data-ttu-id="db1bc-124">コンテナー イメージを取得する</span><span class="sxs-lookup"><span data-stu-id="db1bc-124">Get container images</span></span>

<span data-ttu-id="db1bc-125">コンテナー イメージは、コンテナー イメージ レジストリに格納されています。</span><span class="sxs-lookup"><span data-stu-id="db1bc-125">Container images are stored in a container image registry.</span></span> <span data-ttu-id="db1bc-126">前の例では、Docker のパブリック コンテナー レジストリである Docker Hub から **alpine** イメージが Docker によってプルされています。</span><span class="sxs-lookup"><span data-stu-id="db1bc-126">In the previous example, Docker pulls the **alpine** image from Docker Hub, Docker's public container registry.</span></span>

<span data-ttu-id="db1bc-127">`docker images` を実行して、ご自分のシステムにダウンロードされているイメージの一覧を確認します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-127">Run `docker images` to see a list of images that have been downloaded to your system.</span></span>

```bash
docker images
```

<span data-ttu-id="db1bc-128">&mdash; **alpine** と **nginx** という 2 つのイメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-128">You see two images &mdash; **alpine** and **nginx**.</span></span>

```output
REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
alpine              latest              196d12cf6ab1        8 days ago4.41MB
nginx               latest              06144b287844        2 weeks ago109MB
```

<span data-ttu-id="db1bc-129">コンテナー イメージを検索するには、`docker search` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-129">To search for a container image, use the `docker search` command.</span></span> <span data-ttu-id="db1bc-130">たとえば、次を実行すると、名前に `nginx` が含まれるコンテナー イメージがすべて一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-130">For example, run the following to list all container images that include `nginx` in their names.</span></span>

```bash
docker search nginx --limit 5
```

<span data-ttu-id="db1bc-131">この例では、引数 `--limit` によって、検索操作が最初の 5 つの結果のみに制限されています。</span><span class="sxs-lookup"><span data-stu-id="db1bc-131">The `--limit` argument in this example restricts the search operation to the first five results.</span></span>

<span data-ttu-id="db1bc-132">結果は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="db1bc-132">You see something similar to this.</span></span>

```output
NAME                                     DESCRIPTION         STARS               OFFICIAL            AUTOMATED
nginx                                    Official build of Nginx.         9672                [OK]
jwilder/nginx-proxy                      Automated Nginx reverse proxy for docker con…   1415                                    [OK]
richarvey/nginx-php-fpm                  Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion   LetsEncrypt container to use with nginx as p…   411                                     [OK]
bitnami/nginx                            Bitnami nginx Docker Image         58                                      [OK]
```

<span data-ttu-id="db1bc-133">イメージがローカルにない場合は、`docker run` などのコマンドによって、自動的にイメージがプルダウンされます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-133">Commands like `docker run` pull down the image for you if you don't have it locally.</span></span> <span data-ttu-id="db1bc-134">しかし、イメージは使用する前にダウンロードするのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="db1bc-134">But it's common to download an image before you use it.</span></span> <span data-ttu-id="db1bc-135">これにより、確実に最新バージョンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-135">This ensures you have the latest version.</span></span>

<span data-ttu-id="db1bc-136">そのためには、`docker pull` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-136">To do so, you use the `docker pull` command.</span></span> <span data-ttu-id="db1bc-137">次を実行すると、Docker Hub から最新の **nginx** イメージがプルされます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-137">Run the following to pull the latest **nginx** image from Docker Hub.</span></span>

```bash
docker pull nginx
```

<span data-ttu-id="db1bc-138">この例では、最新バージョンの **nginx** イメージが取得されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="db1bc-138">This example shows that you have the latest version of the **nginx** image.</span></span>

```output
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
Status: Image is up to date for nginx:latest
```

## <a name="run-and-list-a-container"></a><span data-ttu-id="db1bc-139">コンテナーを実行して一覧表示する</span><span class="sxs-lookup"><span data-stu-id="db1bc-139">Run and list a container</span></span>

<span data-ttu-id="db1bc-140">前のパートでは、`docker run` コマンドを使用して Nginx を起動しました。</span><span class="sxs-lookup"><span data-stu-id="db1bc-140">In the previous part, you used the `docker run` command to bring up Nginx.</span></span> <span data-ttu-id="db1bc-141">そのコマンドをもう一度実行して、何が起こるかを詳しく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="db1bc-141">Let's run that command a second time and take a closer look into what's happening.</span></span>

1. <span data-ttu-id="db1bc-142">ご利用の SSH 接続から、このコマンドを実行して Nginx を実行するコンテナーを起動します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-142">From your SSH connection, run this command to bring up a container running Nginx.</span></span>

    ```bash
    docker run -d -p 8080:80 nginx
    ```

    * <span data-ttu-id="db1bc-143">引数 `-d` では、コンテナーがデタッチ モードで実行されること、つまり、コンテナーがバックグラウンドで実行されることが指定されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-143">The `-d` argument specifies that the container will run in a detached mode, which means the container runs in the background.</span></span> <span data-ttu-id="db1bc-144">Nginx プロセスが停止またはクラッシュした場合には、コンテナー自体も停止されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-144">If the Nginx process stops or crashes, the container itself is also stopped.</span></span>
    * <span data-ttu-id="db1bc-145">引数 `-p` では、コンテナー ホスト (この場合はご自分の Linux VM) のポート 8080 で受信されるネットワーク トラフィックが、コンテナー上のポート 80 に転送されることが指定されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-145">The `-p` argument specifies that network traffic arriving to port 8080 on the container host, your Linux VM in this case, is forwarded to port 80 on the container.</span></span> <span data-ttu-id="db1bc-146">前のパートではこの引数を使用して、ご利用のブラウザーから Web サーバーにアクセスしました。</span><span class="sxs-lookup"><span data-stu-id="db1bc-146">You used this argument in the previous part to access the web server from your browser.</span></span>
    * <span data-ttu-id="db1bc-147">引数 `nginx` は実行するコンテナー イメージの名前です。</span><span class="sxs-lookup"><span data-stu-id="db1bc-147">The `nginx` argument is the name of the container image to run.</span></span>

    <span data-ttu-id="db1bc-148">`docker run` オプションの完全な一覧については、後で「[docker run reference](https://docs.docker.com/engine/reference/run?azure-portal=true)」 (Docker run リファレンス) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="db1bc-148">Later, you can explore the complete list of `docker run` options in the [docker run reference](https://docs.docker.com/engine/reference/run?azure-portal=true).</span></span>

    <span data-ttu-id="db1bc-149">`docker run` コマンドでは、コンテナーの一意の識別子が表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-149">The `docker run` command displays a unique identifier for the container.</span></span> <span data-ttu-id="db1bc-150">例:</span><span class="sxs-lookup"><span data-stu-id="db1bc-150">For example:</span></span>

    ```output
    9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466
    ```

1. <span data-ttu-id="db1bc-151">`docker ps` コマンドを実行して、ご利用のシステム上で実行されているコンテナーを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-151">Run `docker ps` to list the running containers on your system.</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="db1bc-152">開始したばかりの Nginx コンテナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-152">You see the Nginx container you just started.</span></span> <span data-ttu-id="db1bc-153">コンテナーに ID と名前の両方があることに注意します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-153">Notice that the container has both an ID and a name.</span></span> <span data-ttu-id="db1bc-154">これらの値のいずれかを使用して、コンテナーを管理できます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-154">You can use either one of these values to manage the container.</span></span> <span data-ttu-id="db1bc-155">コンテナー ID を書き留めます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-155">Note the container ID.</span></span> <span data-ttu-id="db1bc-156">これは後で使用します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-156">You'll use it later.</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS              PORTS                  NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   4 minutes ago     Up 4 minutes        0.0.0.0:8080->80/tcp   compassionate_allen
    ```

1. <span data-ttu-id="db1bc-157">前に行ったように、ブラウザーでご利用の VM の IP アドレスに移動することで、実行中の Web サーバーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-157">As you did previously, you can navigate to your VM's IP address in a browser to see the running web server.</span></span> <span data-ttu-id="db1bc-158">URL の一部として、ポート 8080 を指定することを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="db1bc-158">Don't forget to specify port 8080 as part of the URL.</span></span>

    ![ブラウザーで実行されている Web サイト](../media/2-nginx-browser.png)

## <a name="delete-containers"></a><span data-ttu-id="db1bc-160">コンテナーを削除する</span><span class="sxs-lookup"><span data-stu-id="db1bc-160">Delete containers</span></span>

<span data-ttu-id="db1bc-161">コンテナーを削除するには、`docker rm` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-161">You use the `docker rm` command to delete a container.</span></span> <span data-ttu-id="db1bc-162">コンテナーは、その名前と ID で指定することができます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-162">You can specify the container by its name or ID.</span></span>

1. <span data-ttu-id="db1bc-163">`docker rm` を実行して、Nginx を実行しているご自分のコンテナーを削除してみてください。</span><span class="sxs-lookup"><span data-stu-id="db1bc-163">Try running `docker rm` to delete your container running Nginx.</span></span> <span data-ttu-id="db1bc-164">ID を検索する必要がある場合は、`docker ps` を実行してください。</span><span class="sxs-lookup"><span data-stu-id="db1bc-164">Run `docker ps` if you need to find the ID.</span></span> <span data-ttu-id="db1bc-165">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-165">Here's an example.</span></span>

    ```bash
    docker rm 9d7327e31212
    ```

    <span data-ttu-id="db1bc-166">コンテナーは実行中なので、削除できないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="db1bc-166">You see that the container can't be removed because it's in the running state.</span></span>

    ```output
    Error response from daemon: You cannot remove a running container 9d7327e3121200cfe2dccb627544db1442a4e02ed3151f30681db4e538ef9466. Stop the container before attempting removal or force remove
    ```

1. <span data-ttu-id="db1bc-167">`docker stop` を実行して、コンテナーを停止します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-167">Run `docker stop` to stop the container.</span></span> <span data-ttu-id="db1bc-168">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-168">Here's an example.</span></span>

    ```bash
    docker stop 9d7327e31212
    ```
    
1. <span data-ttu-id="db1bc-169">`docker ps` を実行して、コンテナーがもう実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-169">Run `docker ps` to verify that the container is no longer running.</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="db1bc-170">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-170">You see this.</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATEDSTATUS              PORTS               NAMES
    ```

1. <span data-ttu-id="db1bc-171">`docker ps` をもう一度実行しますが、今回は `-a` フラグを指定します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-171">Run `docker ps` a second time, but this time provide th `-a` flag.</span></span> <span data-ttu-id="db1bc-172">この場合は、停止されていても、すべてのコンテナーが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-172">This lists all containers, even those that are stopped.</span></span>

    ```bash
    docker ps -a
    ```

    <span data-ttu-id="db1bc-173">次のような結果が出力されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-173">You see output similar to this:</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND                  CREATED     STATUS                          PORTS               NAMES
    9d7327e31212        nginx               "nginx -g 'daemon of…"   11 minutes ago     Exited (0) About a minute ago                       compassionate_allen
    df8aeea0320f        alpine              "echo 'Hello World'"     About an hour ago   Exited (0) About an hour ago                        objective_payne
    3a7efb75c288        nginx               "nginx -g 'daemon of…"   About an hour ago   Exited (0) About an hour ago                        nginx
    ```

    <span data-ttu-id="db1bc-174">出力には、停止したばかりの Nginx コンテナーに加えて、以前実行した他のコンテナーも含まれます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-174">The output includes the Nginx container you just stopped as well as the other containers you ran prior.</span></span>

1. <span data-ttu-id="db1bc-175">`docker rm` をもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-175">Run `docker rm` a second time.</span></span> <span data-ttu-id="db1bc-176">表示されている ID をご自分の ID のいずれかに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-176">Replace the ID shown with one of yours.</span></span>

    ```bash
    docker rm 9d7327e31212
    ```

1. <span data-ttu-id="db1bc-177">次の `docker rm` コマンドを実行すると、アクティブなコンテナーが_すべて_削除されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-177">Run the following `docker rm` command to delete _all_ active containers.</span></span>

    ```bash
    docker rm $(docker ps -aq)
    ```

1. <span data-ttu-id="db1bc-178">`docker ps -a` をもう一度実行します。すると、実行中または停止中であるアクティブなコンテナーが存在しないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="db1bc-178">Run `docker ps -a` again and you see that there are no active containers running or stopped.</span></span>

    ```bash
    docker ps -a
    ```

## <a name="delete-a-container-image"></a><span data-ttu-id="db1bc-179">コンテナー イメージを削除する</span><span class="sxs-lookup"><span data-stu-id="db1bc-179">Delete a container image</span></span>

<span data-ttu-id="db1bc-180">コンテナー イメージを削除するには、`docker rmi` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-180">You use the `docker rmi` command to delete a container image.</span></span>

<span data-ttu-id="db1bc-181">イメージの削除を行うことができるのは、そのイメージから開始されたコンテナー (実行中または停止中) がアクティブでない場合のみです。</span><span class="sxs-lookup"><span data-stu-id="db1bc-181">You can only delete an image if no container started from that image, running or stopped, is active.</span></span> <span data-ttu-id="db1bc-182">ただし、引数 `-f` を指定すると、関連付けられたすべてのコンテナーの削除が強制的に行われ、次いでコンテナー イメージが削除されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-182">However, the `-f` argument forces the removal of all associated containers and will then remove the container image.</span></span>

1. <span data-ttu-id="db1bc-183">`docker rmi nginx` を実行して、ご利用の Linux VM から Nginx イメージを削除します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-183">Run `docker rmi nginx` to remove the Nginx image from your Linux VM.</span></span>

    ```bash
    docker rmi nginx
    ```

    <span data-ttu-id="db1bc-184">表示される出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="db1bc-184">The output you see resembles this:</span></span>

    ```output
    Untagged: nginx:latest
    Untagged: nginx@sha256:24a0c4b4a4c0eb97a1aabb8e29f18e917d05abfe1b7a7c07857230879ce7d3d3
    Deleted: sha256:06144b2878448774e55577ae7d66b5f43a87c2e44322b3884e4e6c70d070b262
    Deleted: sha256:824a442ee3c96744d75be3737a22cc6a47aebad1b30be67f3c2f8f29cb0aa879
    Deleted: sha256:8e3d1e9e4945f930c93c30617512998437f6edafd86676770d29b1581f2520bb
    Deleted: sha256:8b15606a9e3e430cb7ba739fde2fbb3734a19f8a59a825ffa877f9be49059817
    ```

1. <span data-ttu-id="db1bc-185">`docker images` を実行して、ご利用の VM 上のイメージを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-185">Run `docker images` to list the images on your VM.</span></span> 

    ```bash
    docker images
    ```

    <span data-ttu-id="db1bc-186">**alpine** イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db1bc-186">You see the **alpine** image.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATEDSIZE
    alpine              latest              196d12cf6ab1        8 days ago4.41MB
    ```

1. <span data-ttu-id="db1bc-187">次の `docker rmi` コマンドを実行して、ご利用の VM から_すべて_のイメージを削除します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-187">Run the following `docker rmi` command to delete _all_ images from your VM.</span></span>

    ```bash
    docker rmi $(docker images -q)
    ```

1. <span data-ttu-id="db1bc-188">`docker images` をもう一度実行して、VM 上にイメージがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="db1bc-188">Run `docker images` again and you see that there are no images on the VM.</span></span>

    ```bash
    docker images
    ```
