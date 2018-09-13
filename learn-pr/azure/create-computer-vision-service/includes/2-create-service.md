このユニットでは、Azure CLI を使用して Computer Vision API サービスを作成します。

# <a name="exercise-create-a-computer-vision-service"></a>演習: Computer Vision サービスを作成する

[Computer Vision](/azure/cognitive-services/computer-vision/home) は、誰でもサービスとして使用できる機械学習アルゴリズムの完全なセットである [Microsoft Cognitive Services](/azure/cognitive-services/welcome) の一部です。 十分なデータを生成して最初からモデルをトレーニングする代わりに、事前トレーニング済みモデルを誰でも使用できるようになっています。

使用可能な多くのサービスの一部として、音声、画像、動画、検索、言語などを処理できるサービスが提供されています。

この演習では、画像を処理できるようにするために必要なサービスの作成に焦点を当てます。 この演習が済むと、画像を識別できる API エンドポイントを作成できるようになります。

# <a name="create-a-computer-vision-api-service"></a>Computer Vision API サービスを作成する

Computer Vision API サービスを作成する前に、そのデプロイ先となる "*リソース グループ*" が必要です。 リソース グループは、Azure リソースをまとめてデプロイして管理するための論理上のコレクションです。

```azurecli
az group create --name ComputerVisionRG --location westus2
```

リソース グループを作成した後、`az cognitiveservices account create` コマンドを使用してサービスを作成します。 お使いのサービスの名前 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
