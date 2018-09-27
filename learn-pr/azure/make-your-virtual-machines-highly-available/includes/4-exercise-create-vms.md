受信インターネット トラフィックをルーティングするためのロード バランサーの準備ができました。ここで、トラフィックをルーティングするための Azure VM がいくつか必要になります。 この例では Linux を使用しますが、どの仮想マシンのイメージでもまったく同じ手法を適用します。

## <a name="create-a-set-of-vms"></a>一連の VM を作成する

3 つの VM を作成し、Azure CLI を使用してそれらを可用性セットにグループ化します。 このタスクではポータルを使用できましたが、ここでは複数のリソースを作成するため、スクリプト ツールを使用して実行する方がはるかに簡単です。

_本当に_ Azure portal を使用したい場合の代替方法は、_テンプレートを_利用することです。 これらは JSON の手順であり、リソースをデプロイするために使用できます。 仮想マシン用のテンプレートを定義し、Azure portal で複数回テンプレートをデプロイできます。

### <a name="create-some-azure-cli-defaults"></a>Azure CLI の既定値をいくつか作成する

1. まず、Azure CLI で既定値をいくつか設定します。 使用するすべてのコマンドには、_リソース グループ_と省略可能な_場所_が必要です。 毎回入力するのではなく、既定のパラメーターとして設定できます。 事前に作成した Azure のサンドボックス リソース グループと、Azure Load Balancer に対して選択した同じ場所を使用します。 繰り返しますが、利用可能な場所は次のとおりです。

    [!include[](../../../includes/azure-sandbox-regions-note.md)]

1. 右側にある Cloud Shell にこのコマンドを入力し、`<location>` プレースホルダーを、Azure Load Balancer で使用した場所に置き換えます。

    ```azurecli
    az configure --defaults group=<rgn>[sandbox Resource Group</rgn> location=<location>
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

### <a name="create-an-availability-set"></a>可用性セットを作成する

最初に、_可用性セット_を作成します。

仮想マシンをグループ化するための可用性セットが必要になります。 これは `vm availability-set` コマンドを使用して作成できます。 必要なのは名前だけです。ここでは **woodgrove-avail-set** を使用します。

```azurecli
az vm availability-set create --name woodgrove-avail-set
```

### <a name="create-a-configuration-script"></a>構成スクリプトを作成する

VM にはそれぞれ同じ基本構成を使用します。

    - Ubuntu Linux
    - Nginx Web サーバー
    - Node.js
    - テスト用の単一ページを含む基本的な Web サイト

OS は選択するイメージに基づいて選びますが、他の構成要素ではスクリプトがいくつか必要になります。 Cloud Shell でローカル ファイルを作成して、Web サーバーをインストールし、基本的なサイトで構成してみましょう。

1. ターミナルで「`code`」と入力して、Cloud Shell エディターを開きます。

    ```bash
    code
    ```

    これは組み込みエディターです。これを使用して、Cloud Shell 環境ですぐにファイルを作成して編集することができます。 ファイルは、Cloud Shell 環境を起動したときに作成される、Azure の Storage アカウントに保存されます。

1. 次のスクリプトをコピーし、エディター ウィンドウに貼り付けます。

    ```script
    #cloud-config
    package_upgrade: true
    packages:
      - nginx
      - nodejs
      - npm
    write_files:
      - owner: www-data:www-data
      - path: /etc/nginx/sites-available/default
        content: |
          server {
            listen 80;
            location / {
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection keep-alive;
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
          }
      - owner: azureuser:azureuser
      - path: /home/azureuser/myapp/index.js
        content: |
          var express = require('express')
          var app = express()
          var os = require('os');
          app.get('/', function (req, res) {
            res.send('Hello World from host ' + os.hostname() + '!')
          })
          app.listen(3000, function () {
            console.log('Hello world app listening on port 3000!')
          })
    runcmd:
      - service nginx restart
      - cd "/home/azureuser/myapp"
      - npm init
      - npm install express -y
      - nodejs index.js
    ```

1. ファイルを保存します。ファイルには **cloud-init.txt** という名前を付けます。 エディターの右上隅にある [...] コンテキスト メニューを使用できます。また、Windows と Linux の場合は <kbd>Ctrl + S</kbd> キー、macOS の場合は <kbd>Cmd + S</kbd> キーを使用することもできます。

1. エディターを終了します。ここでも [...] コンテキスト メニューを使用できます。また、アクセラレータ キー (<kbd>Ctrl + Q</kbd>) を使用することもできます。

1. ファイルはファイル システム上に存在する必要があります。 `ls` コマンドを使用して、フォルダーの内容をリストしてみます。 ファイルに対して `cat` を実行して、内容を確認することもできます。

### <a name="create-the-virtual-machines"></a>仮想マシンを作成する

次は、Azure CLI を使用して 3 つの Ubuntu Linux 仮想マシンを作成してみましょう。 ここではオプションについて説明しませんが、CLI での VM 管理の詳細に関心をお持ちの場合は、「**Azure CLI を使用して仮想マシンを管理する**」モジュールをご覧ください。

各 VM の仮想ネットワーク インターフェイスを作成する必要もあります。 ループを使用して、それらをまとめて作成することができます。

1. `vm-create` コマンドを使用して、ループ内に 3 つの仮想マシンを作成します。 以下のコードを使用して、次の操作を行うことができます。
    - _woodgrove_NicX_ という名前の NIC を作成します。ここで、X は [1,2,3] となります。
    - _woodgrove-VMX_ という名前の VM を作成します。ここで、X は [1,2,3] となります。
    - 作成された VNET に各 NIC を関連付ける
    - 各 VM を NIC と可用性セットに関連付けます。

    ```azurecli
    for i in `seq 1 3`; do

        az network nic create --name woodgrove-Nic$i \
            --vnet-name woodgrove-VNET \
            --subnet backendSubnet

        az vm create --name woodgrove-VM$i \
            --availability-set woodgrove-avail-set \
            --nics woodgrove-Nic$i \
            --image UbuntuLTS \
            --admin-username azureuser \
            --generate-ssh-keys \
            --custom-data cloud-init.txt \
            --no-wait
    done
    ```
    > [!TIP]
    > ここでは、コマンドによって管理者名が "azureuser" に設定されます。これを任意の名前に変更することができます。 SSH キーよりユーザー名/パスワードの方がよい場合は、パスワードを指定することもできます。

1. この実行にはしばらく時間がかかります。ループが完了しても、VM は完全には使用できません。 Azure portal に切り替え、**[すべてのリソース]** 表示を使用して、作成されたリソースを確認することができます。

1. また、`vm list` コマンドを使用して、デプロイされた VM の数を確認することもできます。

    ```azurecli
    az vm list -o table
    ```

次は、ロード バランサーの構成方法について説明します。