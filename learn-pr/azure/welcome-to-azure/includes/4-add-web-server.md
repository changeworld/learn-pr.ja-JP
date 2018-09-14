これで、VM は稼働しているが、何らかの処理にやってみましょう。 ここで、web サーバーをインストールしており、VM のホスト名を表示する基本的な web ページを使用します。

VM を構成するには、いくつかの選択肢があります。 直接接続でき、対話形式で、システムを構成することができます。 たとえば、Windows システム上で確実に挿入されたかのように、リモートの Windows コンピューターの UI に接続するリモート デスクトップ セッションを作成できます。 Linux のシステムでは、ターミナルから、リモートの Linux システムを安全に使用する SSH 接続を作成できます。

手動の構成が、有効な出発点ですが、システムを追加すると、デプロイを自動化できます。 Automation では、プログラムとの面倒な作業を処理するスクリプトなどの反復可能なプロセスを実行している必要があります。

::: zone pivot="windows-cloud"

ここでは、Windows に基づく Azure 仮想マシンを使用するカスタム スクリプト拡張機能と呼ばれる機能を使用して Cloud Shell セッションからリモートで IIS を構成します。

::: zone-end

::: zone pivot="linux-cloud"

ここでは、linux Azure 仮想マシンを使用するカスタム スクリプト拡張機能と呼ばれるの機能を使用して Cloud Shell セッションからリモートで Nginx を構成します。

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a>IIS とは何ですか。

インターネット インフォメーション サービス、または、IIS は、Windows で実行されている web サーバーです。 IIS を使用して、(HTML、CSS、および JavaScript) の標準的な web コンテンツを配信するか、ASP.NET およびその他の web アプリケーションを実行することができます。 Windows server で IIS が web ページの提供を開始するようにアクティブ化する必要があります。

## <a name="whats-the-custom-script-extension"></a>カスタム スクリプト拡張機能とは何ですか。

カスタム スクリプト拡張機能は、ダウンロードして、Azure Vm でスクリプトを実行する簡単な方法です。 後で、VM が稼働している VM を構成するさまざまな方法の 1 つだけになります。

スクリプトは、Azure storage または GitHub などの公共の場所に格納できます。 手動または自動化された展開の一部として、スクリプトを実行できます。 ここでは、GitHub から PowerShell スクリプトをダウンロードして、VM で実行する、Azure CLI コマンドを実行します。 このスクリプトは、IIS を構成します。

## <a name="configure-iis"></a>IIS を構成します。

ここで Cloud Shell から VM にリモートで IIS を構成するのにカスタム スクリプト拡張機能を使用します。 ポート 80 (HTTP) で受信ネットワーク アクセスを許可するファイアウォールを構成することもあります。

1. Cloud Shell から実行`az vm extension set`コマンドをダウンロードし、IIS をインストールし、基本的なホーム ページを構成する PowerShell スクリプトを実行します。

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myWindowsVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    この処理は、完了するまでに数分をかかります。

    それまでは、実行できます[PowerShell スクリプトを調べます](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true)たい場合は、別のブラウザー タブから。 スクリプトでは、IIS をインストールし、VM のコンピューター名"myWindowsVM"と共にようこそメッセージを表示するホーム ページを構成します。

1. この実行`az vm open-port`をファイアウォールでポート 80 (HTTP) を開くコマンド。

    ```azurecli
    az vm open-port \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>構成を確認する

IIS がセットアップしましょうが実行されていることを確認します。

1. この実行`az vm list-ip-addresses`VM のパブリック IP を一覧表示するコマンドに対応します。

    ```azurecli
    az vm list-ip-addresses \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > このコマンドは、特に非常に複雑な`--query`引数。 さらに、Azure を検討するとこの pro になります。

    VM のパブリック IP アドレスを参照してください。 次に例を示します。

    ```console
    104.211.9.245
    ```

1. 新しいブラウザー タブでは、VM の IP アドレスに移動します。 ウェルカム メッセージと、VM の名前が表示されます。

    ![](../media/iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a>Nginx とは何ですか。

Nginx (シー"エンジン-x") は、Unix、Linux、macOS、および Windows で実行されている一般的な無料、オープン ソースの web サーバーです。 ここで基本的な web ページを処理するために、Nginx を使用します。

## <a name="whats-the-custom-script-extension"></a>カスタム スクリプト拡張機能とは何ですか。

カスタム スクリプト拡張機能は、ダウンロードして、Azure Vm でスクリプトを実行する簡単な方法です。 後で、VM が稼働している VM を構成するさまざまな方法の 1 つだけになります。

スクリプトは、Azure storage または GitHub などの公共の場所に格納できます。 手動または自動化された展開の一部として、スクリプトを実行できます。 ここでは、Bash スクリプトを GitHub からダウンロードして、VM で実行し、Azure CLI コマンドを実行します。 このスクリプトは、Nginx を構成します。

## <a name="configure-nginx"></a>Nginx を構成します。

ここで Cloud Shell から VM にリモートで Nginx を構成するのにカスタム スクリプト拡張機能を使用します。 ポート 80 (HTTP) で受信ネットワーク アクセスを許可するファイアウォールを構成することもあります。

1. Cloud Shell から実行`az vm extension set`コマンドをダウンロードし、Nginx をインストールし、基本的なホーム ページを構成する Bash スクリプトを実行します。

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myLinuxVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    この処理は、完了するまでに数分をかかります。

    それまでは、実行できます[Bash スクリプトを調べます](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh?azure-portal=true)たい場合は、別のブラウザー タブから。 スクリプトでは、Nginx をインストールし、VM のコンピューター名"myLinuxVM"と共にようこそメッセージを表示するホーム ページを構成します。

1. この実行`az vm open-port`をファイアウォールでポート 80 (HTTP) を開くコマンド。

    ```azurecli
    az vm open-port \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>構成を確認する

Nginx がセットアップが実行されていることを確認しましょう。

1. この実行`az vm list-ip-addresses`VM のパブリック IP を一覧表示するコマンドに対応します。

    ```azurecli
    az vm list-ip-addresses \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > このコマンドは、特に非常に複雑な`--query`引数。 さらに、Azure を検討するとこの pro になります。

    VM のパブリック IP アドレスを参照してください。 次に例を示します。

    ```console
    137.135.110.210
    ```

1. 新しいブラウザー タブでは、VM の IP アドレスに移動します。 ウェルカム メッセージと、VM の名前が表示されます。

    ![](../media/nginx-browser.png)

::: zone-end

## <a name="summary"></a>まとめ

VM が実行されていると、web ページを使用できるようになりましたが、何を意味するのでしょうか。

注意してください、基本、および大量から、小規模企業から生まれたもので、クラウドでほぼすべての優れたイノベーションをすべて体験を開始する同様の設定の使用を開始します。 アイデアの進化に伴って、ビジネスとユーザーに正の影響を作成を開始します。