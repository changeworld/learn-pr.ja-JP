ここでは、Azure Redis Cache に削除ポリシーを追加します。

## <a name="set-an-eviction-policy"></a>削除ポリシーを設定する

Azure で削除ポリシーを設定するには、ポータルでドロップダウン メニューを使用します。

1. Azure portal で Redis Cache を開きます。

1. **[詳細設定]** ブレードを選択します。

1. **[maxmemory-policy]** ドロップダウン メニューを使用し、**[allkeys-random]** を選択します。

1. **[保存]** をクリックします。 

この時点で、メモリ不足になると、Redis は新しいデータのための領域を確保するために、ランダム キーを選択して削除します。