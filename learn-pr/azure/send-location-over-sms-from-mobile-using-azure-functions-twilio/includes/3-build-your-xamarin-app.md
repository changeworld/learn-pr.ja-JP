<span data-ttu-id="77000-101">この時点で、モバイル アプリはシンプルな "Hello World" アプリです。</span><span class="sxs-lookup"><span data-stu-id="77000-101">At this point, the mobile app is a simple "Hello World" app.</span></span> <span data-ttu-id="77000-102">このユニットでは、UI といくつかの基本的なアプリケーション ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-102">In this unit, you add the UI and some basic application logic.</span></span>

<span data-ttu-id="77000-103">アプリの UI は次のもので構成されています。</span><span class="sxs-lookup"><span data-stu-id="77000-103">The UI for the app will consist of:</span></span>

* <span data-ttu-id="77000-104">電話番号を入力するためのテキスト入力コントロール。</span><span class="sxs-lookup"><span data-stu-id="77000-104">A text-entry control to enter some phone numbers.</span></span>
* <span data-ttu-id="77000-105">Azure 関数を使用してこれらの番号に場所を送信するボタン。</span><span class="sxs-lookup"><span data-stu-id="77000-105">A button to send your location to those numbers using an Azure function.</span></span>
* <span data-ttu-id="77000-106">現在の状態 (送信されている場所、正常に送信された場所など) についてのメッセージをユーザーに表示するラベル。</span><span class="sxs-lookup"><span data-stu-id="77000-106">A label that will show a message to the user of the current status, such as the location being sent and location sent successfully.</span></span>

<span data-ttu-id="77000-107">Xamarin.Forms では、Model-View-ViewModel (MVVM) と呼ばれる設計パターンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="77000-107">Xamarin.Forms supports a design pattern called Model-View-ViewModel (MVVM).</span></span> <span data-ttu-id="77000-108">MVVM について詳しくは、[Xamarin MVVM ドキュメント](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm)をご覧ください。MVVM の最も重要な点は、各ページ (View) がプロパティと動作を公開する ViewModel を持っていることです。</span><span class="sxs-lookup"><span data-stu-id="77000-108">You can read more about MVVM in the [Xamarin MVVM docs](https://docs.microsoft.com/xamarin/xamarin-forms/enterprise-application-patterns/mvvm), but the essence of it is, each page (View) has a ViewModel that exposes properties and behavior.</span></span>

<span data-ttu-id="77000-109">ViewModel のプロパティは名前によって UI に "バインド" されており、このバインドによって View と ViewModel の間でデータが同期されます。</span><span class="sxs-lookup"><span data-stu-id="77000-109">ViewModel properties are 'bound' to components on the UI by name, and this binding synchronizes data between the View and ViewModel.</span></span> <span data-ttu-id="77000-110">たとえば、ViewModel の `Name` という名前の `string` プロパティを、UI のテキスト入力コントロールの `Text` プロパティにバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="77000-110">For example, a `string` property on a ViewModel called `Name` could be bound to the `Text` property of a text-entry control on the UI.</span></span> <span data-ttu-id="77000-111">テキスト入力コントロールには `Name` プロパティの値が表示され、ユーザーが UI のテキストを変更すると、`Name` プロパティが更新されます。</span><span class="sxs-lookup"><span data-stu-id="77000-111">The text-entry control shows the value in the `Name` property and, when the user changes the text in the UI, the `Name` property is updated.</span></span> <span data-ttu-id="77000-112">ViewModel で `Name` プロパティの値が変化すると、UI を更新するためのイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="77000-112">If the value of the `Name` property is changed in the ViewModel, an event is raised to tell the UI to update.</span></span>

<span data-ttu-id="77000-113">ViewModel の動作はコマンドのプロパティとして公開されます。コマンドは、コマンドが呼び出されたときに実行されるアクションをラップするオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="77000-113">ViewModel behavior is exposed as command properties, a command being an object that wraps an action that is executed when the command is invoked.</span></span> <span data-ttu-id="77000-114">これらのコマンドはボタンなどのコントロールに名前によってバインドされ、ボタンをタップするとコマンドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="77000-114">These commands are bound by name to controls like buttons, and tapping a button will invoke the command.</span></span>

## <a name="create-a-base-viewmodel"></a><span data-ttu-id="77000-115">基底 ViewModel を作成する</span><span class="sxs-lookup"><span data-stu-id="77000-115">Create a base ViewModel</span></span>

<span data-ttu-id="77000-116">すべての ViewModel で `INotifyPropertyChanged` インターフェイスが実装されています。</span><span class="sxs-lookup"><span data-stu-id="77000-116">ViewModels all implement the `INotifyPropertyChanged` interface.</span></span> <span data-ttu-id="77000-117">このインターフェイスの単一のイベント `PropertyChanged` は、UI に更新を通知するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="77000-117">This interface has a single event, `PropertyChanged`, which is used to notify the UI of any updates.</span></span> <span data-ttu-id="77000-118">このイベントには、変更されたプロパティの名前を含むイベント引数があります。</span><span class="sxs-lookup"><span data-stu-id="77000-118">This event has event args that contain the name of the property that has changed.</span></span> <span data-ttu-id="77000-119">このインターフェイスを実装していくつかのヘルパー メソッドを提供する基底 ViewModel クラスを作成するのが一般的な方法です。</span><span class="sxs-lookup"><span data-stu-id="77000-119">It's common practice to create a base ViewModel class implementing this interface and providing some helper methods.</span></span>

1. <span data-ttu-id="77000-120">プロジェクトを右クリックし、*[追加] > [クラス]* の順に選択して、`BaseViewModel` という名前の `ImHere` .NET 標準プロジェクトに新しいクラスを作成します。新しいクラスの名前を "BaseViewModel" にして、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="77000-120">Create a new class in the `ImHere` .NET standard project called `BaseViewModel` by right-clicking on the project, and then selecting *Add->Class...*. Name the new class "BaseViewModel" and click **Add**.</span></span>

2. <span data-ttu-id="77000-121">クラスを `public` にして、`INotifyPropertyChanged` から派生します。</span><span class="sxs-lookup"><span data-stu-id="77000-121">Make the class `public` and derive from `INotifyPropertyChanged`.</span></span> <span data-ttu-id="77000-122">`System.ComponentModel` に using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="77000-122">You'll need to add a using directive for `System.ComponentModel`.</span></span>

3. <span data-ttu-id="77000-123">`PropertyChanged` イベントを追加することで `INotifyPropertyChanged` インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="77000-123">Implement the `INotifyPropertyChanged` interface by adding the `PropertyChanged` event:</span></span>

    ```cs
    public event PropertyChangedEventHandler PropertyChanged;
    ```

4. <span data-ttu-id="77000-124">ViewModel のプロパティの一般的なパターンでは、プライベート バッキング フィールドを持つパブリック プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="77000-124">The common pattern for ViewModel properties is to have a public property with a private backing field.</span></span> <span data-ttu-id="77000-125">プロパティ セッターで、新しい値に対してバッキング フィールドが確認されます。</span><span class="sxs-lookup"><span data-stu-id="77000-125">In the property setter, the backing field is checked against the new value.</span></span> <span data-ttu-id="77000-126">新しい値がバッキング フィールドと異なる場合、バッキング フィールドが更新されて、`PropertyChanged` イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="77000-126">If the new value is different to the backing field, the backing field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="77000-127">このロジックを簡単に抽出して `Set` メソッドとして追加できます。</span><span class="sxs-lookup"><span data-stu-id="77000-127">This logic is easy to factor out into a method, so add the `Set` method.</span></span> <span data-ttu-id="77000-128">`System.Runtime.CompilerServices` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="77000-128">You'll need to add a using directive for the `System.Runtime.CompilerServices` namespace.</span></span>

    ```cs
    protected void Set<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return;
        field = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    ```

    <span data-ttu-id="77000-129">このメソッドは、バッキング フィールド、新しい値、プロパティ名への参照を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="77000-129">This method takes a reference to the backing field, the new value, and the property name.</span></span> <span data-ttu-id="77000-130">フィールドが変更されていない場合、メソッドは戻ります。変更されている場合は、フィールドが更新されて `PropertyChanged` イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="77000-130">If the field hasn't changed, the method returns, otherwise, the field is updated and the `PropertyChanged` event is raised.</span></span> <span data-ttu-id="77000-131">`Set` メソッドの `propertyName` パラメーターは既定のパラメーターであり、`CallerMemberName` 属性でマークされています。</span><span class="sxs-lookup"><span data-stu-id="77000-131">The `propertyName` parameter on the `Set` method is a default parameter and is marked with the `CallerMemberName` attribute.</span></span> <span data-ttu-id="77000-132">このメソッドがプロパティ セッターから呼び出されるとき、このパラメーターは通常は既定値のままにされます。</span><span class="sxs-lookup"><span data-stu-id="77000-132">When this method is called from a property setter, this parameter is normally left as the default value.</span></span> <span data-ttu-id="77000-133">コンパイラによって、呼び出し元プロパティの名前がパラメーターの値に自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="77000-133">The compiler will then automatically set the parameter value to be the name of the calling property.</span></span>

<span data-ttu-id="77000-134">このクラスの完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="77000-134">The full code for this class is below.</span></span>

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

## <a name="create-a-viewmodel-for-the-page"></a><span data-ttu-id="77000-135">ページの ViewModel を作成する</span><span class="sxs-lookup"><span data-stu-id="77000-135">Create a ViewModel for the page</span></span>

<span data-ttu-id="77000-136">`MainPage` には、電話番号のためのテキスト入力コントロールとメッセージ表示用のラベルがあります。</span><span class="sxs-lookup"><span data-stu-id="77000-136">The `MainPage` will have a text-entry control for phone numbers and a label to display a message.</span></span> <span data-ttu-id="77000-137">これらのコントロールは、ViewModel のプロパティにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="77000-137">These controls will be bound to properties on a ViewModel.</span></span>

1. <span data-ttu-id="77000-138">`ImHere` .NET 標準プロジェクトに `MainViewModel` というクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="77000-138">Create a class called `MainViewModel` in the `ImHere` .NET standard project.</span></span>

2. <span data-ttu-id="77000-139">このクラスを public にして、`BaseViewModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="77000-139">Make this class public and derive from `BaseViewModel`.</span></span>

3. <span data-ttu-id="77000-140">それぞれバッキング フィールドを備えた 2 つの `string` プロパティ `PhoneNumbers` と `Message` を追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-140">Add two `string` properties, `PhoneNumbers` and `Message`, each with a backing field.</span></span> <span data-ttu-id="77000-141">プロパティ セッターで、基底クラスの `Set` メソッドを使用して値を更新し、`PropertyChanged` イベントを発生させます。</span><span class="sxs-lookup"><span data-stu-id="77000-141">In the property setter, use the base class `Set` method to update the value and raise the `PropertyChanged` event.</span></span>

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

4. <span data-ttu-id="77000-142">読み取り専用のコマンド プロパティ `SendLocationCommand` を追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-142">Add a read-only command property called `SendLocationCommand`.</span></span> <span data-ttu-id="77000-143">このコマンドは `System.Windows.Input` 名前空間の `ICommand` 型です。</span><span class="sxs-lookup"><span data-stu-id="77000-143">This command will have a type of `ICommand` from the `System.Windows.Input` namespace.</span></span>

    ```cs
    public ICommand SendLocationCommand { get; }
    ```

5. <span data-ttu-id="77000-144">クラスにコンストラクターを追加し、このコンストラクターで `SendLocationCommand` を `Xamarin.Forms` 名前空間の新しい `Command` として初期化します。</span><span class="sxs-lookup"><span data-stu-id="77000-144">Add a constructor to the class, and in this constructor, initialize the `SendLocationCommand` as a new `Command` from the `Xamarin.Forms` namespace.</span></span> <span data-ttu-id="77000-145">このコマンドのコンストラクターはコマンドが呼び出されたときに実行する `Action` を受け取るので、`SendLocation` という名前の `async` メソッドを作成し、この呼び出しを `await` するラムダ関数をコンストラクターに渡します。</span><span class="sxs-lookup"><span data-stu-id="77000-145">The constructor for this command takes an `Action` to run when the command is invoked, so create an `async` method called `SendLocation` and pass a lambda function that `await`s this call to the constructor.</span></span> <span data-ttu-id="77000-146">`SendLocation` メソッドの本体は、このモジュールの後のユニットで実装します。</span><span class="sxs-lookup"><span data-stu-id="77000-146">The body of the `SendLocation` method will be implemented in later units in this module.</span></span> <span data-ttu-id="77000-147">`System.Threading.Tasks` 名前空間に対して using ディレクティブを追加し、`Task` を戻すことができるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="77000-147">You'll need to add a using directive for the `System.Threading.Tasks` namespace to be able to return a `Task`.</span></span>

    ```cs
    public MainViewModel()
    {
        SendLocationCommand = new Command(async () => await SendLocation());
    }

    async Task SendLocation()
    {
    }
    ```

<span data-ttu-id="77000-148">このクラスのコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="77000-148">The code for this class is shown below.</span></span>

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

## <a name="create-the-user-interface"></a><span data-ttu-id="77000-149">ユーザー インターフェイスを作成する</span><span class="sxs-lookup"><span data-stu-id="77000-149">Create the user interface</span></span>

<span data-ttu-id="77000-150">Xamarin.Forms UI は XAML を使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="77000-150">Xamarin.Forms UIs can be built using XAML.</span></span>

1. <span data-ttu-id="77000-151">`ImHere` プロジェクトから `MainPage.xaml` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="77000-151">Open the `MainPage.xaml` file from the `ImHere` project.</span></span> <span data-ttu-id="77000-152">XAML エディターでページが開きます。</span><span class="sxs-lookup"><span data-stu-id="77000-152">The page will open in the XAML editor.</span></span>

    <span data-ttu-id="77000-153">注 - `ImHere.UWP` プロジェクトにも `MainPage.xaml` という名前のファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="77000-153">NOTE - The `ImHere.UWP` project also contains a file called `MainPage.xaml`.</span></span> <span data-ttu-id="77000-154">.NET 標準ライブラリのものを編集していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="77000-154">Make sure you're editing the one in the .NET standard library.</span></span>

2. <span data-ttu-id="77000-155">ViewModel のプロパティにコントロールをバインドする前に、ページのバインディング コンテキストとして ViewModel のインスタンスを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="77000-155">Before you can bind controls to properties on a ViewModel, you have to set an instance of the ViewModel as the binding context of the page.</span></span> <span data-ttu-id="77000-156">最上位レベルの `ContentPage` の中に次の XAML を追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-156">Add the following XAML inside the top-level `ContentPage`.</span></span>

    ```xml
    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    ```

3. <span data-ttu-id="77000-157">`StackLayout` の内容を削除し、UI の見た目がよくなるように何かパディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-157">Delete the contents of the `StackLayout` and add some padding inside it to help make the UI look better.</span></span>

    ```xml
    <StackLayout Padding="20">
    </StackLayout>
    ```

4. <span data-ttu-id="77000-158">ユーザーが `StackLayout` に電話番号を追加するのに使用できる `Editor` コントロールを追加します。上の `Label` では、入力コントロールの用途を説明します。</span><span class="sxs-lookup"><span data-stu-id="77000-158">Add an `Editor` control that the user can use to add phone numbers to the `StackLayout`, with a `Label` above to describe what the entry control is for.</span></span> <span data-ttu-id="77000-159">`StackLayout` では子コントロールが追加された順序で水平方向または垂直方向に配置されるので、`Label` を最初に追加すると `Editor` の上に配置されます。</span><span class="sxs-lookup"><span data-stu-id="77000-159">`StackLayout`'s stack child controls either horizontally or vertically in the order in which the controls are added, so adding the `Label` first will put it above the `Editor`.</span></span> <span data-ttu-id="77000-160">`Editor` コントロールは複数行入力コントロールであり、ユーザーは 1 行に 1 つずつ、複数の電話番号を入力できます。</span><span class="sxs-lookup"><span data-stu-id="77000-160">`Editor` controls are multi-line entry controls, allowing the user to enter multiple phone numbers, one per line.</span></span>

    ```xml
    <StackLayout Padding="20">
        <Label Text="Phone numbers to send to:" HorizontalOptions="Center"/>
        <Editor Text="{Binding PhoneNumbers}" HeightRequest="100"/>
    </StackLayout>
    ```

    <span data-ttu-id="77000-161">`Editor` の `Text` プロパティは、`MainViewModel` の `PhoneNumbers` プロパティにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="77000-161">The `Text` property on the `Editor` is bound to the `PhoneNumbers` property on the `MainViewModel`.</span></span> <span data-ttu-id="77000-162">プロパティの値を `"{Binding <property name>}"` に設定するのがバインドの構文です。</span><span class="sxs-lookup"><span data-stu-id="77000-162">The syntax for binding is to set the property value to `"{Binding <property name>}"`.</span></span> <span data-ttu-id="77000-163">中かっこにより、この値は特別であり、シンプルな `string` とは異なる方法で扱う必要があることを XAML コンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="77000-163">The curly braces will tell the XAML compiler that this value is special and should be treated differently from a simple `string`.</span></span>

5. <span data-ttu-id="77000-164">ユーザーの場所を送信するための `Button` を、`Editor` の下に追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-164">Add a `Button` to send the user's location below the `Editor`.</span></span>

    ```xml
    <Button Text="Send Location" BackgroundColor="Blue" TextColor="White"
            Command="{Binding SendLocationCommand}" />
    ```

    <span data-ttu-id="77000-165">`Command` プロパティは、ViewModel の `SendLocationCommand` コマンドにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="77000-165">The `Command` property is bound to the `SendLocationCommand` command on the ViewModel.</span></span> <span data-ttu-id="77000-166">ボタンをタップすると、コマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="77000-166">When the button is tapped, the command will be executed.</span></span>

6. <span data-ttu-id="77000-167">ステータス メッセージを表示する `Label` を、`Button` の下に追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-167">Add a `Label` to show the status message below the `Button`.</span></span>

    ```xml
    <Label Text="{Binding Message}"
           HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
    ```

<span data-ttu-id="77000-168">このページの完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="77000-168">The full code for this page is below.</span></span>

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

<span data-ttu-id="77000-169">アプリを実行して新しい UI を表示します。</span><span class="sxs-lookup"><span data-stu-id="77000-169">Run the app to see the new UI.</span></span> <span data-ttu-id="77000-170">この時点でバインドを検証する場合は、プロパティまたは `SendLocation` メソッドにブレークポイントを追加することによって行うことができます。</span><span class="sxs-lookup"><span data-stu-id="77000-170">If you want to validate the bindings at this point, you can do so by adding breakpoints to the properties or the `SendLocation` method.</span></span>

![アプリの新しい UI](../media-drafts/3-new-ui.png)

## <a name="summary"></a><span data-ttu-id="77000-172">まとめ</span><span class="sxs-lookup"><span data-stu-id="77000-172">Summary</span></span>

<span data-ttu-id="77000-173">このユニットでは、XAML を使用してアプリの UI を作成する方法、およびアプリケーションのロジックを処理する ViewModel を作成する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="77000-173">In this unit, you learned how to create the UI for the app using XAML, along with a ViewModel to handle the applications logic.</span></span> <span data-ttu-id="77000-174">ViewModel を UI にバインドする方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="77000-174">You also learned how to bind the ViewModel to the UI.</span></span> <span data-ttu-id="77000-175">次のユニットでは、ViewModel に場所の参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="77000-175">In the next unit, you add location lookup to the ViewModel.</span></span>