<span data-ttu-id="d7ab7-101">このユニットでは、Azure CLI を使用して Computer Vision API サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-101">In this unit, you will create a Computer Vision API service using the Azure CLI.</span></span>

# <a name="exercise-create-a-computer-vision-service"></a><span data-ttu-id="d7ab7-102">演習: Computer Vision サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="d7ab7-102">Exercise: Create a Computer Vision Service</span></span>

<span data-ttu-id="d7ab7-103">[Computer Vision](/azure/cognitive-services/computer-vision/home) は、誰でもサービスとして使用できる機械学習アルゴリズムの完全なセットである [Microsoft Cognitive Services](/azure/cognitive-services/welcome) の一部です。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-103">[Computer Vision](/azure/cognitive-services/computer-vision/home) is part of [Microsoft Cognitive Services](/azure/cognitive-services/welcome), which is a complete set of machine learning algorithms available as a service for anyone to use.</span></span> <span data-ttu-id="d7ab7-104">十分なデータを生成して最初からモデルをトレーニングする代わりに、事前トレーニング済みモデルを誰でも使用できるようになっています。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-104">Instead of generating enough data to train a model from scratch, we make available pre-trained models for anyone to use.</span></span>

<span data-ttu-id="d7ab7-105">使用可能な多くのサービスの一部として、音声、画像、動画、検索、言語などを処理できるサービスが提供されています。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-105">Among the many services available, we provide services that can handle voice, image, video, search, language, and more.</span></span>

<span data-ttu-id="d7ab7-106">この演習では、画像を処理できるようにするために必要なサービスの作成に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-106">In this exercise, we're going to focus on creating the necessary service that will allow us to handle images.</span></span> <span data-ttu-id="d7ab7-107">この演習が済むと、画像を識別できる API エンドポイントを作成できるようになります。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-107">After this exercise, you will be able to create an API endpoint that will be able to identify images.</span></span>

# <a name="create-a-computer-vision-api-service"></a><span data-ttu-id="d7ab7-108">Computer Vision API サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="d7ab7-108">Create a Computer Vision API service</span></span>

<span data-ttu-id="d7ab7-109">Computer Vision API サービスを作成する前に、そのデプロイ先となる "*リソース グループ*" が必要です。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-109">Before you create your Computer Vision API service, you need a *resource group* to deploy it to.</span></span> <span data-ttu-id="d7ab7-110">リソース グループは、Azure リソースをまとめてデプロイして管理するための論理上のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-110">A resource group is a logical collection into which all Azure resources are deployed and managed.</span></span>

```azurecli
az group create --name ComputerVisionRG --location westus2
```

<span data-ttu-id="d7ab7-111">リソース グループを作成した後、`az cognitiveservices account create` コマンドを使用してサービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7ab7-111">Once you've created the resource group, create the service with the `az cognitiveservices account create` command.</span></span> <span data-ttu-id="d7ab7-112">お使いのサービスの名前</span><span class="sxs-lookup"><span data-stu-id="d7ab7-112">The name of your service</span></span> 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
