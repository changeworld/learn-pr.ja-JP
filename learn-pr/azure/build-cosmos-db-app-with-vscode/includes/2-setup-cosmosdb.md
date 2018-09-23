Visual Studio Code 用 Azure Cosmos DB 拡張機能は、コマンド ウィンドウを使用してリソースを作成できるようにすることで、アカウント、データベース、およびコレクションの作成を簡略化します。

このユニットでは、Visual Studio 用 Azure Cosmos DB 拡張機能をインストールした後、それを使用して、アカウント、データベース、およびコレクションを作成します。

## <a name="install-the-azure-cosmos-db-extension-for-visual-studio"></a>Visual Studio 用 Azure Cosmos DB 拡張機能をインストールする

1. [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) に移動し、Visual Studio Code 用 **Azure Cosmos DB** 拡張機能をインストールします。

1. 拡張機能タブが Visual Studio Code に読み込まれたら、**[インストール]** をクリックします。

1. インストールが完了したら、**[再読み込み]** をクリックします。

    Visual Studio Code によって以下が表示されます。 ![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) 拡張機能がインストールされ、再読み込みされた後の画面左側の Azure アイコン。

## <a name="create-an-azure-cosmos-db-account-in-visual-studio-code"></a>Visual Studio Code で Azure Cosmos DB アカウントを作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

1. Visual Studio Code で、**[表示]** > **[コマンド パレット]** の順にクリックし、「**Azure: Sign In**」と入力して、Azure にサインインします。 Azure: Sign In を使用するには、[Azure アカウント](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)拡張機能がインストールされている必要があります。

    > [!IMPORTANT]
    > サンドボックスを作成するために使用したアカウントで Azure にログインします。 サンドボックスは、コンシェルジェ サブスクリプションへのアクセスを提供します。

    プロンプトに従って、Web ブラウザーで提供されるコードをコピーして貼り付けます。これにより、Visual Studio Code セッションが認証されます。

1. 左側のメニューの![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure** アイコンをクリックします。次に、**コンシェルジェ サブスクリプション**を右クリックし、**[アカウントの作成]** をクリックします。

    コンシェルジェ サブスクリプションが表示されない場合は、確実に Visual Studio Code でサンドボックスを作成するために使用したアカウントで Azure にログインします。 また、Azure アカウント拡張機能で Azure サブスクリプションをフィルター処理した場合は、`> Azure: Select Subscriptions` コマンドでコンシェルジェ サブスクリプションがチェックされていることを確認します。

1. __+__ ボタンをクリックして、Cosmos DB アカウントの作成を開始します。 複数のサブスクリプションがある場合は、サブスクリプションを選択するように求められます。

1. 画面の上部にあるテキスト ボックスに、Azure Cosmos DB アカウントの一意の名前を入力し、Enter キーを押します。 アカウント名には、小文字、数字、'-' 文字のみを含めることができます。文字数は 3 から 31 文字にする必要があります。

1. 次に、**[SQL (DocumentDB)]** > **<rgn>[サンドボックス リソース グループ名]</rgn>** を選択し、場所を選択します。

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    Visual Studio Code の [出力] タブに、アカウント作成の進行状況が表示されます。完了まで数分かかります。

1. アカウントが作成されたら、**Azure: Cosmos DB** ウィンドウで Azure サブスクリプションを展開します。拡張機能によって新しい Azure Cosmos DB アカウントが表示します。 次の図では、新しいアカウントの名前は **learning-modules** になっています。

    ![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/azure-cosmos-db-vs-code-extension.png)

## <a name="create-an-azure-cosmos-db-database-and-collection-in-visual-studio-code"></a>Visual Studio Code で Azure Cosmos DB データベースとコレクションを作成する

次は、顧客用の新しいデータベースとコレクションを作成します。

1. Azure: Cosmos DB ウィンドウで、新しいアカウントを右クリックし、**[データベースの作成]** をクリックします。
1. 画面上部の入力パレットで、データベース名に「`Users`」と入力し、Enter キーを押します。
1. コレクション名に「`WebCustomers`」と入力し、Enter キーを押します。
1. パーティション キーに「`userId`」と入力し、Enter キーを押します。
1. 最後に、最初のスループット容量が `1000` であることを確認し、Enter キーを押します。
1. **[Azure: Cosmos DB]** ウィンドウでアカウントを展開します。新しい **Users** データベースと **WebCustomers** コレクションが表示されます。

    ![上記の手順を示すアニメーションは、Visual Studio Code の Azure Cosmos DB 拡張機能を実行します。](../media/2-setup/vs-code-azure-cosmos-db-extension.gif)

Azure Cosmos DB アカウントが作成されたので、Visual Studio Code での作業にとりかかることができます。
