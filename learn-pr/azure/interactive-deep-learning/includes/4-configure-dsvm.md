## <a name="introduction-to-jupyter-for-more-interactive-deep-learning"></a><span data-ttu-id="63ba0-101">ディープ ラーニングをいっそう対話型にするための Jupyter の概要</span><span class="sxs-lookup"><span data-stu-id="63ba0-101">Introduction to Jupyter for more interactive deep learning</span></span> 

<span data-ttu-id="63ba0-102">Jupyter Notebook はオープン ソースの Web アプリケーションであり、ライブ コード、数式、視覚化、および説明テキストが含まれるドキュメントを作成して共有することができます。</span><span class="sxs-lookup"><span data-stu-id="63ba0-102">Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations, and narrative text.</span></span> <span data-ttu-id="63ba0-103">用途: データのクリーニングと変換、数値のシミュレーション、統計的モデリング、データの視覚化、機械学習、その他。</span><span class="sxs-lookup"><span data-stu-id="63ba0-103">Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.</span></span>

## <a name="serving-jupyter-notebooks-with-nvidia-docker-on-an-azure-dsvm"></a><span data-ttu-id="63ba0-104">Azure DSVM で Nvidia Docker を使用して Jupyter Notebook を提供する</span><span class="sxs-lookup"><span data-stu-id="63ba0-104">Serving Jupyter Notebooks with Nvidia Docker on an Azure DSVM</span></span>

### <a name="step-1-create-a-linux-dsvm"></a><span data-ttu-id="63ba0-105">手順 1: Linux DSVM を作成する</span><span class="sxs-lookup"><span data-stu-id="63ba0-105">Step 1 Create a Linux DSVM</span></span>

<span data-ttu-id="63ba0-106">Azure CLI から呼び出しコードを使用します</span><span class="sxs-lookup"><span data-stu-id="63ba0-106">Use call code from the Azure CLI</span></span>

```
code .
```

<span data-ttu-id="63ba0-107">次の展開スキーマを入力し、parameter_file.json として保存します</span><span class="sxs-lookup"><span data-stu-id="63ba0-107">Fill in the following deployment schema and save it as parameter_file.json</span></span>

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

<span data-ttu-id="63ba0-108">使用可能な VM のサイズの一覧は、こちらの [Ubuntu DSVM ARM テンプレート](https://azure.microsoft.com/en-us/global-infrastructure/services/?WT.mc_id=blog-learning-abornst)で見つかります。</span><span class="sxs-lookup"><span data-stu-id="63ba0-108">A list of available vm sizes can be found here [Ubuntu DSVM ARM template](https://azure.microsoft.com/en-us/global-infrastructure/services/?WT.mc_id=blog-learning-abornst).</span></span>


### <a name="create-a-resource-group-for-your-dsvm-in-a-region-of-your-choice"></a><span data-ttu-id="63ba0-109">選択したリージョンで DSVM 用のリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="63ba0-109">Create a resource group for your DSVM in a region of your choice:</span></span>
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

<span data-ttu-id="63ba0-110">使用できるリージョンの一覧は、こちらの [Azure リージョン](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="63ba0-110">A list of available regions can be found here [Azure Regions](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).</span></span>

### <a name="deploy-your-dsvm-to-your-new-resource-group"></a><span data-ttu-id="63ba0-111">新しいリソース グループに、DSVM を展開します</span><span class="sxs-lookup"><span data-stu-id="63ba0-111">Deploy your DSVM to your new resource group</span></span>

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## <a name="step-2-open-the-port-8888-22-on-the-dsvm"></a><span data-ttu-id="63ba0-112">手順 2: DSVM でポート 8888、22 を開く</span><span class="sxs-lookup"><span data-stu-id="63ba0-112">Step 2 Open the Port 8888, 22 on the DSVM</span></span> 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

<span data-ttu-id="63ba0-113">ポート 8888 は Jupyter Notebook の既定のポートです。ポートを開く詳しい手順については、[こちらをクリック](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)してください</span><span class="sxs-lookup"><span data-stu-id="63ba0-113">Port 8888 is the default port for Jupyter Notebooks For detailed steps on opening a port [click here](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)</span></span>
 
## <a name="step-3-connect-to-the-dsvm-with-the-azure-shell"></a><span data-ttu-id="63ba0-114">手順 3: Azure シェルを使用して DSVM に接続する</span><span class="sxs-lookup"><span data-stu-id="63ba0-114">Step 3 Connect to the DSVM with the Azure Shell</span></span> 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## <a name="step-4-run-jupyter-in-docker-container--link-8888-port-to-the-vm-host"></a><span data-ttu-id="63ba0-115">手順 4: Docker コンテナー内の Jupyter を実行し、8888 ポートを VM ホストにリンクする</span><span class="sxs-lookup"><span data-stu-id="63ba0-115">Step 4 Run Jupyter in Docker Container & link 8888 port to the VM Host</span></span> 

<span data-ttu-id="63ba0-116">VM と Docker コンテナーの間のポート 8888 をリンクし、Jupyter をインストールし、pytorch チュートリアルをプルします。</span><span class="sxs-lookup"><span data-stu-id="63ba0-116">Link port 8888 between the VM and the docker container, install jupyter and pull pytorch tutorials.</span></span>  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## <a name="step-5-navigate-to-the-jupyter-notebook-in-the-browser"></a><span data-ttu-id="63ba0-117">手順 5: ブラウザーで Jupyter Notebook に移動する</span><span class="sxs-lookup"><span data-stu-id="63ba0-117">Step 5 Navigate to the Jupyter Notebook in the Browser</span></span> 

<span data-ttu-id="63ba0-118">Jupyter Notebook が実行すると、次のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="63ba0-118">Once the Jupyter notebook is running you will see a message as follows :</span></span> 

> <span data-ttu-id="63ba0-119">Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken} (初めて接続するときは、この URL をコピーしてブラウザーに貼り付け、トークンを使用してログインします: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken})</span><span class="sxs-lookup"><span data-stu-id="63ba0-119">Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}</span></span>

<span data-ttu-id="63ba0-120">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the url with **[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com** and navigate to the address  in a new a tab in your browser: (URL の http://(5b8783e7911d or 127.0.0.1) の部分を [[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com に置き換え、ブラウザーの新しいタブでアドレスに移動します。)</span><span class="sxs-lookup"><span data-stu-id="63ba0-120">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the url with **[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com** and navigate to the address  in a new a tab in your browser:</span></span>
- <span data-ttu-id="63ba0-121">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="63ba0-121">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span></span>
