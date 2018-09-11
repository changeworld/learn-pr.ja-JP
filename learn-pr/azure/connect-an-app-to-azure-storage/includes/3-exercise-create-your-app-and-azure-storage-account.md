この演習では、.NET Core コンソール アプリケーションと Azure Storage アカウントを作成します。

## <a name="create-a-net-core-console-application"></a>.NET Core コンソール アプリケーションを作成する

Visual Studio 2017 を使用してアプリを作成します。

1. Visual Studio 2017 で、**[ファイル]** > **[新規]** > **[プロジェクト]** メニュー オプションの順に選択します。
1. **[Visual C# - .NET Core]** カテゴリで **[Console App (.NET Core)]\(コンソール アプリ (.NET Core)\)** を選択し、アプリの名前を入力して **[OK]** を選択します。
  ![新しいアプリ](..\media-draft\0-new-console-app.png)

## <a name="create-an-azure-storage-account"></a>Azure Storage アカウントを作成する

Azure Storage アカウントに接続するには、まず接続するストレージ アカウントを作成する必要があります。

1. Azure portal の左上にある [リソースの作成] を選択します。
1. 表示される選択ウィンドウで [ストレージ] を選択します。
1. そのウィンドウの右側にある [ストレージ アカウント - Blob、File、Table、Queue] を選択します。
  ![ポータルの選択](..\media-draft\1-portal-storage-select.png)
1. ストレージ アカウントの詳細を入力する必要があります。 ストレージ アカウントの名前を入力します。
1. リソース グループは、リソースの論理コンテナーです。 リソース グループの選択については、**[新規作成]** を選択し、リソース グループの名前を入力します。
1. その他のオプションはすべて既定値のままにして、**[作成]** をクリックします。
  ![ポータルの詳細](..\media-draft\2-portal-storage-details.png)。

これでリソース グループがプロビジョニングされます。 その後に、ストレージ アカウントがリソース グループ内に作成されます。
さらにアプリケーションを作成したので、ストレージ アカウントを構成して接続する準備が整いました。
