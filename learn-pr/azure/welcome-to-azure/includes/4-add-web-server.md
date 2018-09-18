これで、VM が稼働したので、これを使って何かしてみましょう。 ここでは、Web サーバーをインストールし、VM のホスト名を表示する基本的な Web ページを提供します。

VM を構成するには、いくつかの選択肢があります。 直接接続して、対話形式でシステムを構成することができます。 たとえば、Windows システムでは、リモート デスクトップ セッションを作成してリモート Windows コンピューターの UI に接続して、まるで自分のコンピューターのように操作することができます。 Linux システムでは、SSH 接続を作成して、ターミナルからご自分のリモート Linux システムを安全に使用することができます。

手動構成は出発点としては有効ですが、システムを追加するときに、デプロイを自動化できます。 オートメーションには、あなたの代わりに面倒な作業を処理してくれるプログラムやスクリプトなどの反復可能なプロセスを実行する必要があります。

::: zone pivot="windows-cloud"

ここでは、カスタム スクリプト拡張機能と呼ばれる、Windows ベースの Azure 仮想マシンの機能を使用して、Cloud Shell セッションからリモートで IIS を構成します。

::: zone-end

::: zone pivot="linux-cloud"

ここでは、カスタム スクリプト拡張機能と呼ばれる、Linux ベースの Azure 仮想マシンの機能を使用して、Cloud Shell セッションからリモートで Nginx を構成します。

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a>IIS とは

インターネット インフォメーション サービス (IIS) は、Windows で実行される Web サーバーです。 IIS を使用して、標準的な Web コンテンツ (HTML、CSS、および JavaScript) を提供したり、ASP.NET やその他の Web アプリケーションを実行することができます。 IIS は Windows Server に付属していますが、Web ページの提供を開始するには、アクティブ化する必要があります。

## <a name="whats-the-custom-script-extension"></a>カスタム スクリプト拡張機能とは

カスタム スクリプト拡張機能は、Azure VM でスクリプトをダウンロードして実行する簡単な方法です。 これは、VM が稼働してから VM を構成できるさまざまな方法の 1 つに過ぎません。

スクリプトは、Azure Storage に格納することも、GitHub などの公開されている場所に格納することもできます。 スクリプトは、手動で実行することも、より自動化されたデプロイの一部として実行することもできます。 ここでは、Azure CLI コマンドを実行して、GitHub から PowerShell スクリプトをダウンロードし、VM でそれを実行します。 このスクリプトは IIS を構成します。

## <a name="configure-iis"></a>IIS を構成する

ここでは、カスタム スクリプト拡張機能を使用して、Cloud Shell から VM に IIS をリモートで構成します。 また、ポート 80 (HTTP) で受信ネットワーク アクセスを許可するようにファイアウォールを構成します。

1. Cloud Shell から `az vm extension set` コマンドを実行して、IIS をインストールして基本的なホーム ページを構成する PowerShell スクリプトをダウンロードして実行します。

    ```azurecli
    az vm extension set \
      --resource-group myResourceGroup \
      --vm-name myWindowsVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    プロセスが完了するまでに数分かかります。

    それまで、別のブラウザー タブから、[PowerShell スクリプトを調べる](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true)こともできます。 このスクリプトにより、IIS がインストールされ、VM のコンピューター名 "myWindowsVM" と共にウェルカム メッセージを表示するようにホーム ページが構成されます。

1. この `az vm open-port` コマンドを実行して、ファイアウォールを経由してポート 80 (HTTP) を開きます。

    ```azurecli
    az vm open-port \
      --name myWindowsVM \
      --resource-group myResourceGroup \
      --port 80
    ```

## <a name="verify-the-configuration"></a>構成を確認する

これで IIS が設定されたので、実行されていることを確認しましょう。

1. この `az vm list-ip-addresses` コマンドを実行して、VM のパブリック IP を一覧表示します。

    ```azurecli
    az vm list-ip-addresses \
      --name myWindowsVM \
      --resource-group myResourceGroup \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > このコマンドは非常に複雑で、`--query` 引数は特に複雑です。 しかし、Azure をさらに深く探究すれば、これに関するプロになれます。

    ご自分の VM のパブリック IP アドレスが表示されます。 次に例を示します。

    ```console
    104.211.9.245
    ```

1. 新しいブラウザー タブで、ご自分の VM の IP アドレスに移動します。 ウェルカム メッセージと VM の名前が表示されます。

    ![](../media/iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a>Nginx とは

Nginx (発音は "エンジンエックス") は、UNIX、Linux、macOS、および Windows で実行される、一般的な無料のオープン ソースの Web サーバーです。 ここでは、Nginx を使用して基本的な Web ページを提供します。

## <a name="whats-the-custom-script-extension"></a>カスタム スクリプト拡張機能とは

カスタム スクリプト拡張機能は、Azure VM でスクリプトをダウンロードして実行する簡単な方法です。 これは、VM が稼働してから VM を構成できるさまざまな方法の 1 つに過ぎません。

スクリプトは、Azure Storage に格納することも、GitHub などの公開されている場所に格納することもできます。 スクリプトは、手動で実行することも、より自動化されたデプロイの一部として実行することもできます。 ここでは、Azure CLI コマンドを実行して、GitHub から Bash スクリプトをダウンロードし、VM でそれを実行します。 このスクリプトは Nginx を構成します。

## <a name="configure-nginx"></a>Nginx を構成する

ここでは、カスタム スクリプト拡張機能を使用して、Cloud Shell から VM に Nginx をリモートで構成します。 また、ポート 80 (HTTP) で受信ネットワーク アクセスを許可するようにファイアウォールを構成します。

1. Cloud Shell から `az vm extension set` コマンドを実行して、Nginx をインストールして基本的なホーム ページを構成する、Bash スクリプトをダウンロードして実行します。

    ```azurecli
    az vm extension set \
      --resource-group myResourceGroup \
      --vm-name myLinuxVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    プロセスが完了するまでに数分かかります。

    それまで、別のブラウザー タブから、[Bash スクリプトを調べる](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh?azure-portal=true)こともできます。 このスクリプトにより、Nginx がインストールされ、VM のコンピューター名 "myLinuxVM" と共にウェルカム メッセージを表示するようにホーム ページが構成されます。

1. この `az vm open-port` コマンドを実行して、ファイアウォールを経由してポート 80 (HTTP) を開きます。

    ```azurecli
    az vm open-port \
      --name myLinuxVM \
      --resource-group myResourceGroup \
      --port 80
    ```

## <a name="verify-the-configuration"></a>構成を確認する

これで Nginx が設定されたので、実行されていることを確認しましょう。

1. この `az vm list-ip-addresses` コマンドを実行して、VM のパブリック IP を一覧表示します。

    ```azurecli
    az vm list-ip-addresses \
      --name myLinuxVM \
      --resource-group myResourceGroup \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > このコマンドは非常に複雑で、`--query` 引数は特に複雑です。 しかし、Azure をさらに深く探究すれば、これに関するプロになれます。

    ご自分の VM のパブリック IP アドレスが表示されます。 次に例を示します。

    ```console
    137.135.110.210
    ```

1. 新しいブラウザー タブで、ご自分の VM の IP アドレスに移動します。 ウェルカム メッセージと VM の名前が表示されます。

    ![](../media/nginx-browser.png)

::: zone-end

## <a name="summary"></a>まとめ

VM が実行され、Web ページを提供できるようになりましたが、これはあなたにとってどんな意味がありますか。

すべての体験は基本から始まり、企業の規模に関わらず、クラウドから誕生したほぼすべての優れたイノベーションも、始まりはあなたと同様のセットアップであったことを思い出してください。 アイデアの進化に伴って、ご自分のビジネスとユーザーにプラスの影響が出始めます。