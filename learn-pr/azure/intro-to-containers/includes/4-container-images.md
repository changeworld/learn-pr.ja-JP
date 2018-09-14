ここでは作成してカスタム コンテナー イメージの場合は、これらのイメージをパブリック コンテナー レジストリにプッシュし、これらのイメージからコンテナーを実行します。

コンテナー イメージを手動で作成するか、プロセスを自動化する Dockerfile を使用しています。 推奨される方法は、Dockerfile を使用して、この単位は両方の方法を示します。 手動プロセスを実践する Dockerfile をオートメーションを使用する場合、何が起きてをより深く理解する際に役立ちます。

## <a name="manual-image-creation"></a>手動でのイメージの作成

コンテナー イメージを手動で作成する場合は、次の操作を行います。

- コンテナーのインスタンスを起動します。
- コンテナーとのターミナル セッションを確立します。
- ソフトウェアのインストールおよび構成の変更を行うことにより、コンテナーを変更します。
- `docker capture` コマンドを使用して、コンテナーを新しいイメージに取り込みます。

この最初の例では、"Hello World"アプリケーションを作成や、新しいイメージにコンテナーをキャプチャし、Python を実行しているコンテナーのインスタンスを起動します。

まず、NGINX イメージからコンテナーを実行します。 このコマンドは、前のユニット実行したコマンドとは少し異なります。 実行中のコンテナーとのターミナル セッションを確立するので、`-t` および `-i` 引数を指定します。 同時に、これらの引数を割り当てる擬似端末で実行中の状態のままとなります Docker に指示します。 つまり、`-t` および `-i` 引数によって、実行中のコンテナーとの対話型セッションが作成されます。

次に、`python` コンテナー イメージが使用されると共にコンテナー内で実行されるプロセスが `bash` となるように指定します。

```bash
docker run --name python-demo -ti python bash
```

コマンドの実行後、ターミナル セッションはコンテナーの擬似ターミナルに切り替えられるはずです。 これは、次のように変更する必要がありますが、ターミナルのプロンプトで確認できます。

```output
root@d8ccada9c61e:/#
```

この時点では、コンテナー内で作業しています。 ことができますを一覧表示、作成、およびファイルを削除、ソフトウェアをインストールおよび構成を変更します。 この簡単な例では、Python ベース ' Hello World' のスクリプトが作成されます。 これを行うには、次のコマンドを使用します。

```bash
echo 'print("Hello World!")' > hello.py
```

コンテナー内にとどまったままスクリプトをテストするには、次のコマンドを実行します。

```bash
python hello.py
```

これにより、次の出力が作成されます。

```output
Hello World!
```

完成したら、スクリプトが期待どおりに機能する、」と入力して、コンテナーを終了`exit`:

```bash
exit
```

ご自分のローカル システムのターミナルに戻り、`docker ps` コマンドを使用して実行中のコンテナーをすべてリストします。

```bash
docker ps
```

何も実行されていないことに注目してください。 実行中のコンテナー内で `exit` を入力すると、Bash プロセスが完了し、これによりコンテナーが停止されました。 これは、想定される動作です。

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

`docker ps` を再度使用して、`-a` 引数を含めます。 このコマンドを実行すると、コンテナーが実行中であるかどうかに関係なく、すべてのコンテナーのリストが返されます。

```bash
docker ps -a
```

*python-demo* という名前を持つコンテナーの状態が *Exited* であることに注目してください。 このコンテナーは、終了するコンテナーの停止のインスタンスです。

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

このコンテナーから新しいコンテナー イメージを作成するには、`docker commit` コマンドを使用します。 次の例では、*python-demo* コンテナーから *python-custom* という名前のイメージを作成するように *docker commit* が指定されています。

```bash
docker commit python-demo python-custom
```

コマンドが完了すると、次のような出力が表示されます。

```output
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

ここで、`docker images` を実行して、コンテナー イメージのリストを表示します。

```bash
docker images
```

これで Python のカスタム イメージが表示されます。

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

新しいイメージからコンテナーを実行します。 また、コンテナー内で実行するコマンドまたはプロセスを指定する必要があります。 この例では、`python hello.py` を実行します。


```bash
docker run python-custom python hello.py
```

コンテナーは起動し、"Hello World"メッセージを出力します。 次に Python プロセスが完了し、コンテナーが停止します。

```output
Hello World!
```

## <a name="automated-image-creation"></a>イメージ作成の自動化

最後のセクションでは、コンテナー イメージを手動で作成します。 このメソッドは、1 回限りまたは試験用のイメージの作成機能、運用環境で維持可能なことはできません。 コンテナー イメージの作成は、*Dockerfile* と `docker build` コマンドを使用して自動化することができます。 *Docker ビルド*コマンド コンテナーを起動し、実行手順については、ある、 *Dockerfile*、結果を新しいコンテナー イメージをキャプチャします。

*Dockerfile* という名前のファイルを作成し、次のテキストを入力します。

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

*FROM* ステートメントでは、イメージの作成中に使用される基本イメージが指定されます。 *RUN* ステートメントでは、コンテナー内でコマンドが実行されます。 *RUN* を使用すると、ソフトウェアをインストールし、構成を変更し、キャプチャ イベントの前にコンテナーをクリーンアップすることができます。 *CMD* ステートメントでは、コンテナーの起動時に実行する必要があるプロセスを指定します。

`docker build` コマンドを使用すると、Dockerfile 内で指定された命令を使用して新しいコンテナー イメージを作成することができます。

```bash
docker build -t python-dockerfile .
```

次のような出力が表示されます。

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

`docker images` コマンドを使用すると、コンテナー イメージのリストが返されます。

```bash
docker images
```

これで、カスタム イメージが表示されます。

```output
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

`docker run` コマンドを使用すると、カスタム イメージからコンテナーを実行することができます。

ここでは、`docker run` コマンドに引数が指定されていないことに注目してください。 コンテナー イメージを手動で作成する場合とは異なり、Dockerfile を使用すると、コンテナーの起動時に実行するコマンドを含めることができます。 この場合、指定したコマンドは `python hello.py` です。これにより、Python スクリプトがコンテナーによって実行されます。

```bash
docker run python-dockerfile
```

コマンドを実行した後に、コンテナーの出力が表示されます。

```output
Hello World!
```

## <a name="push-the-image-to-a-public-registry"></a>パブリック レジストリにイメージをプッシュする

Docker Hub はコンテナーのパブリック レジストリです。 コンテナー イメージを Docker Hub にプッシュするには、Docker アカウントを用意する必要があります。 必要に応じて、[Docker Hub サイト](https://hub.docker.com/) にアクセスしてアカウントを登録します。

Docker Hub アカウントを用意したら、アカウント名でコンテナー イメージをタグ付けする必要があります。 そのためには、`docker tag` コマンドを使用します。

次の例では、*python-dockerfile* イメージが Docker Hub アカウント名でタグ付けされています。 `<account name>` を Docker Hub アカウント名に置換します。

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

次に、`docker login` コマンドを使用して Docker Hub にログインしていることを確認します。

```bash
docker login
```

最後に、`docker push` コマンドを使用して *python-dockerfile* イメージを Docker Hub にプッシュします。 `<account name>` を Docker Hub アカウント名に置換します。

```bash
docker push <account name>/python-dockerfile
```

コンテナー イメージは、Docker Hub にアップロードされている、中には、次のような出力が表示されます。

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
```

コンテナー イメージが Docker Hub に格納されていますとを使用して、インターネット接続されているコンピューターからアクセスできる`docker pull`または`docker run`します。

2 つのコンテナー イメージが作成されました。 最初のコンテナー イメージは手動で作成し、2 つ目のコンテナー イメージは Dockerfile を使用して自動的に作成しました。