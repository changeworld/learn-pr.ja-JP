### <a name="exercise-4-create-a-nothotdog-app"></a>演習 4: NotHotDog アプリを作成する

この演習では、[Visual Studio Code](https://code.visualstudio.com/) を使用して、Python で NotHotDog アプリを作成します。Visual Studio Code は、Data Science VM にプレインストールされている Microsoft の無料のクロスプラットフォーム ソース コード エディターです。 このアプリでは、Python 用の人気のある GUI フレームワークである [Tkinter](https://wiki.python.org/moin/TkInter) を使用して、そのユーザー インターフェイスを実装し、ユーザーがローカル ファイル システムから画像を選択できるようにします。 その画像が前の演習でトレーニングしたモデルに渡されて、ホットドッグが含まれるかどうかが示されます。

1. デスクトップの左上隅にある **[アプリケーション]** をクリックし、**[アクセサリ] > [Visual Studio Code]** を選択して Visual Studio Code を開始します。 Visual Studio Code の **[ファイル] > [フォルダー...]** コマンドを使用して、モデルのトレーニング時に作成された **retrained_graph_hotdog.pb** ファイルが含まれる "notebooks/tensorflow-for-poets-2/tf_files" フォルダーを開きます。

1. 現在のフォルダーに **classify.py** という名前の新しいファイルを作成します。 Visual Studio Code で Python 拡張機能をインストールできる場合は、**[インストール]** をクリックしてインストールします。 次のコードをクリップボードにコピーし、**Shift + Insert** キーを使用して **classify.py** に貼り付けます。 その後、ファイルを保存します。

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

    ここで重要なコードは、```subprocess.check_output``` の呼び出しです。これにより、"scripts" フォルダーにある **label_image.py** という名前の Python スクリプトを実行することによってトレーニング済みのモデルが呼び出され、ユーザーが選択した画像が渡されます。 このスクリプトは、前の演習で複製したリポジトリから取得されたものです。

1. 好みの検索エンジンを使用して、食べ物の画像をいくつか検索します (ホットドッグが含まれているものと、含まれていないもの)。 これらの画像をダウンロードし、VM のファイル システムの適切な場所に格納します。

1. Visual Studio Code の **[表示] > [統合端末]** コマンドを使用して、統合端末を開きます。 次に、統合端末で次のコマンドを実行して、アプリを実行します。

     ```bash
     python classify.py
     ```

1. アプリの **[Select]** ボタンをクリックして、手順 3 でダウンロードしたホットドッグの画像のいずれかを選択します。 画像にホットドッグが含まれるかどうかを示すメッセージ ボックスが表示されるまで待ちます。 モデルは正しく動作しましたか。

    > 画像を処理するとカーネル ドライバーがないことを示すエラー メッセージがターミナル ウィンドウに表示される場合は、無視してかまいません。 このエラーは、Data Science VM に仮想 GPU が含まれないために発生します。

    ![画像の選択](../images/select-image.png)

    "画像の選択"__

1. ホットドッグが含まれていない画像を使用して、前の手順を繰り返します。 今度もモデルは正解しましたか。

ホットドッグを含む画像をモデルが識別できることを納得するまで、アプリに食べ物の画像を提供してください。 100% 正しい結果になることを期待してはいけませんが、"*ほとんど*" の場合は正しい結果になるはずです。