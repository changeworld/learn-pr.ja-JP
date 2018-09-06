### <a name="exercise-4-create-a-nothotdog-app"></a><span data-ttu-id="2ac10-101">演習 4: NotHotDog アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="2ac10-101">Exercise 4: Create a NotHotDog app</span></span>

<span data-ttu-id="2ac10-102">この演習では、[Visual Studio Code](https://code.visualstudio.com/) を使用して、Python で NotHotDog アプリを作成します。Visual Studio Code は、Data Science VM にプレインストールされている Microsoft の無料のクロスプラットフォーム ソース コード エディターです。</span><span class="sxs-lookup"><span data-stu-id="2ac10-102">In this exercise, you will use [Visual Studio Code](https://code.visualstudio.com/), Microsoft's free, cross-platform source-code editor which is preinstalled in the Data Science VM, to write a NotHotDog app in Python.</span></span> <span data-ttu-id="2ac10-103">このアプリでは、Python 用の人気のある GUI フレームワークである [Tkinter](https://wiki.python.org/moin/TkInter) を使用して、そのユーザー インターフェイスを実装し、ユーザーがローカル ファイル システムから画像を選択できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ac10-103">The app will use [Tkinter](https://wiki.python.org/moin/TkInter), which is a popular GUI framework for Python, to implement its user interface, and it will allow you to select images from your local file system.</span></span> <span data-ttu-id="2ac10-104">その画像が前の演習でトレーニングしたモデルに渡されて、ホットドッグが含まれるかどうかが示されます。</span><span class="sxs-lookup"><span data-stu-id="2ac10-104">Then it will pass those images to the model you trained in the previous exercise and tell you whether they contain a hot dog.</span></span>

1. <span data-ttu-id="2ac10-105">デスクトップの左上隅にある **[アプリケーション]** をクリックし、**[アクセサリ] > [Visual Studio Code]** を選択して Visual Studio Code を開始します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-105">Click **Applications** in the upper-left corner of the desktop and select **Accessories > Visual Studio Code** to start Visual Studio Code.</span></span> <span data-ttu-id="2ac10-106">Visual Studio Code の **[ファイル] > [フォルダー...]** コマンドを使用して、モデルのトレーニング時に作成された **retrained_graph_hotdog.pb** ファイルが含まれる "notebooks/tensorflow-for-poets-2/tf_files" フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="2ac10-106">Use Visual Studio Code's **File > Open Folder...** command to open the "notebooks/tensorflow-for-poets-2/tf_files" folder containing the **retrained_graph_hotdog.pb** file created when you trained the model.</span></span>

1. <span data-ttu-id="2ac10-107">現在のフォルダーに **classify.py** という名前の新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-107">Create a new file named **classify.py** in the current folder.</span></span> <span data-ttu-id="2ac10-108">Visual Studio Code で Python 拡張機能をインストールできる場合は、**[インストール]** をクリックしてインストールします。</span><span class="sxs-lookup"><span data-stu-id="2ac10-108">If Visual Studio Code offers to install the Python extension, click **Install** to install it.</span></span> <span data-ttu-id="2ac10-109">次のコードをクリップボードにコピーし、**Shift + Insert** キーを使用して **classify.py** に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="2ac10-109">Copy the code below to the clipboard and use **Shift+Ins** to paste it into **classify.py**.</span></span> <span data-ttu-id="2ac10-110">その後、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-110">Then save the file:</span></span>

    ```python
    import tkinter as tk
    from tkinter import messagebox, filedialog, font
    from PIL import ImageTk, Image
    import subprocess

    def select_image_click(img_label):
        try:
            file = filedialog.askopenfilename()

            img = Image.open(file)
            img = img.resize((300, 300))
            selected_img = ImageTk.PhotoImage(img)

            img_label.configure(image=selected_img, width=240)

            output = subprocess.check_output(["python",
                "../scripts/label_image.py",
                "--graph=retrained_graph_hotdog.pb",
                "--image={0}".format(file),
                "--labels=retrained_labels_hotdog.txt"])

            highest = str(output).split("\\n")[3].split(" ")

            if len(highest) == 3:
                score = float(highest[2])
                is_hotdog = True
            else:
                score = float(highest[3])
                is_hotdog = False

            if score > 0.95:
                if is_hotdog:
                    messagebox.showinfo("Result", "That's a hot dog!")
                else:
                    messagebox.showinfo("Result", "That's not a hot dog.")
            else:
                messagebox.showinfo("Result", "Can't tell.")

        except FileNotFoundError as e:
            messagebox.showerror("File not found", "File {0} was not found.".format(e.filename))

    def run():
        window = tk.Tk()

        window.title("Hotdog or Not Hotdog")
        window.geometry('400x600')

        text_font = font.Font(size=18, family="Helvetica Neue")
        welcome_text = tk.Label(window, text="Hot Dog or Not Hot Dog", font=text_font)
        welcome_text.pack()

        instructions_text = tk.Label(window, text="\n\nUse a neural network built with Tensorflow\n"
            "to identify photos containing hot dogs")
        instructions_text.pack(fill=tk.X)

        select_btn = tk.Button(window, text="Select", bg="#0063B1", fg="white", width=5, height=1)
        select_btn.pack(pady=30)

        image_label = tk.Label(window)
        image_label.pack()

        select_btn.configure(command=lambda: select_image_click(image_label))
        window.mainloop()

    if __name__ == "__main__":
        run()
    ```

    <span data-ttu-id="2ac10-111">ここで重要なコードは、```subprocess.check_output``` の呼び出しです。これにより、"scripts" フォルダーにある **label_image.py** という名前の Python スクリプトを実行することによってトレーニング済みのモデルが呼び出され、ユーザーが選択した画像が渡されます。</span><span class="sxs-lookup"><span data-stu-id="2ac10-111">They key code here is the call to ```subprocess.check_output```, which invokes the trained model by executing a Python script named **label_image.py** found in the "scripts" folder, passing in the image that the user selected.</span></span> <span data-ttu-id="2ac10-112">このスクリプトは、前の演習で複製したリポジトリから取得されたものです。</span><span class="sxs-lookup"><span data-stu-id="2ac10-112">This script came from the repo that you cloned in the previous exercise.</span></span>

1. <span data-ttu-id="2ac10-113">好みの検索エンジンを使用して、食べ物の画像をいくつか検索します (ホットドッグが含まれているものと、含まれていないもの)。</span><span class="sxs-lookup"><span data-stu-id="2ac10-113">Use your favorite search engine to find a few food images — some containing hot dogs, and some not.</span></span> <span data-ttu-id="2ac10-114">これらの画像をダウンロードし、VM のファイル システムの適切な場所に格納します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-114">Download these images and store them in the location of your choice in the VM's file system.</span></span>

1. <span data-ttu-id="2ac10-115">Visual Studio Code の **[表示] > [統合端末]** コマンドを使用して、統合端末を開きます。</span><span class="sxs-lookup"><span data-stu-id="2ac10-115">Use Visual Studio Code's **View > Integrated Terminal** command to open an integrated terminal.</span></span> <span data-ttu-id="2ac10-116">次に、統合端末で次のコマンドを実行して、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-116">Then execute the following command in the integrated terminal to run the app:</span></span>

     ```bash
     python classify.py
     ```

1. <span data-ttu-id="2ac10-117">アプリの **[Select]** ボタンをクリックして、手順 3 でダウンロードしたホットドッグの画像のいずれかを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-117">Click the app's **Select** button and pick one of the hot-dog images you downloaded in Step 3.</span></span> <span data-ttu-id="2ac10-118">画像にホットドッグが含まれるかどうかを示すメッセージ ボックスが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="2ac10-118">Wait for a message box to appear, indicating whether the image contains a hot dog.</span></span> <span data-ttu-id="2ac10-119">モデルは正しく動作しましたか。</span><span class="sxs-lookup"><span data-stu-id="2ac10-119">Did the model get it correct?</span></span>

    > <span data-ttu-id="2ac10-120">画像を処理するとカーネル ドライバーがないことを示すエラー メッセージがターミナル ウィンドウに表示される場合は、無視してかまいません。</span><span class="sxs-lookup"><span data-stu-id="2ac10-120">If you see error messages regarding a missing kernel driver in the terminal window when you process an image, you can safely ignore them.</span></span> <span data-ttu-id="2ac10-121">このエラーは、Data Science VM に仮想 GPU が含まれないために発生します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-121">They result from the fact that the Data Science VM does not contain a virtual GPU.</span></span>

    ![画像の選択](../images/select-image.png)

    <span data-ttu-id="2ac10-123">"画像の選択"__</span><span class="sxs-lookup"><span data-stu-id="2ac10-123">_Selecting an image_</span></span>

1. <span data-ttu-id="2ac10-124">ホットドッグが含まれていない画像を使用して、前の手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="2ac10-124">Repeat the previous step using an image that doesn't contain a hot dog.</span></span> <span data-ttu-id="2ac10-125">今度もモデルは正解しましたか。</span><span class="sxs-lookup"><span data-stu-id="2ac10-125">Was the model right this time?</span></span>

<span data-ttu-id="2ac10-126">ホットドッグを含む画像をモデルが識別できることを納得するまで、アプリに食べ物の画像を提供してください。</span><span class="sxs-lookup"><span data-stu-id="2ac10-126">Continue feeding food images into the app until you're satisfied that it can identify images containing hot dogs.</span></span> <span data-ttu-id="2ac10-127">100% 正しい結果になることを期待してはいけませんが、"*ほとんど*" の場合は正しい結果になるはずです。</span><span class="sxs-lookup"><span data-stu-id="2ac10-127">Don't expect it to be right 100% of the time, but do expect it to be right *most* of the time.</span></span>