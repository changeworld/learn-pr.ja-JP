VM 上で最後に試してみたいのは、Web サーバーをインストールすることです。 インストールするのが最も簡単なパッケージの 1 つとして、`nginx` があります。

1. 使用する Linux 仮想マシンのパブリック IP アドレスを検索します。 この検索には、`vm list-ip-addresses` コマンドが使用できることに注意してください。

2. 次に、マシンのテストを行ったときと同様に、マシンへの `ssh` 接続を開きます。 管理者名 ("**aldis**") を渡す必要があることに注意してください。

3. 提示されたシェルで、次のコマンドを実行して `nginx` Web サーバーをインストールします。

```azurecli
sudo apt-get -y update && sudo apt-get -y install nginx
```

4. Azure Cloud Shell で、`curl` を使用してご利用の Linux Web サーバーから既定のページを読み取ります。 あるいは、新しいブラウザー タブを開いて、パブリック IP アドレスを参照することもできます。

```azurecli
curl 168.61.54.62
```

Linux 仮想マシンでは組み込みのファイアウォールを経由してポート 80 (`http`) が公開されないため、それは失敗します。 さいわい、Azure CLI にはそれ用のコマンドとして `vm open-port` があります。 

5. Cloud Shell に次の内容を入力して、ポート 80 を開きます。

```
az vm open-port --port 80 --resource-group ExerciseResources --name SampleVM
```

最後に、`curl` をもう一度お試しください。 今回は、それによってデータが返されます。 ブラウザーで該当するページを表示することもできます。



