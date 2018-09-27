<span data-ttu-id="b9cf0-101">これまでに、他のユーザーが作成した Docker イメージをダウンロードして実行しました。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-101">Up until now, you downloaded and ran Docker images created by others.</span></span> <span data-ttu-id="b9cf0-102">ここでは、自分のコンテナー イメージをビルドします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-102">Here, you'll build your own container images.</span></span> <span data-ttu-id="b9cf0-103">学習内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-103">You'll learn how to:</span></span>

* <span data-ttu-id="b9cf0-104">手動プロセスを使用してイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="b9cf0-104">Create an image using a manual process.</span></span>
* <span data-ttu-id="b9cf0-105">Dockerfile と呼ばれるさらに自動化されたプロセスを使って、同じイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="b9cf0-105">Create the same image using a more automated process, called a Dockerfile.</span></span>
* <span data-ttu-id="b9cf0-106">ご自分のイメージを Docker Hub (パブリック コンテナー レジストリ) に発行する</span><span class="sxs-lookup"><span data-stu-id="b9cf0-106">Publish your image to Docker Hub, a public container registry.</span></span>

## <a name="what-is-a-dockerfile"></a><span data-ttu-id="b9cf0-107">Dockerfile とは</span><span class="sxs-lookup"><span data-stu-id="b9cf0-107">What is a Dockerfile?</span></span>

<span data-ttu-id="b9cf0-108">Dockerfile は、コンテナーで実行するために必要なすべてを説明しているテキスト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-108">A Dockerfile is a text file that describes everything your container needs to run.</span></span>

<span data-ttu-id="b9cf0-109">Dockerfile 内の命令では次を定義できます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-109">An instruction in the Dockerfile can define:</span></span>

* <span data-ttu-id="b9cf0-110">ご自分のイメージが基準にする親イメージ</span><span class="sxs-lookup"><span data-stu-id="b9cf0-110">The parent image your image is based on.</span></span>
* <span data-ttu-id="b9cf0-111">インストールするソフトウェア パッケージ</span><span class="sxs-lookup"><span data-stu-id="b9cf0-111">A software package to install.</span></span>
* <span data-ttu-id="b9cf0-112">ご利用のアプリで実行する必要がある構成またはその他のファイル</span><span class="sxs-lookup"><span data-stu-id="b9cf0-112">A configuration or other file your app needs to run.</span></span>
* <span data-ttu-id="b9cf0-113">使用できるようにするポートなどのネットワーク設定</span><span class="sxs-lookup"><span data-stu-id="b9cf0-113">Networking settings, such as a port you want to make available.</span></span>
* <span data-ttu-id="b9cf0-114">コンテナーの起動時に実行するコマンド</span><span class="sxs-lookup"><span data-stu-id="b9cf0-114">The command to run when the container starts.</span></span>

<span data-ttu-id="b9cf0-115">手動プロセスを使用してコンテナー イメージを作成したり、Dockerfile を使用してプロセスを自動化したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-115">You can create a container image using a manual process or use a Dockerfile to automate the process.</span></span> <span data-ttu-id="b9cf0-116">Docker イメージを手動で作成することは、このプロセスを理解するのに適しています。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-116">Creating a Docker image manually is a good way to understand the process.</span></span> <span data-ttu-id="b9cf0-117">Dockerfile を使用すると、プロセスをすばやく簡単に繰り返すことができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-117">Using a Dockerfile makes the process faster and easier to repeat.</span></span>

## <a name="what-is-docker-hub"></a><span data-ttu-id="b9cf0-118">Docker Hub とは</span><span class="sxs-lookup"><span data-stu-id="b9cf0-118">What is Docker Hub?</span></span>

<span data-ttu-id="b9cf0-119">Docker Hub は、Docker イメージを格納してダウンロードする場所です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-119">Docker Hub is a place for you to store and download Docker images.</span></span> <span data-ttu-id="b9cf0-120">Docker および Docker コミュニティ (以前に使用した Nginx イメージなど) によって作成された Docker イメージをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-120">You can download Docker images that have been created by Docker and the Docker community, such as the Nginx image you used previously.</span></span> <span data-ttu-id="b9cf0-121">また、Docker コミュニティ、またはご自分のチームのみと自分のイメージを共有することもできます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-121">You can also share your own images with the Docker community or just with your team.</span></span>

## <a name="create-an-image-manually"></a><span data-ttu-id="b9cf0-122">手動でイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="b9cf0-122">Create an image manually</span></span>

<span data-ttu-id="b9cf0-123">ここでは、基本的な Python アプリケーションを実行する Docker イメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-123">Here you'll create a Docker image that runs a basic Python application.</span></span>

1. <span data-ttu-id="b9cf0-124">[python](https://hub.docker.com/_/python/?azure-portal=true) イメージからコンテナー インスタンスを呼び出すことから始めます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-124">Start by bringing up a container instance from the [python](https://hub.docker.com/_/python/?azure-portal=true) image.</span></span>

    ```bash
    docker run --name python-demo -it python bash
    ```

    <span data-ttu-id="b9cf0-125">Docker で Docker Hub からこのイメージをプル ダウンするには数分かかります。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-125">It'll take a few moments for Docker to pull down this image from Docker Hub.</span></span> <span data-ttu-id="b9cf0-126">待っている間に、このコマンドで行う内容を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-126">While you wait, let's look at what the command does.</span></span>

    * <span data-ttu-id="b9cf0-127">`--name` 引数では、ご利用のコンテナーの名前 "python-demo" を指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-127">The `--name` argument specifies your container's name, "python-demo".</span></span>
    * <span data-ttu-id="b9cf0-128">`-i` 引数と `-t` 引数は、1 つの引数 `-it` に結合されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-128">The `-i` and `-t` arguments are combined into one argument, `-it`.</span></span> <span data-ttu-id="b9cf0-129">これらの引数によって、実行中のコンテナーとの対話型セッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-129">These arguments create an interactive session with the running container.</span></span>
      * <span data-ttu-id="b9cf0-130">`-i` では、コンテナーとの対話型セッションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-130">`-i` creates an interactive session with the container.</span></span>
      * <span data-ttu-id="b9cf0-131">`-t` では、実行中の状態のまま擬似ターミナルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-131">`-t` creates a pseudo terminal that remains in a running state.</span></span>
    * <span data-ttu-id="b9cf0-132">`python` では、基本イメージを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-132">`python` specifies the base image.</span></span> <span data-ttu-id="b9cf0-133">Docker によって Docker Hub からこのイメージがダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-133">Docker downloads this image from Docker Hub.</span></span> <span data-ttu-id="b9cf0-134">このイメージは Python プログラミング用の環境で事前に設定されています。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-134">This image is preconfigued with an environment for Python programming.</span></span>
    * <span data-ttu-id="b9cf0-135">`bash` では、実行するプログラムを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-135">`bash` specifies the program to run.</span></span>

    <span data-ttu-id="b9cf0-136">このセットアップを、自動的に Python アプリが作成される対話型環境として考えます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-136">Think of this setup as an interactive environment for you to write Python apps.</span></span> <span data-ttu-id="b9cf0-137">ご利用のアプリを起動して実行すると、ご自分の環境のイメージ (スナップショット) をキャプチャできます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-137">Once you have your app up and running, you can capture an image, or snapshot, of your environment.</span></span> <span data-ttu-id="b9cf0-138">アプリを改善すると、お客様またはチーム用に追加のイメージを作成できます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-138">As you improve your app, you can create additional images for you or your team.</span></span>

    <span data-ttu-id="b9cf0-139">ターミナル セッションが、コンテナーの擬似ターミナルに切り替わります。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-139">Your terminal session switches to the container's pseudo terminal.</span></span> <span data-ttu-id="b9cf0-140">プロンプトは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-140">Your prompt resembles this:</span></span>

    ```docker
    root@d8ccada9c61e:/#
    ```

1. <span data-ttu-id="b9cf0-141">ご自分の Docker セッションからこれを実行し、`./app` という名前のディレクトリを作成してそこに移動します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-141">From your Docker session, run this to create a directory named `./app` and move to it.</span></span>

    ```bash
    mkdir ./app && cd ./app
    ```

    <span data-ttu-id="b9cf0-142">必須ではありませんが、`./app` ディレクトリでは Python コードを追加する場所が提供されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-142">Although not required, the `./app` directory gives you a place to add your Python code.</span></span>

1. <span data-ttu-id="b9cf0-143">これを実行し、非常に基本的な Python プログラムを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-143">Run this to create a very basic Python program.</span></span>

    ```bash
    echo 'print("Hello World!")' > hello.py
    ```

    <span data-ttu-id="b9cf0-144">このプログラムによって "Hello World!" が</span><span class="sxs-lookup"><span data-stu-id="b9cf0-144">This program prints "Hello World!"</span></span> <span data-ttu-id="b9cf0-145">ターミナルに出力されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-145">to the terminal.</span></span>

    > [!TIP]
    > <span data-ttu-id="b9cf0-146">実際には、ご利用のワークステーション上のディレクトリをご使用のコンテナー上のディレクトリに_マウント_することができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-146">In practice, you can _mount_ a directory on your workstation to a directory on your container.</span></span> <span data-ttu-id="b9cf0-147">これにより、ご利用のワークステーション上の好みのエディターを使用してコードを記述できます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-147">This enables you to use your favorite editor on your workstation to write code.</span></span> <span data-ttu-id="b9cf0-148">ファイルを保存すると、そのファイルもご使用のコンテナー上に表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-148">When you save your file, that file also appears on your container.</span></span>

1. <span data-ttu-id="b9cf0-149">次を実行して、ご自分の Python プログラムを試してみます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-149">Run the following to try out your Python program.</span></span>

    ```bash
    python hello.py
    ```

    <span data-ttu-id="b9cf0-150">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-150">You see this.</span></span>

    ```output
    Hello World!
    ```

1. <span data-ttu-id="b9cf0-151">ご利用の対話型セッションを終了します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-151">Exit your interactive session.</span></span>

    ```bash
    exit
    ```

1. <span data-ttu-id="b9cf0-152">ご使用の Linux VM から `docker ps` を実行し、実行中のコンテナーをすべて一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-152">From your Linux VM, run `docker ps` to list all running containers:</span></span>

    ```bash
    docker ps
    ```

    <span data-ttu-id="b9cf0-153">何も実行されていないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-153">You see that nothing is running.</span></span> 

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

    <span data-ttu-id="b9cf0-154">これは、ご自分の Bash セッションを終了するコンテナーを終了しているためです。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-154">That's because exiting the container ends your Bash session.</span></span> <span data-ttu-id="b9cf0-155">ご使用のコンテナーで実行しているアプリケーションまたはサービスを終了すると、Docker によってそのコンテナーが停止されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-155">When the application or service your container runs exits, Docker stops the container.</span></span>

1. <span data-ttu-id="b9cf0-156">`-a` 引数を含めて、2 回目の `docker ps` を実行します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-156">Run `docker ps` a second time and include the `-a` argument.</span></span> <span data-ttu-id="b9cf0-157">このコマンドでは、実行中または停止されたすべてのコンテナーが返されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-157">This command returns all containers, running or stopped.</span></span>

    ```bash
    docker ps -a
    ```

    <span data-ttu-id="b9cf0-158">次のような結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-158">You see something like this:</span></span>

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
    ```

   <span data-ttu-id="b9cf0-159">ご使用のコンテナー **python-demo** は、**Exited** の状態です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-159">Your container, **python-demo**, has a status of **Exited**.</span></span>

1. <span data-ttu-id="b9cf0-160">`docker commit` を実行して、ご使用のコンテナーからイメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-160">Run `docker commit` to create an image from your container.</span></span>

   ```bash
   docker commit python-demo python-custom
   ```

   <span data-ttu-id="b9cf0-161">ここでは、Docker によって **python-demo** という名前のご使用のコンテナーから、**python-custom** という名前のイメージが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-161">Here, Docker creates an image named **python-custom** from your container named **python-demo**.</span></span> <span data-ttu-id="b9cf0-162">イメージ名はどのような名前にもすることができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-162">The image name can be whatever you want.</span></span>

1. <span data-ttu-id="b9cf0-163">`docker images` を実行してご自分のイメージを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-163">Run `docker images` to list your images.</span></span>

    ```bash
    docker images
    ```

    <span data-ttu-id="b9cf0-164">**python-custom** イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-164">You see the **python-custom** image.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-custom       latest              1f231e7127a1        6 seconds ago       922MB
    python              latest              638817465c7d        24 hours ago        922MB
    ```

1. <span data-ttu-id="b9cf0-165">`docker run` を使用してご自分のイメージをテストします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-165">Use `docker run` to test out your image.</span></span>

    ```bash
    docker run python-custom python app/hello.py
    ```

    <span data-ttu-id="b9cf0-166">その最後の引数として実行するコマンドを取得する `docker run` を呼び戻します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-166">Recall that `docker run` takes the command to run as its final argument.</span></span> <span data-ttu-id="b9cf0-167">ここでのコマンドは `python app/hello.py` です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-167">Here, the command is `python app/hello.py`.</span></span>

    <span data-ttu-id="b9cf0-168">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-168">You see this.</span></span>

    ```output
    Hello World!
    ```

    <span data-ttu-id="b9cf0-169">このコンテナーは対話的に実行していないため、Python プログラムは実行され、コンテナーは停止されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-169">Because you're not running this container interactively, the Python program runs and the container stops.</span></span>

## <a name="use-a-dockerfile-to-create-an-image-automatically"></a><span data-ttu-id="b9cf0-170">Dockerfile を使用してイメージを自動的に作成する</span><span class="sxs-lookup"><span data-stu-id="b9cf0-170">Use a Dockerfile to create an image automatically</span></span>

<span data-ttu-id="b9cf0-171">ここでは、より自動化されたプロセスを使用して、ご自分の Python プログラムを実行する、同じ Docker イメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-171">Here you'll use a more automated process to create the same Docker image that runs your Python program.</span></span>

<span data-ttu-id="b9cf0-172">手動でイメージをビルドすることは、新機能を試して調べるのに最適な方法です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-172">Building an image manually is a great way to experiment and explore new features.</span></span> <span data-ttu-id="b9cf0-173">しかし、あなたはこのプロセスをチームと共有するか、繰り返し使えるようにしたいと仮定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-173">But say you want to share the process with your team or make it repeatable.</span></span> <span data-ttu-id="b9cf0-174">たとえば、チームがビルドしている最新バージョンのソフトウェアを実行する夜ごとに、新しいイメージを作成する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-174">For example, say you want to create a new image each evening that runs the latest version of the software your team is building.</span></span>

<span data-ttu-id="b9cf0-175">ここで Dockerfile の出番です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-175">That's where the Dockerfile comes in.</span></span> <span data-ttu-id="b9cf0-176">命令を記述する方法として Dockerfile を考えます。それ以外の場合は、手動で行います。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-176">Think of a Dockerfile as a way to describe instructions you would otherwise do manually.</span></span>

1. <span data-ttu-id="b9cf0-177">ご利用の Linux VM からこのコマンドを実行し、ご自分の Python プログラムを実行する Dockerfile を作成します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-177">From your Linux VM, run this command to create a Dockerfile that runs your Python program.</span></span>

    ```bash
    cat << EOF > Dockerfile
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    EOF
    ```

    <span data-ttu-id="b9cf0-178">この例では、ファイルを作成するのに簡単な方法というだけです。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-178">This example is just an easy way for you to create the file.</span></span> <span data-ttu-id="b9cf0-179">実際には、任意のコード エディターを使用して作成することになります。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-179">In practice, you would use your favorite code editor to create it.</span></span>

    <span data-ttu-id="b9cf0-180">Dockerfile をコンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-180">Print your Dockerfile to the console.</span></span>

    ```bash
    cat Dockerfile
    ```

    <span data-ttu-id="b9cf0-181">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-181">You see this.</span></span>

    ```output
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    ```

    <span data-ttu-id="b9cf0-182">Dockerfile によって行う内容を分解しましょう。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-182">Let's break down what the Dockerfile does.</span></span>

    * <span data-ttu-id="b9cf0-183">`FROM` では、このイメージの基にする親イメージを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-183">`FROM` specifies the parent image to base this image on.</span></span> <span data-ttu-id="b9cf0-184">ここでの親イメージは **python** です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-184">Here, the parent image is **python**.</span></span>
    * <span data-ttu-id="b9cf0-185">`WORKDIR` では、Dockerfile の後で表示される任意の `RUN` ステートメントと `CMD` ステートメント用の作業ディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-185">`WORKDIR` specifies the working directory for any `RUN` and `CMD` statements that appear later in the Dockerfile.</span></span> <span data-ttu-id="b9cf0-186">Docker によってこのディレクトリが自動的に作成されます。この場合は、`./app` です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-186">Docker creates this directory for you, in this case, `./app`.</span></span>
    * <span data-ttu-id="b9cf0-187">`RUN` では、コンテナー内で実行するコマンドを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-187">`RUN` specifies a command to run in the container.</span></span> <span data-ttu-id="b9cf0-188">`RUN` を使用すると、ソフトウェアをインストールし、構成を変更し、キャプチャされる前にコンテナーをクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-188">You can use `RUN` to install software, make configuration changes, and cleanup the container before it's captured.</span></span> <span data-ttu-id="b9cf0-189">ここでは、基本的な Python プログラムを `./app/hello.py` に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-189">Here, we write a basic Python program to `./app/hello.py`.</span></span>
    * <span data-ttu-id="b9cf0-190">`CMD` では、コンテナーの起動時に実行するプロセスを指定します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-190">`CMD` specifies the process to run when the container starts.</span></span> <span data-ttu-id="b9cf0-191">ここでは、`/.app` ディレクトリから `python hello.py` を実行します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-191">Here, we run `python hello.py` from the `/.app` directory.</span></span>

    <span data-ttu-id="b9cf0-192">これらは、イメージを手動でビルドするために行う手順とほぼまったく同じであることにお気付きでしょう。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-192">You've probably noticed that these are nearly the exact same steps you took to build the image manually.</span></span> <span data-ttu-id="b9cf0-193">コードとしてこれらの手順を表現すると、共有と繰り返しのプロセスを簡単にすることができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-193">Expressing these steps as code makes the process easier to share and repeat.</span></span>

1. <span data-ttu-id="b9cf0-194">Dockerfile 内で指定された命令を使用して、`docker build` を実行してイメージを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-194">Run `docker build` to create the image, using the instructions specified in the Dockerfile.</span></span>

    ```bash
    docker build -t python-dockerfile .
    ```

    <span data-ttu-id="b9cf0-195">通常、Docker によるイメージのビルドにかかるのは数秒だけです。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-195">It will typically take only a few seconds for Docker to build your image.</span></span>

    <span data-ttu-id="b9cf0-196">次のような出力結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-196">You see output like the following.</span></span>

    ```output
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

1. <span data-ttu-id="b9cf0-197">`docker images` を使用してご自分のイメージを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-197">Use the `docker images` to list your images.</span></span>

    ```bash
    docker images
    ```

    <span data-ttu-id="b9cf0-198">ビルドしたばかりの **python-dockerfile** イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-198">You see the **python-dockerfile** image you just built.</span></span>

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-dockerfile   latest              8ed5f85c5e5b        24 seconds ago      923MB
    python-custom       latest              6789cbe2cceb        4 minutes ago       923MB
    python              latest              a9d071760c82        2 weeks ago         923MB
    ```

1. <span data-ttu-id="b9cf0-199">`docker run` を使用してご使用のコンテナーをテストします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-199">Use `docker run` to test out your container.</span></span>

    ```bash
    docker run python-dockerfile
    ```

    <span data-ttu-id="b9cf0-200">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-200">You see this.</span></span>

    ```output
    Hello World!
    ```

    <span data-ttu-id="b9cf0-201">手動で作成したイメージをテストしたときとは異なり、ここでは実行するコマンドを指定しません (`python app/hello.py`)。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-201">Unlike when you tested the image you created manually, here you don't specify the command to run, `python app/hello.py`.</span></span> <span data-ttu-id="b9cf0-202">ご自分の Dockerfile では指定するため、このコマンドはイメージにビルドされます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-202">Your Dockerfile specifies that, so the command is built into the image.</span></span>

## <a name="publish-your-image-to-docker-hub"></a><span data-ttu-id="b9cf0-203">イメージを Docker Hub に発行する</span><span class="sxs-lookup"><span data-stu-id="b9cf0-203">Publish your image to Docker Hub</span></span>

<span data-ttu-id="b9cf0-204">イメージを Docker Hub に発行するには、アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-204">To publish an image to Docker Hub, you need an account.</span></span> <span data-ttu-id="b9cf0-205">ここでは、Docker Hub アカウントを設定して、ご自分の **python-dockerfile** イメージを Docker Hub に発行します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-205">Here you'll set up your Docker Hub account and publish your **python-dockerfile** image to Docker Hub.</span></span>

1. <span data-ttu-id="b9cf0-206">[ご自分の Docker Hub アカウントを作成します](https://hub.docker.com?azure-portal=true)。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-206">[Create your Docker Hub account](https://hub.docker.com?azure-portal=true).</span></span>

1. <span data-ttu-id="b9cf0-207">ご自分の Docker Hub アカウント名を環境変数としてエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-207">Export your Docker Hub account name as an environment variable.</span></span> <span data-ttu-id="b9cf0-208">**account-name** をご自分のアカウント名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-208">Replace **account-name** with your account name.</span></span>

    ```bash
    export docker_account=account-name
    ```

    <span data-ttu-id="b9cf0-209">この環境変数では、続くコマンドを簡単に実行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-209">This environment variable will make it easier for you to run the commands that follow.</span></span>

1. <span data-ttu-id="b9cf0-210">`docker tag` を実行して、ご自分のイメージに Docker Hub アカウント名でタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-210">Run `docker tag` to tag your image with your Docker Hub account name.</span></span>

    ```bash
    docker tag python-dockerfile $docker_account/python-dockerfile
    ```

1. <span data-ttu-id="b9cf0-211">`docker login` を実行して Docker Hub にログインします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-211">Run `docker login` to log in to Docker Hub.</span></span>

    ```bash
    docker login
    ```

    <span data-ttu-id="b9cf0-212">メッセージが表示されたら、ご自分のユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-212">When prompted, enter your username and password.</span></span>

1. <span data-ttu-id="b9cf0-213">`docker push` を実行して、ご自分の **python-dockerfile** イメージを Docker Hub にプッシュ (アップロード) します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-213">Run `docker push` to push, or upload, your **python-dockerfile** image to Docker Hub.</span></span>

    ```bash
    docker push $docker_account/python-dockerfile
    ```

    <span data-ttu-id="b9cf0-214">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-214">You see output similar to the following:</span></span>

    ```output
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
    latest: digest: sha256:b816c32382d06ac74530d56f2a7b83000c86174218f430c6bb5039fdcfba38aa size: 2631
    ```

    <span data-ttu-id="b9cf0-215">これで、ご自分の Docker イメージは、Docker Hub に格納されました。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-215">Your Docker image is now stored in Docker Hub.</span></span> <span data-ttu-id="b9cf0-216">別のコンピューターから `docker pull` または `docker run` を使用して、Docker Hub からご自分のイメージをダウンロードまたは実行することができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-216">You can use `docker pull` or `docker run` from another computer to download or run your image from Docker Hub.</span></span> <span data-ttu-id="b9cf0-217">次に 2 つの例を示します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-217">Here are two examples:</span></span>

    1. <span data-ttu-id="b9cf0-218">この例では、Docker Hub から最新のイメージをプルします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-218">This example pulls the latest image from Docker Hub.</span></span>

        ```bash
        docker pull my_docker_account/python-dockerfile
        ```

    1. <span data-ttu-id="b9cf0-219">この例では、コンテナーを実行します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-219">This example runs the container.</span></span>

        ```bash
        docker run my_docker_account/python-dockerfile
        ```

1. <span data-ttu-id="b9cf0-220">ご使用のコンテナーをテストします。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-220">Test out your container.</span></span>

    <span data-ttu-id="b9cf0-221">[ご自分の Docker Hub アカウント](https://hub.docker.com?azure-portal=true)に移動します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-221">Navigate to [your Docker Hub account](https://hub.docker.com?azure-portal=true).</span></span> <span data-ttu-id="b9cf0-222">**[リポジトリ]** タブから、ご自分の Docker イメージを確認します。</span><span class="sxs-lookup"><span data-stu-id="b9cf0-222">From the **Repositories** tab, you see your Docker image.</span></span>

    ![Docker Hub 上の Docker イメージ](../media/4-docker-hub-python-dockerfile.png)
