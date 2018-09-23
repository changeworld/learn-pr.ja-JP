VM 上で最後に試してみたいのは、Web サーバーをインストールすることです。 インストールするのが最も簡単なパッケージの 1 つとして、`nginx` があります。

## <a name="install-nginx-web-server"></a>NGINX Web サーバーをインストールする

1. 使用する Linux 仮想マシンのパブリック IP アドレスを検索します。 この検索には、`vm list-ip-addresses` コマンドが使用できることに注意してください。

1. 次に、マシンのテストを行ったときと同様に、マシンへの `ssh` 接続を開きます。 管理者名 ("**aldis**") を渡す必要があることに注意してください。

1. 提示されたシェルで、次のコマンドを実行して `nginx` Web サーバーをインストールします。

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. Secure Shell を終了します。

```bash
exit
```

## <a name="retrieve-our-default-page"></a>既定のページを取得する

1. Azure Cloud Shell で、`curl` を使用してご利用の Linux Web サーバーから既定のページを読み取ります。 あるいは、新しいブラウザー タブを開いて、パブリック IP アドレスを参照することもできます。

```azurecli
curl 40.83.165.85
```

Linux 仮想マシンでは組み込みのファイアウォールを経由してポート 80 (`http`) が公開されないため、それは失敗します。 さいわい、Azure CLI にはそれ用のコマンドとして `vm open-port` があります。 

1. Cloud Shell に次の内容を入力して、ポート 80 を開きます。

```azurecli
az vm open-port --port 80 --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM
```

ネットワーク ルールを追加し、ファイアウォールを介してポートを開く処理には、しばらく時間がかかります。 もう一度 `curl` を試みてください。 今回は、それによってデータが返されます。 ブラウザーで該当するページを表示することもできます。

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
