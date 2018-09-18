### <a name="create-an-ubuntu-data-science-vm"></a>Ubuntu Data Science VM の作成

Linux 用の Data Science Virtual Machine は、データ サイエンスの使用の開始を簡単にする仮想マシン イメージです。 すぐに起動して実行できるように、複数のツールが既に構築、インストール、および構成されています。 [Jupyter](http://jupyter.org/)、一部のサンプル Jupyter ノートブック、[TensorFlow](https://www.tensorflow.org/) のように、NVIDIA GPU ドライバー、[NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads)、[NVIDIA CUDA ディープ ニューラル ネットワーク](https://developer.nvidia.com/cudnn) (cuDNN) ライブラリも含まれます。 事前にインストールされているフレームワークはすべて、GPU 対応ですが、CPU でも動作します。 このユニットでは、Azure で Linux Data Science Virtual Machine (DSVM) のインスタンスを作成します。

1. ブラウザーで [Azure portal](https://portal.azure.com/?azure-portal=true) を開きます。

1. ポータルの左側にあるメニューで **[リソースの作成]** をクリックし、検索ボックスに「data science」と入力します。 検索一覧から **[Linux (Ubuntu) Data Science Virtual Machine]** を選択します。

    ![Ubuntu Data Science VM の検索](../media-draft/1-new-data-science-vm.png)

1. 少しばかり時間をとって、VM に含まれているツールの一覧をご確認ください。 次に、ブレードの下部にある **[作成]** をクリックします。

1. [基本] ブレードに次のように入力します。 12 文字以上で大文字、小文字、数字、特殊文字を含むパスワードを指定します。 "*入力したユーザー名とパスワードは必ず覚えておいてください。後にモジュールで必要になります。*"

    ![VM に関する基本的な情報の入力。](../media-draft/1-create-data-science-vm-1.png)

1. [サイズの選択] ブレードで **[DS1_V2 Standard]** を選択します。データ サイエンス VM を低コストで実験できます。 次に、ブレードの下部にある **[選択]** ボタンをクリックします。

    ![VM サイズの選択](../media-draft/1-create-data-science-vm-2.png)

1. **[設定]** ブレードで、既定値のまま **[OK]** ボタンをクリックします。

1. **[作成]** ブレードで少し時間をとって VM に選択したオプションを見直し、**[作成]** をクリックして VM 作成プロセスを開始します。

    ![VM の作成](../media-draft/1-create-data-science-vm-4.png)

1. ポータルの左側のメニューで **[リソース グループ]** をクリックします。 次に **data-science-rg** リソース グループをクリックします。

    ![リソース グループを開く](../media-draft/1-open-resource-group.png)

  
1. "デプロイ中" が "成功" に変わり、DSVM と補助 Azure リソースが作成されたことが示されるまで待ちます。 通常、デプロイに要する時間は 5 分以下です。 ブレードの上部にある **[更新]** をときどきクリックして、デプロイの状態を更新します。

    ![デプロイ状態の監視](../media-draft/1-deployment-succeeded.png)
