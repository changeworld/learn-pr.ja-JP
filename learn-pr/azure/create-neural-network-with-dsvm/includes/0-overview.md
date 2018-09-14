**データ サイエンス仮想マシン**Linux データ サイエンスの概要を簡単にする仮想マシン イメージのです。複数のツールが既に構築された、インストールされと簡単になるを取得するように構成します。 Jupyter や TensorFlow のように、NVIDIA GPU ドライバー、NVIDIA CUDA、NVIDIA CUDA ディープ ニューラル ネットワーク (cuDNN) ライブラリも含まれます。 事前にインストールされているフレームワークはすべて、GPU 対応ですが、CPU でも動作します。

## <a name="learning-objectives"></a>学習の目的

このモジュールでは、次のことを行います。

- Azure で Linux Data Science Virtual Machine を作成します。
- リモート デスクトップを使って DSVM に接続します。
- 画像を、ホットドッグが含まれているものと、含まれていないものに分類できるように、TensorFlow モデルをトレーニングします。
- Python アプリでモデルを使用します。

### <a name="prerequisites"></a>前提条件
<!---TODO: This is really long, need to make more concise and also add to index.yml--->
<!---TODO: Update for free sandbox.--->

このモジュールを完了するには、Azure サブスクリプションと Xfce リモート デスクトップ クライアントを必要があります。

 1. **Microsoft Azure アカウント**: このモジュールには有効かつアクティブな Azure アカウントが必要です。 アカウントがない場合でも、[無料試用版](https://azure.microsoft.com/free/)にサインアップできます

    * Visual Studio のアクティブ サブスクライバーの場合は、1 か月あたり $50 から $150 クレジットを取得できます。 毎月 Azure クレジットをアクティブ化して使用を開始する方法などの詳細については、この[リンク](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)を参照してください。

    * Visual Studio サブスクライバーでない場合は、無料の [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) プログラムにサインアップして、**Azure の無料アカウント** (1 年間の無料サービス、最初の 1 か月間の $200 を含む) を作成できます。

    * 学生の場合は、無料の [Microsoft Azure for Students](https://aka.ms/azure4students) アカウントにサインアップして、クレジット カードなしで、無料の Azure クレジットとして $100 と 1 年間の無料サービスを受け取ることができます。 

1. [X2Go](https://wiki.x2go.org/doku.php/download:start) などの [Xfce](https://xfce.org/) リモートデスクトップ クライアント