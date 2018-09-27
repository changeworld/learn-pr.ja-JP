あなたは、勤務する会社でソフトウェア開発者としてのスキルを伸ばす機会を得て、社内の AI チームに所属することになりました。 やりがいのあるこの新しい役割に取り組みながら、日常業務もこなさなければなりません。 チーム内の先輩 AI エンジニアが、PyTorch ベースのラボを使って画像分類モデルをトレーニングする、役に立つ Jupyter ノートブックをいくつか紹介してくれました。 面白そうですが、あなたはフレームワークのセットを自分の dev rig にインストールしたくないと考えています。 その代わりに、Data Science Virtual Machine (DSVM) イメージをベースにした仮想マシンを作成することにしました。 

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-the-azure-cli"></a>Azure CLI とは

Azure CLI は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。 このツールは macOS、Linux、および Windows で利用でき、ブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使って利用することもできます。 このツールの使い方については、**CLI を使用した Azure サービスの制御**に関するモジュールで詳しく学習します。

## <a name="managing-deployments"></a>デプロイの管理

Azure CLI には、Azure Resource Manager のデプロイを管理するための `az group deployment` コマンドがあります。 特定のタスクを実行するためのサブコマンドがいくつかあります。 最も一般的なものを次に示します。

| サブコマンド | 説明 |
|-------------|-------------|
| `create` | デプロイを開始します。 |
| `list` | リソース グループに対して行われたすべてのデプロイを取得します。 |
| `export` | デプロイに使用されたテンプレートをエクスポートします。 |

使用可能なすべてのデプロイ コマンドの一覧を確認するには、[az group deployment のコマンド リファレンス](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)を参照してください。

ここでは、`az group deployment create` を使用して仮想マシンをプロビジョニングします。

## <a name="create-a-json-deployment-parameters-file"></a>JSON デプロイ パラメーター ファイルを作成する

ここでは、Azure Resource Manager テンプレートを使用して VM を作成します。 テンプレートには、プロビジョニングしようとしている Linux DSVM イメージが定義されています。 使用する VM サイズ、管理者ユーザー名とパスワード、マシンのホスト名など、いくつかのパラメーターをテンプレートに指定する必要があります。 

1. このユニットの右にある Azure Cloud Shell で次のコマンドを実行します。

    ```bash
    code parameter_file.json
    ```
    <!-- TODO add a link to official doc that explains the built-in editor when it becomes available -->このコマンドにより、`parameter_file.json` という空のファイルが組み込みエディターで開きます。 

1. 次の JSON スニペットをコード エディターの空のファイルに貼り付けます。

    ```json
    { 
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "adminUsername": { "value" : "<USERNAME>"},
         "adminPassword": { "value" : "<PASSWORD>"},
         "vmName": { "value" : "<HOSTNAME>"},
         "vmSize": { "value" : "Standard_DS2_v2"},
      }
    }
    ```

1. エディター上で、貼り付けた JSON に含まれる次のパラメーターを更新します。

    |パラメーター  |現在の値  |指定する値  |
    |---------|---------|---------|
    |adminUsername     |  `<USERNAME>`       |    この新しいマシンの管理者ユーザーの名前 (*azuser* など) を選択します。     |
    |adminPassword     |  `<PASSWORD>`       |   この管理者ユーザー アカウントのパスワードを選択します。 パスワードの要件の詳細については、「[Linux 仮想マシンについてのよく寄せられる質問](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)」を参照してください。     |
    |vmName     |   `<HOSTNAME>`      |  新しい仮想マシンの名前を選択します。 名前は文字で始め、小文字と数字のみで作成する必要があります。 ご自分のイニシャルと生まれた年を含めた名前など、一意の名前を選択するようにしてください。 |
    |vmSize     |  Standard_DS2_v2       |  この VM サイズは、この演習用としては問題ありませんが、自由に変更できます。 使用可能な VM サイズの一覧は、「[Azure の Linux 仮想マシンのサイズ](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)」で確認できます。       |

1. 変更を `parameter_file.json` に保存し、テキスト エディターを閉じます。

    > [!IMPORTANT]
    > adminUsername、adminPassword、vmName 用に選択した値を覚えておいてください。 この演習でもう一度使用します。

## <a name="create-a-resource-group"></a>リソース グループを作成する 

> [!IMPORTANT]
> 通常は、選択したリージョン内にリソース グループを作成します。 しかし、現在作業しているサンドボックス セッションでは、用意されているリソース グループを使用できます。 このセッションで使用するリソース グループは、**<rgn>[サンドボックス リソース グループ名]</rgn>** です。

## <a name="deploy-the-dsvm-to-your-resource-group"></a>リソース グループに DSVM をデプロイする

ここまで、リソース グループを作成し、`parameter_file.json` というファイルに DSVM Resource Manager テンプレートのパラメーターを定義しました。 次に、`az group deployment create` を実行して仮想マシンをプロビジョニングします。

1. Azure Cloud Shell で次のコマンドを実行します。

    ```azurecli
    az group deployment create \
    --resource-group  <rgn>[sandbox resource group name]</rgn> \
    --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json \
    --parameters parameter_file.json
    ```

    このコマンドにより、Resource Manager テンプレートと指定したパラメーターを使用して、リソース グループ内に仮想マシンを作成します。 

2. 仮想マシンのデプロイが完了するまでに数分かかる場合があります。 コンソールに ` - Running ..` と表示され、操作が完了するまでそれ以外はあまり表示されません。 操作が終了すると、JSON の応答が画面に出力されます。 JSON の一番下までスクロールし、**"provisioningState"** フィールドに "*Succeeded*" という値が表示されていることを確認します。

    > [!TIP]
    > DNS レコードが別のパブリック IP によって既に使用されているというエラーが表示された場合は、`parameter_file.json` 内の **vmName** を一意な別の名前に変更してみてください。

3. 次のコマンドを実行して VM についての情報を取得します。`<HOSTNAME>` は、ご利用の VM 用に定義したホスト名に置き換えてください。

    ```azurecli
    az vm get-instance-view \
    --name <HOSTNAME> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query instanceView.statuses[1] \
    --output table
    ```

    このコマンドにより、VM の状態が表示されます。 *VM running* と表示されるはずです。

お疲れさまでした。 DSVM イメージをベースにした Linux VM の作成とデプロイが完了しました。

## <a name="open-the-vm-to-ssh-traffic-on-port-22"></a>VM をポート 22 で ssh トラフィックに対して開く

既定では、この VM はどのポートも開かれていません。 ここでは、リモート接続を行い、Jupyter Notebook サーバーを起動して、マシン上で他のローカル コマンドを実行することが目的です。 Secure Shell (SSH) プロトコルを使用して VM にリモート接続するには、ポートを開く必要があります。 ポート 22 が ssh 用の既定のポートです。  

1. Azure Cloud Shell で次のコマンドを実行します。`<HOSTNAME>` は、セットアップ時に指定した DSV 仮想マシンの名前に置き換えてください。 

    ```azurecli
    az vm open-port \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 22 \
    --priority 900
    ```

このコマンドは、完了するまで 1 分ほどかかることがあります。 コマンドが終了すると、JSON の応答がコマンド ラインに返されます。 **"provisioningState"** フィールドの値が *Succeeded* になっていることを確認します。 この後すぐに、ssh が動作することをテストしますが、まず 1 つ以上のポートを開きましょう。

## <a name="open-the-vm-to-access-the-jupyter-notebook-server-remotely"></a>Jupyter Notebook サーバーにリモートでアクセスするように VM を開く

前述のように、DSVM イメージにはソフトウェア、ツール、サンプルがあらかじめインストールされていて、データ サイエンス プロジェクト、機械学習プロジェクト、ディープ ラーニング プロジェクトに役立てることができます。 Jupyter は、サンプル ノートブックと共にイメージにインストールされています。 VM 上で Jupyter Notebook サーバーを起動し、その後、ローカル マシンから Notebook サーバーにリモート接続します。 既定では、Notebook サーバーはポート 8888 上で稼働します。 サーバーにリモート アクセスするには、VM 上のそのポートを開く必要があります。 

1. Azure Cloud Shell で次のコマンドを実行します。`<HOSTNAME>` は、セットアップ時に指定したご自分の DSVM 仮想マシンの名前に置き換えてください。

    ```azurecli
    az vm open-port \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 8888 \
    --priority 901
    ```

ここでも、このコマンドは、完了するまで 1 分ほどかかることがあります。 コマンドが終了すると、JSON の応答がコマンド ラインに返されます。 **"provisioningState"** フィールドの値が *Succeeded* になっていることを確認します。  

## <a name="connect-to-the-vm-with-secure-shell-ssh"></a>Secure Shell (ssh) を使って VM に接続する

1. Azure Cloud Shell で次のコマンドを実行して、VM のパブリック IP アドレスを確認します。 `<HOSTNAME>` は、セットアップ時に指定したご自分の DSVM 仮想マシンの名前に置き換えてください。

    ```azurecli
    az vm list-ip-addresses \
    -g <rgn>[sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --output table
    ```

1. Cloud Shell で次のコマンドを実行して VM にサインインします。 `<USERNAME>` は、この演習の始めに選択したユーザー名に置き換えてください。 `<IP>` は、前のステップで確認した **PublicIPAddresses** 列の値に置き換えてください。

    たとえば、選択したユーザー名が *azuser* で、PublicIPAddresses の値が 33.165.103.23 であった場合、このコマンドは次のようになります。
    
    `ssh azuser@33.165.103.23`
    
    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 

1. プロンプトが表示されたら、この演習の始めに選択した管理者ユーザーのパスワードを入力します。 正常にサインインすると、表示されるプロンプトは `username@hostname` (たとえば `azuser@js1982`) に変わります。

次のステップでは、VM 上で Jupyter Notebook サーバーを起動し、リモートでノートブックを開きます。

## <a name="start-the-jupyter-notebook-server-on-the-vm"></a>VM 上で Jupyter Notebook サーバーを起動する

この VM の `~/notebooks` フォルダーに複数のノートブックがあります。 SSH セッション経由でまだログインしているという前提で、Notebook サーバーを起動し、それらのノートブックのうちのどれかを開いて、すべて正常に動作していることを確認します。


1. この VM のコマンド プロンプトで次のコマンドを実行します。

    ```bash
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
    ```

> [!CAUTION]
> この演習では、Notebook サーバーへのアクセスを `http://` 経由で行います。 公の場所で Notebook サーバーを実行する場合は、セキュリティで保護する必要があります。 Notebook サーバーをセキュリティで保護する方法の詳細については、Jupyter の公式オンライン ドキュメントを参照してください。 

前述のコマンドでは、`jupyter notebook` コマンドを使用して Jupyter Notebook サーバーを起動しました。 3 つの重要なコマンド ライン引数を指定します。 このマシンにコンソールからリモートにログインしていることに注意してください。 ノートブックはブラウザーに表示されます。 

 - `--ip=0.0.0.0` 既定では、Notebook サーバーは 127.0.0.1:8888 でローカルに稼働し、localhost からのみアクセス可能です。 http://127.0.0.1:8888 を使用してブラウザーから Notebook サーバーにローカルにアクセスできます。 IP アドレスを 0.0.0.0 に設定することで、サーバーはすべての IP を対象にトラフィックをリッスンします。 Notebook サーバーが 0.0.0.0 でリッスンしている場合は、ホスト マシンの IP アドレスを介してアクセスできます。  
 - `--no-browser`  別のコンピューターからインターネット経由でノートブックに接続したいので、ブラウザーを開かないように Notebook サーバーを構成します。ブラウザーを開くのは既定の動作です。 
 - `--allow-root`  この演習では、VM 上の管理者アカウントしかないので、root としてノートブックを実行できるようにする必要があります。

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a>リモート ブラウザーから Jupyter Notebook サーバーに接続する

上記のコマンドが VM 上で実行されると、Notebook サーバーが起動し、コンソールに URL が表示されます。その URL をブラウザーで使用できます。 

![コンソールに表示された、実行中の Notebook サーバーのメッセージのスクリーンショット。ホスト マシンからサーバーにアクセスするための URL が示されています](../media/notebook-url.png)

1. Notebook サーバーから返された URL を好きなブラウザーのアドレス バーにコピーします。 URL をクリックして既定のブラウザーで開くこともできます。 

    "サイトにアクセスできません" という旨のメッセージが表示されます。返された URL はホスト マシンから Notebook サーバーへの接続 URL であるためです。

1. サーバーにリモートでアクセスするには、URL のホスト名を、以前のステップで保存した VM の IP アドレスに置き換えます。 

    サンプルの Notebook サーバーの URL を次に示します。

    "http://**ab-dsvm-4**:8888/?token={some token}"

    この場合、**ab-dsvm-4** をマシンの IP アドレスに置き換えます。 IP アドレスが `52.175.199.43` であれば、URL は次のようになります。

    "http://**52.175.199.43**:8888/?token={some token}"

    ポート アドレス `:8888` が URL に使用されていることを確認してください。

    > [!TIP]
    > IP アドレスを使用したくない場合は、サーバーの完全修飾名を使用することもできます。形式は、`<HOST NAME>.<REGION>.cloudapp.azure.com` です。

    次のスクリーンショットは、Jupyter ダッシュボードがブラウザーにどのように表示されるかを示しています。

    ![Jupyter Notebook のダッシュボードのスクリーンショット。 ](../media/jupyter-in-browser.png)

1. **notebooks/IntroToJupyterPython.ipynb** に移動し、それを選択します。 このノートブックを使ってみて、すべてが予想どおりに動作することを確認してください。

    お疲れさまでした。 DSVM ベースの稼働する仮想マシンが実行されており、Jupyter をリモートで使用できるようになりました。 この演習では、VM 上にインストールされたソフトウェアを実行しています。 次の演習では、安心して実験できるようにソフトウェアを VM 上のコンテナーに分離します。

4. ノートブックを使用し終わったら、Cloud Shell で `Control-C` を使用して Jupyter サーバーを停止できます。
