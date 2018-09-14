## <a name="introduction-to-jupyter-for-more-interactive-deep-learning"></a>ディープ ラーニングをいっそう対話型にするための Jupyter の概要 

Jupyter Notebook はオープン ソースの Web アプリケーションであり、ライブ コード、数式、視覚化、および説明テキストが含まれるドキュメントを作成して共有することができます。 用途: データのクリーニングと変換、数値のシミュレーション、統計的モデリング、データの視覚化、機械学習、その他。

## <a name="serving-jupyter-notebooks-with-nvidia-docker-on-an-azure-dsvm"></a>Azure DSVM で Nvidia Docker を使用して Jupyter Notebook を提供する

### <a name="step-1-create-a-linux-dsvm"></a>手順 1: Linux DSVM を作成する

Azure CLI から呼び出しコードを使用します

```
code .
```

次の展開スキーマを入力し、parameter_file.json として保存します

``` 
{ 
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "YOURUSERNAME FOR NEW DSVM"},
     "adminPassword": { "value" : "PASSWORD FOR NEW DSVM"},
     "vmName": { "value" : "HOSTNAME OF DSVM"},
     "vmSize": { "value" : "VM SIZE For eg: Standard_DS2_v2"},
  }
}
```

使用可能な VM のサイズの一覧は、こちらの [Ubuntu DSVM ARM テンプレート](https://azure.microsoft.com/global-infrastructure/services/?WT.mc_id=blog-learning-abornst)で見つかります。


### <a name="create-a-resource-group-for-your-dsvm-in-a-region-of-your-choice"></a>選択したリージョンで DSVM 用のリソース グループを作成します。
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

使用できるリージョンの一覧は、こちらの [Azure リージョン](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json)をご覧ください。

### <a name="deploy-your-dsvm-to-your-new-resource-group"></a>新しいリソース グループに、DSVM を展開します

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## <a name="step-2-open-the-port-8888-22-on-the-dsvm"></a>手順 2: DSVM でポート 8888、22 を開く 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

ポート 8888 は Jupyter Notebook の既定のポートです。ポートを開く詳しい手順については、[こちらをクリック](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)してください
 
## <a name="step-3-connect-to-the-dsvm-with-the-azure-shell"></a>手順 3: Azure シェルを使用して DSVM に接続する 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## <a name="step-4-run-jupyter-in-docker-container--link-8888-port-to-the-vm-host"></a>手順 4: Docker コンテナー内の Jupyter を実行し、8888 ポートを VM ホストにリンクする 

VM と Docker コンテナーの間のポート 8888 をリンクし、Jupyter をインストールし、pytorch チュートリアルをプルします。  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## <a name="step-5-navigate-to-the-jupyter-notebook-in-the-browser"></a>手順 5: ブラウザーで Jupyter Notebook に移動する 

Jupyter Notebook が実行すると、次のようなメッセージが表示されます。 

> Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken} (初めて接続するときは、この URL をコピーしてブラウザーに貼り付け、トークンを使用してログインします: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken})

Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the url with **[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com** and navigate to the address  in a new a tab in your browser: (URL の http://(5b8783e7911d or 127.0.0.1) の部分を [[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com に置き換え、ブラウザーの新しいタブでアドレスに移動します。)
- [[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}
