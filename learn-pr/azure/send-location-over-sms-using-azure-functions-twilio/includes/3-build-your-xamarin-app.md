この時点で、モバイル アプリはシンプルな "Hello World" アプリです。 このユニットでは、UI といくつかの基本的なアプリケーション ロジックを追加します。

アプリの UI は次のもので構成されています。

- 電話番号を入力するためのテキスト入力コントロール。
- Azure 関数を使用してこれらの番号に場所を送信するボタン。
- 現在の状態 (送信されている場所、正常に送信された場所など) についてのメッセージをユーザーに表示するラベル。

Xamarin.Forms では、Model-View-ViewModel (MVVM) と呼ばれる設計パターンがサポートされています。 MVVM について詳しくは、[Xamarin MVVM ドキュメント](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm)をご覧ください。MVVM の最も重要な点は、各ページ (View) がプロパティと動作を公開する ViewModel を持っていることです。

ViewModel のプロパティは名前によって UI に "バインド" されており、このバインドによって View と ViewModel の間でデータが同期されます。 たとえば、ViewModel の `Name` という名前の `string` プロパティを、UI のテキスト入力コントロールの `Text` プロパティにバインドすることができます。 テキスト入力コントロールには `Name` プロパティの値が表示され、ユーザーが UI のテキストを変更すると、`Name` プロパティが更新されます。 ViewModel で `Name` プロパティの値が変化すると、UI を更新するためのイベントが発生します。

ViewModel の動作はコマンドのプロパティとして公開されます。コマンドは、コマンドが呼び出されたときに実行されるアクションをラップするオブジェクトです。 これらのコマンドはボタンなどのコントロールに名前によってバインドされ、ボタンをタップするとコマンドが呼び出されます。

## <a name="create-a-base-viewmodel"></a>基底 ViewModel を作成する

すべての ViewModel で `INotifyPropertyChanged` インターフェイスが実装されています。 このインターフェイスの単一のイベント `PropertyChanged` は、UI に更新を通知するために使用されます。 このイベントには、変更されたプロパティの名前を含むイベント引数があります。 このインターフェイスを実装していくつかのヘルパー メソッドを提供する基底 ViewModel クラスを作成するのが一般的な方法です。

1. プロジェクトを右クリックし、*[追加] > [クラス]* の順に選択して、`BaseViewModel` という名前の `ImHere` .NET 標準プロジェクトに新しいクラスを作成します。新しいクラスの名前を "BaseViewModel" にして、**[追加]** をクリックします。

1. クラスを `public` にして、`INotifyPropertyChanged` から派生します。 `System.ComponentModel` に using ディレクティブを追加する必要があります。

1. `PropertyChanged` イベントを追加することで `INotifyPropertyChanged` インターフェイスを実装します。

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

1. ViewModel のプロパティの一般的なパターンでは、プライベート バッキング フィールドを持つパブリック プロパティを使用します。 プロパティ セッターで、新しい値に対してバッキング フィールドが確認されます。 新しい値がバッキング フィールドと異なる場合、バッキング フィールドが更新されて、`PropertyChanged` イベントが発生します。 このロジックを簡単に抽出して `Set` メソッドとして追加できます。 `System.Runtime.CompilerServices` 名前空間に対して using ディレクティブを追加する必要があります。

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    このメソッドは、バッキング フィールド、新しい値、プロパティ名への参照を受け取ります。 フィールドが変更されていない場合、メソッドは戻ります。変更されている場合は、フィールドが更新されて `PropertyChanged` イベントが発生します。 `Set` メソッドの `propertyName` パラメーターは既定のパラメーターであり、`CallerMemberName` 属性でマークされています。 このメソッドがプロパティ セッターから呼び出されるとき、このパラメーターは通常は既定値のままにされます。 コンパイラによって、呼び出し元プロパティの名前がパラメーターの値に自動的に設定されます。

このクラスの完全なコードは次のとおりです。

```cs
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace ImHere
{
    public class BaseViewModel : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
        {
            if (Equals(field, value)) return;
            field = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## <a name="create-a-viewmodel-for-the-page"></a>ページの ViewModel を作成する

`MainPage` には、電話番号のためのテキスト入力コントロールとメッセージ表示用のラベルがあります。 これらのコントロールは、ViewModel のプロパティにバインドされています。

1. `ImHere` .NET 標準プロジェクトに `MainViewModel` というクラスを作成します。

1. このクラスを public にして、`BaseViewModel` から派生します。

1. それぞれバッキング フィールドを備えた 2 つの `string` プロパティ `PhoneNumbers` と `Message` を追加します。 プロパティ セッターで、基底クラスの `Set` メソッドを使用して値を更新し、`PropertyChanged` イベントを発生させます。

   ```cs
    string message = "";
    public string Message
    {
        get => message;
        set => Set(ref message, value);
    }

    string phoneNumbers = "";
    public string PhoneNumbers
    {
        get => phoneNumbers;
        set => Set(ref phoneNumbers, value);
    }
   ```

1. 読み取り専用のコマンド プロパティ `SendLocationCommand` を追加します。 このコマンドは `System.Windows.Input` 名前空間の `ICommand` 型です。

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

1. クラスにコンストラクターを追加し、このコンストラクターで `SendLocationCommand` を `Xamarin.Forms` 名前空間の新しい `Command` として初期化します。 このコマンドのコンストラクターはコマンドが呼び出されたときに実行する `Action` を受け取るので、`SendLocation` という名前の `async` メソッドを作成し、この呼び出しを `await` するラムダ関数をコンストラクターに渡します。 `SendLocation` メソッドの本体は、このモジュールの後のユニットで実装します。 `System.Threading.Tasks` 名前空間に対して using ディレクティブを追加し、`Task` を戻すことができるようにする必要があります。

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

このクラスのコードは次のとおりです。

```cs
using System.Threading.Tasks;
using System.Windows.Input;
using Xamarin.Forms;

namespace ImHere
{
    public class MainViewModel : BaseViewModel
    {
        string message = "";
        public string Message
        {
            get => message;
            set => Set(ref message, value);
        }

        string phoneNumbers = "";
        public string PhoneNumbers
        {
            get => phoneNumbers;
            set => Set(ref phoneNumbers, value);
        }

        public MainViewModel()
        {
            SendLocationCommand = new Command(async () => await SendLocation());
        }

        public ICommand SendLocationCommand { get; }

        async Task SendLocation()
        {
        }
    }
}
```

## <a name="create-the-user-interface"></a>ユーザー インターフェイスを作成する

Xamarin.Forms UI は XAML を使用して作成できます。

1. `ImHere` プロジェクトから `MainPage.xaml` ファイルを開きます。 XAML エディターでページが開きます。

    注 - `ImHere.UWP` プロジェクトにも `MainPage.xaml` という名前のファイルが含まれています。 .NET 標準ライブラリのものを編集していることを確認してください。

1. ViewModel のプロパティにコントロールをバインドする前に、ページのバインディング コンテキストとして ViewModel のインスタンスを設定する必要があります。 最上位レベルの `ContentPage` の中に次の XAML を追加します。

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

1. `StackLayout` の内容を削除し、UI の見た目がよくなるように何かパディングを追加します。

    ```xml
    <StackLayout Padding="20">
    </StackLayout>
    ```

1. ユーザーが `StackLayout` に電話番号を追加するのに使用できる `Editor` コントロールを追加します。上の `Label` では、入力コントロールの用途を説明します。 `StackLayout` では子コントロールが追加された順序で水平方向または垂直方向に配置されるので、`Label` を最初に追加すると `Editor` の上に配置されます。 `Editor` コントロールは複数行入力コントロールであり、ユーザーは 1 行に 1 つずつ、複数の電話番号を入力できます。

    ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```

    `Editor` の `Text` プロパティは、`MainViewModel` の `PhoneNumbers` プロパティにバインドされます。 プロパティの値を `"{Binding <property name>}"` に設定するのがバインドの構文です。 中かっこにより、この値は特別であり、シンプルな `string` とは異なる方法で扱う必要があることを XAML コンパイラに指示します。

1. ユーザーの場所を送信するための `Button` を、`Editor` の下に追加します。

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    `Command` プロパティは、ViewModel の `SendLocationCommand` コマンドにバインドされています。 ボタンをタップすると、コマンドが実行されます。

1. ステータス メッセージを表示する `Label` を、`Button` の下に追加します。

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

このページの完全なコードは次のとおりです。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ImHere"
             x:Class="ImHere.MainPage">
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
        <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
                Command="{Binding SendLocationCommand}" />
        <Label Text="{Binding Message}"
               HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

アプリを実行して新しい UI を表示します。 この時点でバインドを検証する場合は、プロパティまたは `SendLocation` メソッドにブレークポイントを追加することによって行うことができます。

![アプリの新しい UI](../media-drafts/3-new-ui.png)

## <a name="summary"></a>まとめ

このユニットでは、XAML を使用してアプリの UI を作成する方法、およびアプリケーションのロジックを処理する ViewModel を作成する方法について説明しました。 ViewModel を UI にバインドする方法も学習しました。 次のユニットでは、ViewModel に場所の参照を追加します。