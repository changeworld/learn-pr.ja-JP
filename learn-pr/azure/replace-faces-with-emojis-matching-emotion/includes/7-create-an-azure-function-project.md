最終的には、クラウドのどこかにコードをホストする必要があります。 Azure のテクノロジを使用して、これを行うには、Azure Functions です。

この講義では、次の説明は。

- Azure Function App を作成する方法と`JavaScript`Http トリガーします。
- 実行し、Azure 関数をローカルでデバッグする方法。

## <a name="create-an-azure-function-project"></a>Azure 関数プロジェクトを作成します。

Azure 関数プロジェクトは、複数の関数のコンテナーです。 さまざまな方法でトリガーされる関数私たちがするごとにトリガー関数の HTTP 要求を行います。

Azure Functions; を作成する方法はたくさんあります。このモジュール内で事前にインストールされている Azure の関数の拡張機能で Visual Studio Code を使用するでしょう。

まず、関数プロジェクトを作成してみましょう。

1. 型<kbd>Ctrl</kbd>+<kbd>P</kbd>コマンド パレットを起動し、検索して選択するには `Azure Functions: Create New Project...`

   > **注**
   >
   > など一部のファイルを上書きするかどうかがありますが問わ`.gitignore`、回答**ありません**すべてに!

   ![新しいプロジェクトの作成](/media-drafts/7.create-new-project.png)

2. (最初のオプションは、現在のフォルダー)、関数アプリを作成するフォルダーを選択します。

   ![フォルダーの選択](/media-drafts/7.select-folder.png)

3. 選択`JavaScript`目的の言語として。

   ![言語の選択](/media-drafts/7.select-language.png)

4. 正常に完了、上記の場合は表示のフォルダーに作成されたファイルの下

   ![アプリの作成](/media-drafts/7.app-created.png)

## <a name="create-an-azure-function"></a>Azure 関数の作成

それでは次に HTTP 要求に応答するコードの一部、Azure 関数自体を作成します。

もう一度 Visual Studio コード拡張機能を使用して行います。

1.  型<kbd>Ctrl</kbd>+<kbd>P</kbd>コマンド パレットを起動し、検索して選択するには `Azure Functions: Create Function...`

    ![新しい関数を作成します。](/media-drafts/7.create-function.png)

2.  以前で関数プロジェクトを作成したフォルダーを選択します (最初のオプションは、現在のフォルダー)

    ![フォルダーの選択](/media-drafts/7.select-current-project.png)

3.  ここでは作成、 _HTTP トリガー_、ようにするオプションを選択します。

    ![フォルダーの選択](/media-drafts/7.select-trigger.png)

4.  入力`MojifyImage`として作成する関数の名前

    ![名前を選択します。](/media-drafts/7.choose-function-name.png)

5.  選択`Anonymous`認証レベルとして

    > 注: 選択して`Anonymous`関数が、世界中に開かれ、安全でないです。

    ![名前を選択します。](/media-drafts/7.choose-auth-level.png)

## <a name="run-the-function-locally"></a>関数をローカルで実行する

関数のプロジェクトにスタート プロジェクトを変換するこれらのコマンドが正常に完了すると、 _HTTP トリガー_関数と呼ばれる`MojifyImage`します。

すべてが動作することを確認する関数アプリを実行できるよう、ローカルで。

```bash
func host start
```

関数のランタイムをローカルで起動します。場合、そのしくみを最終的に正しく表示されるはずのように、次のコンソールに出力します。

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

> **注**
>
> これは、Azure Functions ランタイムをインストールする利点の 1 つ、実稼働環境での関数の実行に使用される同じ基になるテクノロジを使用してローカルで関数を実行することができます。

関数が正常に動作させるには、出力はブラウザーのウィンドウで、コンソールに出力 URL にアクセスします。

![Function App が機能しています。](/media-drafts/7.default-function-app-working.png)

## <a name="debug-the-function-locally"></a>ローカル関数をデバッグします。

> **重要**
>
> 終了するかどうかを確認、 `func host start` Visual Studio Code を使用してデバッグする前に実行したコマンドします。

さらに、実行できる_およびデバッグ_visual studio のコード内でアプリケーション。

ブレークポイントを追加、`index.js`ファイル、JavaScript 関数の上部にあるようになります。

![ブレークポイントを追加します。](/media-drafts/7.add-breakpoint.png)

デバッグ モードの初回アクセス時に、[デバッグ] メニューから関数を実行する<kbd>Cmd<kbd> + <kbd>Shift<kbd> + <kbd>D<kbd>します。

並べ替えた`Attach to JavaScript Functions`最後に、デバッグ セッションを開始する緑の三角形を押す前に、デバッグ構成から。

> **注**
>
> `Attach to JavaScript Functions`関数プロジェクトを作成したときに、デバッグ構成が自動的に追加されました。

または、キー <kbd>F5<kbd>から任意の場所は、アプリケーションでこれを実行、前回のデバッグ構成します。

`func host start`コマンドを実行するは、ターミナルする必要がありますを開くことで最終的に同じ上記と同じ出力します。

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

デバッグのメニュー バーに表示されますを参照もデバッグしているために次のようにします。

![メニュー バーのデバッグ](/media-drafts/7.debug-menu-bar.png)

これで、ブレークポイントで中断が上記の URL にアクセスすると、指定したし、関数は、手順を実行できます。

<!-- TODO Find Link -->

できます[詳細](https://code.visualstudio.com/docs/editor/debugging)Visual Studio Code でのデバッグについて
