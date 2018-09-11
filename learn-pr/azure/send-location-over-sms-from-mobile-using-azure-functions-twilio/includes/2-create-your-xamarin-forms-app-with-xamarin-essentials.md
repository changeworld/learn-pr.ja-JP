ビルドするアプリケーションはクロスプラットフォームのモバイル アプリであり、Azure 関数と通信を行って場所を共有します。 このユニットでは、Visual Studio を使用して空のモバイル アプリを作成し、ユーザーの場所を取得するための API を含む NuGet パッケージをインストールします。

## <a name="create-the-xamarinforms-project"></a>Xamarin.Forms プロジェクトを作成する

1. Visual Studio で、*[ファイル]、[新規]、[プロジェクト]* の順に選択します。

2. 左側にあるツリーから、*[Visual C#]、[クロス プラットフォーム]* の順に選択し、中央のパネルから *[モバイル アプリ (Xamarin.Forms)]* を選びます。

3. ソリューションに "ImHere" という名前を付けます。

4. ソリューションに適した場所を選びます。

    > このモジュールを Windows でローカルに実行しており、Android 用のビルドを計画している場合は、パスをできるだけ短くしてください。 Android SDK にはパスの長さに関する制限があるため、ルート パスをできるだけ短くする必要があります。

5. Click **OK**.

    ![[新しいソリューション] ダイアログ](../media/2-new-solution-dialog.png)

6. **[New Cross Platform App]\(新しいクロス プラットフォーム アプリ\)** ダイアログで、*[空のアプリ]* テンプレートを選択します。

7. このモジュールでは、UWP アプリをビルドするため、iOS と Android をオフにし、UWP はオンのままにします。

    > ローカルでこれを実行する場合は、Android をオンのままにしておいてかまいません。Android SDK は、Visual Studio 内で *[.NET によるモバイル開発]* ワークロードの一部としてインストールされるためです。 また iOS 用にビルドする場合は、macOS ビルド エージェントにペアリングする必要があります。 これに関する詳細については、[Xamarin iOS のドキュメント](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/connecting-to-mac/)を参照してください。

8. *[コード共有方法]* では、**[.NET Standard]** を選択します。

9. Click **OK**.

    ![新しいソリューションの構成ダイアログ](../media/2-configure-solution-dialog.png)

Visual Studio では 2 つのプロジェクト (`ImHere.UWP` という UWP アプリと `ImHere` という .NET 標準ライブラリ) が作成されます。 Xamarin.Forms アプリは 2 つの部分、つまり、1 つ以上のプラットフォーム固有のアプリ プロジェクトと 1 つ (またはそれ以上の) .NET 標準ライブラリで構成されます。 プラットフォーム固有のアプリ プロジェクトには、関連するプラットフォームでアプリを実行するために必要なプラットフォーム固有のコードが含まれます。 その後、これらのプロジェクトでは、クロスプラットフォームの .NET 標準ライブラリで定義されている Xamarin.Forms アプリが起動されます。 クロスプラットフォーム コードでアプリをビルドすると、実行時に、作成するすべてのユーザー インターフェイスが、関連するプラットフォーム固有の UI コンポーネントに変換されます。

## <a name="adding-xamarinessentials"></a>Xamarin.Essentials の追加

UWP、Android、iOS プラットフォームでは、オペレーティング システムとハードウェアを利用する数多くの同様の機能が提供されます。 これらは似ていますが、API は大きく異なります。 クロスプラットフォーム コードからこれらの API を使用するには、.NET 標準ライブラリに公開するアプリ プロジェクトでプラットフォーム固有のコードを記述する必要があります。 [Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/) は NuGet パッケージであり、これらの多くの API でのクロスプラットフォームの抽象化が提供されます。これらの API には、ユーザーの場所を取得するためにアプリで使用する地理位置情報 API が含まれます。

1. Visual Studio のソリューション エクスプローラーで `ImHere` ソリューションを右クリックし、*[ソリューションの NuGet パッケージの管理]* を選択します。

2. **[参照]** タブを選択し、"Xamarin.Essentials" を検索します。 このパッケージは現在、プレリリース版の NuGet パッケージとして利用可能であるため、*[include prelease]\(プレリリースを含める\)* ボックスをオンにします。

3. **[Xamarin.Essentials]** NuGet パッケージを選択します。

4. 右側にあるプロジェクトの一覧で自分のプロジェクトをすべて確認します。

5. **[インストール]** ボタンをクリックして、NuGet パッケージをインストールします。 続行するにはライセンスに同意する必要があります。

    ![ソリューション内のすべてのプロジェクトへの Xamarin.Essentials NuGet パッケージの追加](../media/2-add-essentials-nuget.png)

    > このモジュールをローカルで実行し、Android を対象とする場合は、追加のセットアップをいくつか行う必要があります。 詳細については、[Xamarin.Essentials の概要に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/get-started?context=xamarin%2Fios&tabs=windows%2Candroid)を参照してください。

## <a name="building-and-running-the-app"></a>アプリのビルドと実行

1. ソリューション エクスプローラーで、`ImHere.UWP` プロジェクトを右クリックして *[スタートアップ プロジェクトに設定]* を選択します。

2. ビルド構成を **[デバッグ]** に、プラットフォームを **[x86]** に、実行先のデバイスを **[ローカル コンピューター]** にそれぞれ設定します。

    ![ローカル デバイスで実行する場合のデバッグ x86 構成の設定](../media/2-debug-configuration.png)

3. アプリのデバッグを開始します。

    ![実行中のアプリ](../media/2-debuging-app.png)

## <a name="summary"></a>まとめ

このユニットでは、新しい Xamarin.Forms クロスプラットフォームのモバイル アプリを作成し、Xamarin.Essentials NuGet パッケージを追加しました。 次は、モバイル アプリの UI とロジックを構築する方法を学習します。