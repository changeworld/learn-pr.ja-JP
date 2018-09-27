これまでに、他のユーザーが作成した Docker イメージをダウンロードして実行しました。 ここでは、自分のコンテナー イメージをビルドします。 学習内容は次のとおりです。

* 手動プロセスを使用してイメージを作成する
* Dockerfile と呼ばれるさらに自動化されたプロセスを使って、同じイメージを作成する
* ご自分のイメージを Docker Hub (パブリック コンテナー レジストリ) に発行する

## <a name="what-is-a-dockerfile"></a>Dockerfile とは

Dockerfile は、コンテナーで実行するために必要なすべてを説明しているテキスト ファイルです。

Dockerfile 内の命令では次を定義できます。

* ご自分のイメージが基準にする親イメージ
* インストールするソフトウェア パッケージ
* ご利用のアプリで実行する必要がある構成またはその他のファイル
* 使用できるようにするポートなどのネットワーク設定
* コンテナーの起動時に実行するコマンド

手動プロセスを使用してコンテナー イメージを作成したり、Dockerfile を使用してプロセスを自動化したりすることができます。 Docker イメージを手動で作成することは、このプロセスを理解するのに適しています。 Dockerfile を使用すると、プロセスをすばやく簡単に繰り返すことができます。

## <a name="what-is-docker-hub"></a>Docker Hub とは

Docker Hub は、Docker イメージを格納してダウンロードする場所です。 Docker および Docker コミュニティ (以前に使用した Nginx イメージなど) によって作成された Docker イメージをダウンロードできます。 また、Docker コミュニティ、またはご自分のチームのみと自分のイメージを共有することもできます。

## <a name="create-an-image-manually"></a>手動でイメージを作成する

ここでは、基本的な Python アプリケーションを実行する Docker イメージを作成します。

1. [python](https://hub.docker.com/_/python/?azure-portal=true) イメージからコンテナー インスタンスを呼び出すことから始めます。

    ```bash
    docker run --name python-demo -it python bash
    ```

    Docker で Docker Hub からこのイメージをプル ダウンするには数分かかります。 待っている間に、このコマンドで行う内容を見てみましょう。

    * `--name` 引数では、ご利用のコンテナーの名前 "python-demo" を指定します。
    * `-i` 引数と `-t` 引数は、1 つの引数 `-it` に結合されます。 これらの引数によって、実行中のコンテナーとの対話型セッションが作成されます。
      * `-i` では、コンテナーとの対話型セッションが作成されます。
      * `-t` では、実行中の状態のまま擬似ターミナルが作成されます。
    * `python` では、基本イメージを指定します。 Docker によって Docker Hub からこのイメージがダウンロードされます。 このイメージは Python プログラミング用の環境で事前に設定されています。
    * `bash` では、実行するプログラムを指定します。

    このセットアップを、自動的に Python アプリが作成される対話型環境として考えます。 ご利用のアプリを起動して実行すると、ご自分の環境のイメージ (スナップショット) をキャプチャできます。 アプリを改善すると、お客様またはチーム用に追加のイメージを作成できます。

    ターミナル セッションが、コンテナーの擬似ターミナルに切り替わります。 プロンプトは次のようになります。

    ```docker
    root@d8ccada9c61e:/#
    ```

1. ご自分の Docker セッションからこれを実行し、`./app` という名前のディレクトリを作成してそこに移動します。

    ```bash
    mkdir ./app && cd ./app
    ```

    必須ではありませんが、`./app` ディレクトリでは Python コードを追加する場所が提供されます。

1. これを実行し、非常に基本的な Python プログラムを作成します。

    ```bash
    echo 'print("Hello World!")' > hello.py
    ```

    このプログラムによって "Hello World!" が ターミナルに出力されます。

    > [!TIP]
    > 実際には、ご利用のワークステーション上のディレクトリをご使用のコンテナー上のディレクトリに_マウント_することができます。 これにより、ご利用のワークステーション上の好みのエディターを使用してコードを記述できます。 ファイルを保存すると、そのファイルもご使用のコンテナー上に表示されます。

1. 次を実行して、ご自分の Python プログラムを試してみます。

    ```bash
    python hello.py
    ```

    次のように表示されます。

    ```output
    Hello World!
    ```

1. ご利用の対話型セッションを終了します。

    ```bash
    exit
    ```

1. ご使用の Linux VM から `docker ps` を実行し、実行中のコンテナーをすべて一覧表示します。

    ```bash
    docker ps
    ```

    何も実行されていないことがわかります。 

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

    これは、ご自分の Bash セッションを終了するコンテナーを終了しているためです。 ご使用のコンテナーで実行しているアプリケーションまたはサービスを終了すると、Docker によってそのコンテナーが停止されます。

1. `-a` 引数を含めて、2 回目の `docker ps` を実行します。 このコマンドでは、実行中または停止されたすべてのコンテナーが返されます。

    ```bash
    docker ps -a
    ```

    次のような結果が表示されます。

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
    ```

   ご使用のコンテナー **python-demo** は、**Exited** の状態です。

1. `docker commit` を実行して、ご使用のコンテナーからイメージを作成します。

   ```bash
   docker commit python-demo python-custom
   ```

   ここでは、Docker によって **python-demo** という名前のご使用のコンテナーから、**python-custom** という名前のイメージが作成されます。 イメージ名はどのような名前にもすることができます。

1. `docker images` を実行してご自分のイメージを一覧表示します。

    ```bash
    docker images
    ```

    **python-custom** イメージが表示されます。

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-custom       latest              1f231e7127a1        6 seconds ago       922MB
    python              latest              638817465c7d        24 hours ago        922MB
    ```

1. `docker run` を使用してご自分のイメージをテストします。

    ```bash
    docker run python-custom python app/hello.py
    ```

    その最後の引数として実行するコマンドを取得する `docker run` を呼び戻します。 ここでのコマンドは `python app/hello.py` です。

    次のように表示されます。

    ```output
    Hello World!
    ```

    このコンテナーは対話的に実行していないため、Python プログラムは実行され、コンテナーは停止されます。

## <a name="use-a-dockerfile-to-create-an-image-automatically"></a>Dockerfile を使用してイメージを自動的に作成する

ここでは、より自動化されたプロセスを使用して、ご自分の Python プログラムを実行する、同じ Docker イメージを作成します。

手動でイメージをビルドすることは、新機能を試して調べるのに最適な方法です。 しかし、あなたはこのプロセスをチームと共有するか、繰り返し使えるようにしたいと仮定します。 たとえば、チームがビルドしている最新バージョンのソフトウェアを実行する夜ごとに、新しいイメージを作成する必要があるとします。

ここで Dockerfile の出番です。 命令を記述する方法として Dockerfile を考えます。それ以外の場合は、手動で行います。

1. ご利用の Linux VM からこのコマンドを実行し、ご自分の Python プログラムを実行する Dockerfile を作成します。

    ```bash
    cat << EOF > Dockerfile
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    EOF
    ```

    この例では、ファイルを作成するのに簡単な方法というだけです。 実際には、任意のコード エディターを使用して作成することになります。

    Dockerfile をコンソールに出力します。

    ```bash
    cat Dockerfile
    ```

    次のように表示されます。

    ```output
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    ```

    Dockerfile によって行う内容を分解しましょう。

    * `FROM` では、このイメージの基にする親イメージを指定します。 ここでの親イメージは **python** です。
    * `WORKDIR` では、Dockerfile の後で表示される任意の `RUN` ステートメントと `CMD` ステートメント用の作業ディレクトリを指定します。 Docker によってこのディレクトリが自動的に作成されます。この場合は、`./app` です。
    * `RUN` では、コンテナー内で実行するコマンドを指定します。 `RUN` を使用すると、ソフトウェアをインストールし、構成を変更し、キャプチャされる前にコンテナーをクリーンアップすることができます。 ここでは、基本的な Python プログラムを `./app/hello.py` に書き込みます。
    * `CMD` では、コンテナーの起動時に実行するプロセスを指定します。 ここでは、`/.app` ディレクトリから `python hello.py` を実行します。

    これらは、イメージを手動でビルドするために行う手順とほぼまったく同じであることにお気付きでしょう。 コードとしてこれらの手順を表現すると、共有と繰り返しのプロセスを簡単にすることができます。

1. Dockerfile 内で指定された命令を使用して、`docker build` を実行してイメージを作成します。

    ```bash
    docker build -t python-dockerfile .
    ```

    通常、Docker によるイメージのビルドにかかるのは数秒だけです。

    次のような出力結果が表示されます。

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

1. `docker images` を使用してご自分のイメージを一覧表示します。

    ```bash
    docker images
    ```

    ビルドしたばかりの **python-dockerfile** イメージが表示されます。

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-dockerfile   latest              8ed5f85c5e5b        24 seconds ago      923MB
    python-custom       latest              6789cbe2cceb        4 minutes ago       923MB
    python              latest              a9d071760c82        2 weeks ago         923MB
    ```

1. `docker run` を使用してご使用のコンテナーをテストします。

    ```bash
    docker run python-dockerfile
    ```

    次のように表示されます。

    ```output
    Hello World!
    ```

    手動で作成したイメージをテストしたときとは異なり、ここでは実行するコマンドを指定しません (`python app/hello.py`)。 ご自分の Dockerfile では指定するため、このコマンドはイメージにビルドされます。

## <a name="publish-your-image-to-docker-hub"></a>イメージを Docker Hub に発行する

イメージを Docker Hub に発行するには、アカウントが必要です。 ここでは、Docker Hub アカウントを設定して、ご自分の **python-dockerfile** イメージを Docker Hub に発行します。

1. [ご自分の Docker Hub アカウントを作成します](https://hub.docker.com?azure-portal=true)。

1. ご自分の Docker Hub アカウント名を環境変数としてエクスポートします。 **account-name** をご自分のアカウント名に置き換えます。

    ```bash
    export docker_account=account-name
    ```

    この環境変数では、続くコマンドを簡単に実行できるようになります。

1. `docker tag` を実行して、ご自分のイメージに Docker Hub アカウント名でタグ付けします。

    ```bash
    docker tag python-dockerfile $docker_account/python-dockerfile
    ```

1. `docker login` を実行して Docker Hub にログインします。

    ```bash
    docker login
    ```

    メッセージが表示されたら、ご自分のユーザー名とパスワードを入力します。

1. `docker push` を実行して、ご自分の **python-dockerfile** イメージを Docker Hub にプッシュ (アップロード) します。

    ```bash
    docker push $docker_account/python-dockerfile
    ```

    次のような出力が表示されます。

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

    これで、ご自分の Docker イメージは、Docker Hub に格納されました。 別のコンピューターから `docker pull` または `docker run` を使用して、Docker Hub からご自分のイメージをダウンロードまたは実行することができます。 次に 2 つの例を示します。

    1. この例では、Docker Hub から最新のイメージをプルします。

        ```bash
        docker pull my_docker_account/python-dockerfile
        ```

    1. この例では、コンテナーを実行します。

        ```bash
        docker run my_docker_account/python-dockerfile
        ```

1. ご使用のコンテナーをテストします。

    [ご自分の Docker Hub アカウント](https://hub.docker.com?azure-portal=true)に移動します。 **[リポジトリ]** タブから、ご自分の Docker イメージを確認します。

    ![Docker Hub 上の Docker イメージ](../media/4-docker-hub-python-dockerfile.png)
