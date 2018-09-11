<span data-ttu-id="211d3-101">直前のユニットでは、あらかじめ作成されているコンテナー イメージを使用して、基本的な Docker 操作をいくつか実行しました。</span><span class="sxs-lookup"><span data-stu-id="211d3-101">In the last unit, you worked with pre-created container images to perform some basic Docker operations.</span></span> <span data-ttu-id="211d3-102">このユニットでは、カスタム コンテナー イメージを作成し、それらのイメージをパブリック コンテナー レジストリにプッシュし、それらのイメージからコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="211d3-102">In this unit, you will create custom container images, push these images to a public container registry, and run containers from these images.</span></span>

<span data-ttu-id="211d3-103">コンテナー イメージは手動で作成することも、いわゆる Dockerfile を使用してその作成プロセスを自動化することもできます。</span><span class="sxs-lookup"><span data-stu-id="211d3-103">Container images can be created by hand or using what's called a Dockerfile to automate the process.</span></span> <span data-ttu-id="211d3-104">お勧めするのは Dockerfile を使用した方法ですが、このユニットでは両方の方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="211d3-104">The preferred method is using a Dockerfile, but this unit will demonstrate both methods.</span></span> <span data-ttu-id="211d3-105">手動プロセスを理解することで、Dockerfile を使用して自動化を行う場合に何が行われているのかをよく理解できるようにすることを意図しています。</span><span class="sxs-lookup"><span data-stu-id="211d3-105">The intention is that having an understanding of the manual process will help you to better understand what's occurring when using a Dockerfile for automation.</span></span>

## <a name="manual-image-creation"></a><span data-ttu-id="211d3-106">手動でのイメージの作成</span><span class="sxs-lookup"><span data-stu-id="211d3-106">Manual image creation</span></span>

<span data-ttu-id="211d3-107">コンテナー イメージを手動で作成する場合は、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="211d3-107">When manually creating a container image, the following actions are taken:</span></span>

- <span data-ttu-id="211d3-108">コンテナーのインスタンスを起動します。</span><span class="sxs-lookup"><span data-stu-id="211d3-108">Start an instance of a container.</span></span>
- <span data-ttu-id="211d3-109">コンテナーとのターミナル セッションを確立します。</span><span class="sxs-lookup"><span data-stu-id="211d3-109">Establish a terminal session with the container.</span></span>
- <span data-ttu-id="211d3-110">ソフトウェアのインストールおよび構成の変更を行うことにより、コンテナーを変更します。</span><span class="sxs-lookup"><span data-stu-id="211d3-110">Modify the container by installing software and making configuration changes.</span></span>
- <span data-ttu-id="211d3-111">`docker capture` コマンドを使用して、コンテナーを新しいイメージに取り込みます。</span><span class="sxs-lookup"><span data-stu-id="211d3-111">Capturing the container into a new image using the `docker capture` command.</span></span>

<span data-ttu-id="211d3-112">この最初の例では、Python を実行しているコンテナーのインスタンスを起動し、hello world アプリケーションを作成してから、コンテナーを新しいイメージに取り込みます。</span><span class="sxs-lookup"><span data-stu-id="211d3-112">In this first example, you start an instance of a container that's running Python, create a hello world application, and then capture the container to a new image.</span></span>

<span data-ttu-id="211d3-113">まず、NGINX イメージからコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="211d3-113">First, run a container from the NGINX image.</span></span> <span data-ttu-id="211d3-114">このコマンドは、前のユニット実行したコマンドとは少し異なります。</span><span class="sxs-lookup"><span data-stu-id="211d3-114">This command looks a bit different from the commands that you ran in the previous unit.</span></span> <span data-ttu-id="211d3-115">実行中のコンテナーとのターミナル セッションを確立するので、`-t` および `-i` 引数を指定します。</span><span class="sxs-lookup"><span data-stu-id="211d3-115">Because you want to establish a terminal session with the running container, the `-t` and `-i` arguments are provided.</span></span> <span data-ttu-id="211d3-116">これらの引数の組み合わせによって、実行状態のままで保持される疑似ターミナルを割り当てるように Docker は指示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-116">Together, these arguments instruct Docker to allocate a pseudo terminal that will remain in a runnings state.</span></span> <span data-ttu-id="211d3-117">つまり、`-t` および `-i` 引数によって、実行中のコンテナーとの対話型セッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-117">In other words, the `-t` and `-i` arguments create an interactive session with the running container.</span></span>

<span data-ttu-id="211d3-118">次に、`python` コンテナー イメージが使用されると共にコンテナー内で実行されるプロセスが `bash` となるように指定します。</span><span class="sxs-lookup"><span data-stu-id="211d3-118">You then specify that the `python` container image is used, and the process to run inside of the container is `bash`.</span></span>

```bash
docker run --name python-demo -ti python bash
```

<span data-ttu-id="211d3-119">コマンドの実行後、ターミナル セッションはコンテナーの擬似ターミナルに切り替えられるはずです。</span><span class="sxs-lookup"><span data-stu-id="211d3-119">After the command is run, your terminal session should switch to the containers pseudo terminal.</span></span> <span data-ttu-id="211d3-120">これはターミナルのプロンプトで確認できます。プロンプトが次に示すような内容に変化しているはずです。</span><span class="sxs-lookup"><span data-stu-id="211d3-120">This can be seen by the terminal prompt, which should have changed to something similar to the following:</span></span>

```bash
root@d8ccada9c61e:/#
```

<span data-ttu-id="211d3-121">この時点では、コンテナー内で作業しています。</span><span class="sxs-lookup"><span data-stu-id="211d3-121">At this point, you're working inside the container.</span></span> <span data-ttu-id="211d3-122">コンテナー内での作業は、仮想システムまたは物理システム内での作業によく似ていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="211d3-122">You should find that working inside a container is much like working inside a virtual or physical system.</span></span> <span data-ttu-id="211d3-123">たとえば、ファイルの一覧表示、作成、および削除、ソフトウェアのインストール、構成の変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="211d3-123">For instance, you can list, create, and delete files, install software, and make configuration changes.</span></span> <span data-ttu-id="211d3-124">この簡単な例では、Python ベースの hello world スクリプトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-124">For this simple example, a Python-based hello world script is created.</span></span> <span data-ttu-id="211d3-125">これを行うには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="211d3-125">This can be done with the following command:</span></span>

```bash
echo 'print("Hello World!")' > hello.py
```

<span data-ttu-id="211d3-126">コンテナー内にとどまったままスクリプトをテストするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="211d3-126">To test the script while you're still in the container, run the following command:</span></span>

```bash
python hello.py
```

<span data-ttu-id="211d3-127">これにより、次の出力が作成されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-127">This will produce the following output:</span></span>

```bash
Hello World!
```

<span data-ttu-id="211d3-128">スクリプトが期待どおりに機能することを確認したら、`exit` を入力してコンテナーを終了します。</span><span class="sxs-lookup"><span data-stu-id="211d3-128">When you're satisfied that the script functions as expected, exit out of the container by typing `exit`:</span></span>

```bash
exit
```

<span data-ttu-id="211d3-129">ご自分のローカル システムのターミナルに戻り、`docker ps` コマンドを使用して実行中のコンテナーをすべてリストします。</span><span class="sxs-lookup"><span data-stu-id="211d3-129">Back in the terminal of your local system, use the `docker ps` command to list all running containers:</span></span>

```bash
docker ps
```

<span data-ttu-id="211d3-130">何も実行されていないことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="211d3-130">Notice that nothing is running.</span></span> <span data-ttu-id="211d3-131">実行中のコンテナー内で `exit` を入力すると、Bash プロセスが完了し、これによりコンテナーが停止されました。</span><span class="sxs-lookup"><span data-stu-id="211d3-131">When you entered `exit` in the running container, the Bash process completed, which then stopped the container.</span></span> <span data-ttu-id="211d3-132">これは予期した動作であり、問題ありません。</span><span class="sxs-lookup"><span data-stu-id="211d3-132">This is the expected behavior and is ok.</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

<span data-ttu-id="211d3-133">`docker ps` を再度使用して、`-a` 引数を含めます。</span><span class="sxs-lookup"><span data-stu-id="211d3-133">Use `docker ps` again and include the `-a` argument.</span></span> <span data-ttu-id="211d3-134">このコマンドを実行すると、コンテナーが実行中であるかどうかに関係なく、すべてのコンテナーのリストが返されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-134">This command will return a list of all containers, regardless if they're running.</span></span>

```bash
docker ps -a
```

<span data-ttu-id="211d3-135">*python-demo* という名前を持つコンテナーの状態が *Exited* であることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="211d3-135">Notice that a container with the name *python-demo* has a status of *Exited*.</span></span> <span data-ttu-id="211d3-136">このコンテナーは、終了したばかりのコンテナーの停止状態にあるインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="211d3-136">This container is the stopped instance of the container that you just exited from.</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

<span data-ttu-id="211d3-137">このコンテナーから新しいコンテナー イメージを作成するには、`docker commit` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="211d3-137">To create a new container image from this container, use the `docker commit` command.</span></span> <span data-ttu-id="211d3-138">次の例では、*python-demo* コンテナーから *python-custom* という名前のイメージを作成するように *docker commit* が指定されています。</span><span class="sxs-lookup"><span data-stu-id="211d3-138">The following example instructs *docker commit* to create an image named *python-custom* from the *python-demo* containers.</span></span>

```bash
docker commit python-demo python-custom
```

<span data-ttu-id="211d3-139">コマンドが完了すると、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-139">After the command completes, you should see output similar to the following:</span></span>

```bash
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

<span data-ttu-id="211d3-140">ここで、`docker images` を実行して、コンテナー イメージのリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="211d3-140">Now run `docker images` to see a list of container images:</span></span>

```bash
docker images
```

<span data-ttu-id="211d3-141">これで Python のカスタム イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-141">You should now see the custom Python image.</span></span>

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

<span data-ttu-id="211d3-142">新しいイメージからコンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="211d3-142">Run a container from the new image.</span></span> <span data-ttu-id="211d3-143">また、コンテナー内で実行するコマンドまたはプロセスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="211d3-143">You also need to specify what command or process to run inside of the container.</span></span> <span data-ttu-id="211d3-144">この例では、`python hello.py` を実行します。</span><span class="sxs-lookup"><span data-stu-id="211d3-144">With this example, run `python hello.py`.</span></span>


```bash
docker run python-custom python hello.py
```

<span data-ttu-id="211d3-145">コンテナーが起動し、コンテナーから hello world メッセージが出力されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-145">The container will start and output the hello world message.</span></span> <span data-ttu-id="211d3-146">次に Python プロセスが完了し、コンテナーが停止します。</span><span class="sxs-lookup"><span data-stu-id="211d3-146">The Python process is then complete and the container stops.</span></span>

```bash
Hello World!
```

## <a name="automated-image-creation"></a><span data-ttu-id="211d3-147">イメージ作成の自動化</span><span class="sxs-lookup"><span data-stu-id="211d3-147">Automated image creation</span></span>

<span data-ttu-id="211d3-148">直前のセクションでは、コンテナー イメージを手動で作成しました。</span><span class="sxs-lookup"><span data-stu-id="211d3-148">In the last section, a container image was manually created.</span></span> <span data-ttu-id="211d3-149">この方法は 1 回限りのイメージ作成または経験に基づくイメージ作成に適していますが、運用環境では持続可能ではありません。</span><span class="sxs-lookup"><span data-stu-id="211d3-149">While this method works great for one-off or experiential image creation, it's not sustainable in a production environment.</span></span> <span data-ttu-id="211d3-150">コンテナー イメージの作成は、*Dockerfile* と `docker build` コマンドを使用して自動化することができます。</span><span class="sxs-lookup"><span data-stu-id="211d3-150">Container image creation can be automated using a *Dockerfile* and the `docker build` command.</span></span> <span data-ttu-id="211d3-151">*docker build* コマンドでは、コンテナーが起動され、*Dockerfile* 内で検出された命令が実行され、その結果が新しいコンテナー イメージに取り込まれます。</span><span class="sxs-lookup"><span data-stu-id="211d3-151">The *docker build* command essentially starts a container, runs the instructions found in the *Dockerfile*, and then captures the results to a new container image.</span></span>

<span data-ttu-id="211d3-152">*Dockerfile* という名前のファイルを作成し、次のテキストを入力します。</span><span class="sxs-lookup"><span data-stu-id="211d3-152">Create a file named *Dockerfile* and enter the following text:</span></span>

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

<span data-ttu-id="211d3-153">*FROM* ステートメントでは、イメージの作成中に使用される基本イメージが指定されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-153">The *FROM* statement specifies the base image to be used during image creations.</span></span> <span data-ttu-id="211d3-154">*RUN* ステートメントでは、コンテナー内でコマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-154">The *RUN* statement runs commands inside of the container.</span></span> <span data-ttu-id="211d3-155">*RUN* を使用すると、ソフトウェアをインストールし、構成を変更し、キャプチャ イベントの前にコンテナーをクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="211d3-155">*RUN* can be used to install software, make configuration changes, and cleanup the container before the capture event.</span></span> <span data-ttu-id="211d3-156">*CMD* ステートメントでは、コンテナーの起動時に実行する必要があるプロセスを指定します。</span><span class="sxs-lookup"><span data-stu-id="211d3-156">The *CMD* statement specifies the process that should run when a container is started.</span></span>

<span data-ttu-id="211d3-157">`docker build` コマンドを使用すると、Dockerfile 内で指定された命令を使用して新しいコンテナー イメージを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="211d3-157">Use the `docker build` command to create a new container image using the instructions specified in the Dockerfile.</span></span>

```bash
docker build -t python-dockerfile .
```

<span data-ttu-id="211d3-158">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-158">You should see output similar to the following.</span></span>

```bash
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM python
 ---> 638817465c7d
Step 2/4 : WORKDIR ./app
 ---> Running in 990d17e86466
Removing intermediate container 990d17e86466
 ---> 59a074a092cc
Step 3/4 : RUN echo 'print("Hello World!")' > hello.py
 ---> Running in aed707c53bc5
Removing intermediate container aed707c53bc5
 ---> d7f55a9d0e85
Step 4/4 : CMD python hello.py
 ---> Running in e87ec55a8d36
Removing intermediate container e87ec55a8d36
 ---> 98c39b91770f
Successfully built 98c39b91770f
Successfully tagged python-dockerfile:latest
```

<span data-ttu-id="211d3-159">`docker images` コマンドを使用すると、コンテナー イメージのリストが返されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-159">Use the `docker images` command to return a list of container images.</span></span>

```bash
docker images
```

<span data-ttu-id="211d3-160">これで、カスタム イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-160">You should now see the custom image.</span></span>

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

<span data-ttu-id="211d3-161">`docker run` コマンドを使用すると、カスタム イメージからコンテナーを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="211d3-161">Use the `docker run` command to run a container from the custom image.</span></span>

<span data-ttu-id="211d3-162">ここでは、`docker run` コマンドに引数が指定されていないことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="211d3-162">Notice here that no arguments have been provided to the `docker run` command.</span></span> <span data-ttu-id="211d3-163">コンテナー イメージを手動で作成する場合とは異なり、Dockerfile を使用すると、コンテナーの起動時に実行するコマンドを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="211d3-163">Unlike when you manually create a container image, a Dockerfile allows you to include a command to run when the container starts.</span></span> <span data-ttu-id="211d3-164">この場合、指定したコマンドは `python hello.py` です。これにより、Python スクリプトがコンテナーによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-164">In this case, the specified command is `python hello.py`, which causes the container to run the Python script.</span></span>

```bash
docker run python-dockerfile
```

<span data-ttu-id="211d3-165">コマンドを実行した後に、コンテナーの出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-165">After you run the command, you should see the container output.</span></span>

```bash
Hello World!
```

## <a name="push-the-image-to-a-public-registry"></a><span data-ttu-id="211d3-166">パブリック レジストリにイメージをプッシュする</span><span class="sxs-lookup"><span data-stu-id="211d3-166">Push the image to a public registry</span></span>

<span data-ttu-id="211d3-167">Docker Hub はコンテナーのパブリック レジストリです。</span><span class="sxs-lookup"><span data-stu-id="211d3-167">Docker Hub is a public container registry.</span></span> <span data-ttu-id="211d3-168">コンテナー イメージを Docker Hub にプッシュするには、Docker アカウントを用意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="211d3-168">In order to push container images to Docker Hub, you must have a Docker account.</span></span> <span data-ttu-id="211d3-169">必要に応じて、[Docker Hub サイト](https://hub.docker.com/) にアクセスしてアカウントを登録します。</span><span class="sxs-lookup"><span data-stu-id="211d3-169">If needed, visit the [Docker Hub site](https://hub.docker.com/) to register for an account.</span></span>

<span data-ttu-id="211d3-170">Docker Hub アカウントを用意したら、アカウント名でコンテナー イメージをタグ付けする必要があります。</span><span class="sxs-lookup"><span data-stu-id="211d3-170">After you have a Docker Hub account, the container image must be tagged with the account name.</span></span> <span data-ttu-id="211d3-171">そのためには、`docker tag` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="211d3-171">To do so, use the `docker tag` command.</span></span>

<span data-ttu-id="211d3-172">次の例では、*python-dockerfile* イメージが Docker Hub アカウント名でタグ付けされています。</span><span class="sxs-lookup"><span data-stu-id="211d3-172">In the following example, the *python-dockerfile* image is tagged with a Docker Hub account name.</span></span> <span data-ttu-id="211d3-173">`<account name>` を Docker Hub アカウント名に置換します。</span><span class="sxs-lookup"><span data-stu-id="211d3-173">Replace `<account name>` with your Docker Hub account name.</span></span>

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

<span data-ttu-id="211d3-174">次に、`docker login` コマンドを使用して Docker Hub にログインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="211d3-174">Next, make sure that you are logged into Docker Hub using the `docker login` command.</span></span>

```bash
docker login
```

<span data-ttu-id="211d3-175">最後に、`docker push` コマンドを使用して *python-dockerfile* イメージを Docker Hub にプッシュします。</span><span class="sxs-lookup"><span data-stu-id="211d3-175">Finally push the *python-dockerfile* image to Docker Hub using the `docker push` command.</span></span> <span data-ttu-id="211d3-176">`<account name>` を Docker Hub アカウント名に置換します。</span><span class="sxs-lookup"><span data-stu-id="211d3-176">Replace `<account name>` with your Docker Hub account name.</span></span>

```bash
docker push <account name>/python-dockerfile
```

<span data-ttu-id="211d3-177">コンテナー イメージが Docker Hub にアップロードされている間、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="211d3-177">While the container image is being uploaded to Docker Hub, you will see output similar to the following:</span></span>

```bash
The push refers to repository [docker.io/account/python-dockerfile]
f39073ca4d5a: Pushed
9dfcec2738a9: Pushed
ffab8273c674: Mounted from account/python
e6f6cbe5e14e: Mounted from account/python
3eb255f8a6ac: Mounted from account/python
b860b6c48eec: Mounted from account/python
1fa8778eb779: Mounted from account/python
fa0c3f992cbd: Mounted from account/python
ce6466f43b11: Mounted from account/python
719d45669b35: Mounted from account/python
3b10514a95be: Mounted from account/python
```

<span data-ttu-id="211d3-178">これでコンテナー イメージは Docker Hub に格納され、インターネット接続されている任意のコンピューターから `docker pull` または `docker run` を使用してアクセスできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="211d3-178">The container image is now stored in Docker Hub and can be accessed from any internet-connected machine using `docker pull` or `docker run`.</span></span>

## <a name="summary"></a><span data-ttu-id="211d3-179">まとめ</span><span class="sxs-lookup"><span data-stu-id="211d3-179">Summary</span></span>

<span data-ttu-id="211d3-180">このユニットでは 2 つのコンテナー イメージを作成しました。</span><span class="sxs-lookup"><span data-stu-id="211d3-180">In this unit, you created two container images.</span></span> <span data-ttu-id="211d3-181">最初のコンテナー イメージは手動で作成し、2 つ目のコンテナー イメージは Dockerfile を使用して自動的に作成しました。</span><span class="sxs-lookup"><span data-stu-id="211d3-181">The first was created manually and the second was automated using a Dockerfile.</span></span>
