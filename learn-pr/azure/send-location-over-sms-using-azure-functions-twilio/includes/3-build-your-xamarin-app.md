<span data-ttu-id="ebc51-101">この時点で、モバイル アプリはシンプルな "Hello World" アプリです。</span><span class="sxs-lookup"><span data-stu-id="ebc51-101">At this point, the mobile app is a simple "Hello World" app.</span></span> <span data-ttu-id="ebc51-102">このユニットでは、UI といくつかの基本的なアプリケーション ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-102">In this unit, you add the UI and some basic application logic.</span></span>

<span data-ttu-id="ebc51-103">アプリの UI は次のもので構成されています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-103">The UI for the app will consist of:</span></span>

- <span data-ttu-id="ebc51-104">電話番号を入力するためのテキスト入力コントロール。</span><span class="sxs-lookup"><span data-stu-id="ebc51-104">A text-entry control to enter some phone numbers.</span></span>
- <span data-ttu-id="ebc51-105">Azure 関数を使用してこれらの番号に場所を送信するボタン。</span><span class="sxs-lookup"><span data-stu-id="ebc51-105">A button to send your location to those numbers using an Azure function.</span></span>
- <span data-ttu-id="ebc51-106">現在の状態 (送信されている場所、正常に送信された場所など) についてのメッセージをユーザーに表示するラベル。</span><span class="sxs-lookup"><span data-stu-id="ebc51-106">A label that will show a message to the user of the current status, such as the location being sent and location sent successfully.</span></span>

<span data-ttu-id="ebc51-107">Xamarin.Forms では、Model-View-ViewModel (MVVM) と呼ばれる設計パターンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-107">Xamarin.Forms supports a design pattern called Model-View-ViewModel (MVVM).</span></span> <span data-ttu-id="ebc51-108">MVVM について詳しくは、[Xamarin MVVM ドキュメント](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm?azure-portal=true)をご覧ください。MVVM の最も重要な点は、各ページ (View) がプロパティと動作を公開する ViewModel を持っていることです。</span><span class="sxs-lookup"><span data-stu-id="ebc51-108">You can read more about MVVM in the [Xamarin MVVM docs](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm?azure-portal=true), but the essence of it is, each page (View) has a ViewModel that exposes properties and behavior.</span></span>

<span data-ttu-id="ebc51-109">ViewModel のプロパティは名前によって UI に "バインド" されており、このバインドによって View と ViewModel の間でデータが同期されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-109">ViewModel properties are 'bound' to components on the UI by name, and this binding synchronizes data between the View and ViewModel.</span></span> <span data-ttu-id="ebc51-110">たとえば、ViewModel の `Name` という名前の `string` プロパティを、UI のテキスト入力コントロールの `Text` プロパティにバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-110">For example, a `string` property on a ViewModel called `Name` could be bound to the `Text` property of a text-entry control on the UI.</span></span> <span data-ttu-id="ebc51-111">テキスト入力コントロールには `Name` プロパティの値が表示され、ユーザーが UI のテキストを変更すると、`Name` プロパティが更新されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-111">The text-entry control shows the value in the `Name` property and, when the user changes the text in the UI, the `Name` property is updated.</span></span> <span data-ttu-id="ebc51-112">ViewModel で `Name` プロパティの値が変化すると、UI を更新するためのイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-112">If the value of the `Name` property is changed in the ViewModel, an event is raised to tell the UI to update.</span></span>

<span data-ttu-id="ebc51-113">ViewModel の動作はコマンドのプロパティとして公開されます。コマンドは、コマンドが呼び出されたときに実行されるアクションをラップするオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="ebc51-113">ViewModel behavior is exposed as command properties, a command being an object that wraps an action that is executed when the command is invoked.</span></span> <span data-ttu-id="ebc51-114">これらのコマンドはボタンなどのコントロールに名前によってバインドされ、ボタンをタップするとコマンドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-114">These commands are bound by name to controls like buttons, and tapping a button will invoke the command.</span></span>

## <a name="create-a-base-viewmodel"></a><span data-ttu-id="ebc51-115">基底 ViewModel を作成する</span><span class="sxs-lookup"><span data-stu-id="ebc51-115">Create a base ViewModel</span></span>

<span data-ttu-id="ebc51-116">すべての ViewModel で `INotifyPropertyChanged` インターフェイスが実装されています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-116">ViewModels all implement the `INotifyPropertyChanged` interface.</span></span> <span data-ttu-id="ebc51-117">このインターフェイスの単一のイベント `PropertyChanged` は、UI に更新を通知するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-117">This interface has a single event, `PropertyChanged`, which is used to notify the UI of any updates.</span></span> <span data-ttu-id="ebc51-118">このイベントには、変更されたプロパティの名前を含むイベント引数があります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-118">This event has event args that contain the name of the property that has changed.</span></span> <span data-ttu-id="ebc51-119">このインターフェイスを実装していくつかのヘルパー メソッドを提供する基底 ViewModel クラスを作成するのが一般的な方法です。</span><span class="sxs-lookup"><span data-stu-id="ebc51-119">It's common practice to create a base ViewModel class implementing this interface and providing some helper methods.</span></span>

1. <span data-ttu-id="ebc51-120">プロジェクトを右クリックし、*[追加] > [クラス]* の順に選択して、`BaseViewModel` という名前の `ImHere` .NET Standard プロジェクト内に新しいクラスを作成します。新しいクラスの名前を "BaseViewModel" にして、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ebc51-120">Create a new class in the `ImHere` .NET Standard project called `BaseViewModel` by right-clicking on the project, and then selecting *Add->Class...*. Name the new class "BaseViewModel" and click **Add**.</span></span>

1. <span data-ttu-id="ebc51-121">クラスを `public` にして、`INotifyPropertyChanged` から派生します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-121">Make the class `public` and derive from `INotifyPropertyChanged`.</span></span> <span data-ttu-id="ebc51-122">`System.ComponentModel` に using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-122">You'll need to add a using directive for `System.ComponentModel`.</span></span>

1. <span data-ttu-id="ebc51-123">`PropertyChanged` イベントを追加することで `INotifyPropertyChanged` インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-123">Implement the `INotifyPropertyChanged` interface by adding the `PropertyChanged` event:</span></span>

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

1. <span data-ttu-id="ebc51-124">ViewModel のプロパティの一般的なパターンでは、プライベート バッキング フィールドを持つパブリック プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-124">The common pattern for ViewModel properties is to have a public property with a private backing field.</span></span> <span data-ttu-id="ebc51-125">プロパティ セッターで、新しい値に対してバッキング フィールドが確認されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-125">In the property setter, the backing field is checked against the new value.</span></span> <span data-ttu-id="ebc51-126">新しい値がバッキング フィールドと異なる場合、バッキング フィールドが更新されて、`PropertyChanged` イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-126">If the new value is different to the backing field, the backing field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="ebc51-127">このロジックを簡単に抽出して `Set` メソッドとして追加できます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-127">This logic is easy to factor out into a method, so add the `Set` method.</span></span> <span data-ttu-id="ebc51-128">`System.Runtime.CompilerServices` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-128">You'll need to add a using directive for the `System.Runtime.CompilerServices` namespace.</span></span>

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    <span data-ttu-id="ebc51-129">このメソッドは、バッキング フィールド、新しい値、プロパティ名への参照を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-129">This method takes a reference to the backing field, the new value, and the property name.</span></span> <span data-ttu-id="ebc51-130">フィールドが変更されていない場合、メソッドは戻ります。変更されている場合は、フィールドが更新されて `PropertyChanged` イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-130">If the field hasn't changed, the method returns, otherwise, the field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="ebc51-131">`Set` メソッドの `propertyName` パラメーターは既定のパラメーターであり、`CallerMemberName` 属性でマークされています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-131">The `propertyName` parameter on the `Set` method is a default parameter and is marked with the `CallerMemberName` attribute.</span></span> <span data-ttu-id="ebc51-132">このメソッドがプロパティ セッターから呼び出されるとき、このパラメーターは通常は既定値のままにされます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-132">When this method is called from a property setter, this parameter is normally left as the default value.</span></span> <span data-ttu-id="ebc51-133">コンパイラによって、呼び出し元プロパティの名前がパラメーターの値に自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-133">The compiler will then automatically set the parameter value to be the name of the calling property.</span></span>

<span data-ttu-id="ebc51-134">このクラスの完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ebc51-134">The full code for this class is below.</span></span>

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

## <a name="create-a-viewmodel-for-the-page"></a><span data-ttu-id="ebc51-135">ページの ViewModel を作成する</span><span class="sxs-lookup"><span data-stu-id="ebc51-135">Create a ViewModel for the page</span></span>

<span data-ttu-id="ebc51-136">`MainPage` には、電話番号のためのテキスト入力コントロールとメッセージ表示用のラベルがあります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-136">The `MainPage` will have a text-entry control for phone numbers and a label to display a message.</span></span> <span data-ttu-id="ebc51-137">これらのコントロールは、ViewModel 上のプロパティにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-137">These controls will be bound to properties on a ViewModel.</span></span>

1. <span data-ttu-id="ebc51-138">`ImHere` .NET Standard プロジェクト内に `MainViewModel` というクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-138">Create a class called `MainViewModel` in the `ImHere` .NET Standard project.</span></span>

1. <span data-ttu-id="ebc51-139">このクラスを public にして、`BaseViewModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-139">Make this class public and derive from `BaseViewModel`.</span></span>

1. <span data-ttu-id="ebc51-140">それぞれバッキング フィールドを備えた 2 つの `string` プロパティ `PhoneNumbers` と `Message` を追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-140">Add two `string` properties, `PhoneNumbers` and `Message`, each with a backing field.</span></span> <span data-ttu-id="ebc51-141">プロパティ セッターで、基底クラスの `Set` メソッドを使用して値を更新し、`PropertyChanged` イベントを発生させます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-141">In the property setter, use the base class `Set` method to update the value and raise the `PropertyChanged` event.</span></span>

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

1. <span data-ttu-id="ebc51-142">読み取り専用のコマンド プロパティ `SendLocationCommand` を追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-142">Add a read-only command property called `SendLocationCommand`.</span></span> <span data-ttu-id="ebc51-143">このコマンドは `System.Windows.Input` 名前空間からの `ICommand` 型です。</span><span class="sxs-lookup"><span data-stu-id="ebc51-143">This command will have a type of `ICommand` from the `System.Windows.Input` namespace.</span></span>

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

1. <span data-ttu-id="ebc51-144">クラスにコンストラクターを追加し、`SendLocationCommand` を新しい Xamarin.Forms `Command` として初期化します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-144">Add a constructor to the class, and in this constructor, initialize the `SendLocationCommand` as a new Xamarin.Forms `Command`.</span></span> <span data-ttu-id="ebc51-145">`Xamarin.Forms` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-145">You will need to add a using directive for the `Xamarin.Forms` namespace.</span></span> <span data-ttu-id="ebc51-146">このコマンドのコンストラクターでは、コマンドが呼び出されたときに実行する `Action` を受け取るので、`SendLocation` という名前の `async` メソッドが作成され、この呼び出しを `await` するラムダ関数がコンストラクターに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-146">The constructor for this command takes an `Action` to run when the command is invoked, so create an `async` method called `SendLocation` and pass a lambda function that `await`s this call to the constructor.</span></span> <span data-ttu-id="ebc51-147">`SendLocation` メソッドの本体は、このモジュールの後のユニットで実装します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-147">The body of the `SendLocation` method will be implemented in later units in this module.</span></span> <span data-ttu-id="ebc51-148">`System.Threading.Tasks` 名前空間に対して using ディレクティブを追加し、`Task` を戻すことができるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-148">You'll need to add a using directive for the `System.Threading.Tasks` namespace to be able to return a `Task`.</span></span>

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

<span data-ttu-id="ebc51-149">このクラスのコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ebc51-149">The code for this class is shown below.</span></span>

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

## <a name="create-the-user-interface"></a><span data-ttu-id="ebc51-150">ユーザー インターフェイスを作成する</span><span class="sxs-lookup"><span data-stu-id="ebc51-150">Create the user interface</span></span>

<span data-ttu-id="ebc51-151">Xamarin.Forms UI は XAML を使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-151">Xamarin.Forms UIs can be built using XAML.</span></span>

1. <span data-ttu-id="ebc51-152">`ImHere` プロジェクトから `MainPage.xaml` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-152">Open the `MainPage.xaml` file from the `ImHere` project.</span></span> <span data-ttu-id="ebc51-153">XAML エディターでページが開きます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-153">The page will open in the XAML editor.</span></span>

    > [!NOTE]
    >  <span data-ttu-id="ebc51-154">`ImHere.UWP` プロジェクトにも `MainPage.xaml` という名前のファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-154">The `ImHere.UWP` project also contains a file called `MainPage.xaml`.</span></span> <span data-ttu-id="ebc51-155">.NET Standard ライブラリ内のものを編集していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="ebc51-155">Make sure you're editing the one in the .NET Standard library.</span></span>

1. <span data-ttu-id="ebc51-156">ViewModel 上のプロパティにコントロールをバインドするには、事前にページのバインディング コンテキストとして ViewModel のインスタンスを設定しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebc51-156">Before you can bind controls to properties on a ViewModel, you have to set an instance of the ViewModel as the binding context of the page.</span></span> <span data-ttu-id="ebc51-157">最上位レベルの `ContentPage` の中に次の XAML を追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-157">Add the following XAML inside the top-level `ContentPage`.</span></span>

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

1. <span data-ttu-id="ebc51-158">`StackLayout` を以下のコードで上書きします。</span><span class="sxs-lookup"><span data-stu-id="ebc51-158">Overwrite the `StackLayout` with the following code:</span></span>

     ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Start"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```
    - <span data-ttu-id="ebc51-159">`Editor` コントロールを使用して電話番号を追加します。上記の `Label` ではユーザーに表示するこのフィールドの目的が説明されています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-159">The `Editor` control will be used to add phone numbers, and the `Label` above describes the purpose of this field to the user.</span></span> 
    - <span data-ttu-id="ebc51-160">`StackLayout` では子コントロールが追加された順序で水平方向または垂直方向に配置されるので、`Label` を最初に追加すると `Editor` の上に配置されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-160">`StackLayout`'s stack child controls either horizontally or vertically in the order in which the controls are added, so adding the `Label` first will put it above the `Editor`.</span></span>
    - <span data-ttu-id="ebc51-161">`Editor` コントロールは複数行入力コントロールであり、ユーザーは 1 行に 1 つずつ、複数の電話番号を入力できます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-161">`Editor` controls are multi-line entry controls, allowing the user to enter multiple phone numbers, one per line.</span></span>

   

    <span data-ttu-id="ebc51-162">`Editor` の `Text` プロパティは、`MainViewModel` の `PhoneNumbers` プロパティにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-162">The `Text` property on the `Editor` is bound to the `PhoneNumbers` property on the `MainViewModel`.</span></span> <span data-ttu-id="ebc51-163">プロパティの値を `"{Binding <property name>}"` に設定するのがバインドの構文です。</span><span class="sxs-lookup"><span data-stu-id="ebc51-163">The syntax for binding is to set the property value to `"{Binding <property name>}"`.</span></span> <span data-ttu-id="ebc51-164">中かっこにより、この値は特別であり、シンプルな `string` とは異なる方法で扱う必要があることを XAML コンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-164">The curly braces will tell the XAML compiler that this value is special and should be treated differently from a simple `string`.</span></span>

1. <span data-ttu-id="ebc51-165">`Editor` コントロールの下に `Button` を追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-165">Add a `Button` below the `Editor` control.</span></span> <span data-ttu-id="ebc51-166">このボタンは、ユーザーの場所を送信する場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-166">We'll use this button to send the user's location.</span></span>

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    <span data-ttu-id="ebc51-167">`Command` プロパティは、ViewModel 上の `SendLocationCommand` コマンドにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="ebc51-167">The `Command` property is bound to the `SendLocationCommand` command on the ViewModel.</span></span> <span data-ttu-id="ebc51-168">ボタンをタップすると、コマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-168">When the button is tapped, the command will be executed.</span></span>

1. <span data-ttu-id="ebc51-169">`Button` コントロールの下に `Label` を追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-169">Add a `Label` below the `Button` control.</span></span> <span data-ttu-id="ebc51-170">このラベルには、ステータス メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-170">We'll display status messages in this label.</span></span>

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

    <span data-ttu-id="ebc51-171">このページの完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ebc51-171">The full code for this page is below.</span></span>
    
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
            <Label Text="Phone numbers to send to:" HorizontalOptions="Start"/>
            <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
            <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
                    Command="{Binding SendLocationCommand}" />
            <Label Text="{Binding Message}"
                   HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    ```

1. <span data-ttu-id="ebc51-172">アプリを実行して新しい UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-172">Run the app to see the new UI.</span></span> <span data-ttu-id="ebc51-173">この時点でバインドを検証する場合は、プロパティまたは `SendLocation` メソッドにブレークポイントを追加することによって行うことができます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-173">If you want to validate the bindings at this point, you can do so by adding breakpoints to the properties or the `SendLocation` method.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ebc51-174">このアプリをコンパイルすると、`SendLocation` に `await` 修飾子が不足していることを示す警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ebc51-174">When you compile this app, you will see a warning about `SendLocation` lacking `await` modifiers.</span></span> <span data-ttu-id="ebc51-175">この警告は、次のユニットでこのメソッドにさらにコードを追加すると解決されるので、無視して構いません。</span><span class="sxs-lookup"><span data-stu-id="ebc51-175">You can ignore this warning as this will be resolved once more code is added to this method in the next unit.</span></span>
    
    
    ![アプリの新しい UI](../media/3-new-ui.png)

## <a name="summary"></a><span data-ttu-id="ebc51-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="ebc51-177">Summary</span></span>

<span data-ttu-id="ebc51-178">このユニットでは、XAML を使用してアプリの UI を作成する方法、およびアプリケーションのロジックを処理する ViewModel を作成する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="ebc51-178">In this unit, you learned how to create the UI for the app using XAML, along with a ViewModel to handle the applications logic.</span></span> <span data-ttu-id="ebc51-179">ViewModel を UI にバインドする方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="ebc51-179">You also learned how to bind the ViewModel to the UI.</span></span> <span data-ttu-id="ebc51-180">次のユニットでは、ViewModel に場所の参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="ebc51-180">In the next unit, you add location lookup to the ViewModel.</span></span>