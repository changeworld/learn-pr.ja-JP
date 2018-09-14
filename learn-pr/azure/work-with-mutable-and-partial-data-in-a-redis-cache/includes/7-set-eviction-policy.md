ここでは、Azure Redis Cache を削除ポリシーを追加します。

## <a name="set-an-eviction-policy"></a>削除ポリシーを設定します。

Azure での削除ポリシーを設定するには、Azure portal で単にドロップダウン メニューを使用しましただけです。

1. Azure portal で Azure Redis Cache を開きます。

1. 選択、**詳細設定**ブレード。

1. 使用して、 **maxmemory ポリシー**ドロップダウン メニューを選択し、**ペアをランダム**します。

1. **[保存]** をクリックします。 

この時点では、メモリが不足する場合、Azure Redis Cache は、新しいデータを確保するために削除するランダム キーを選択します。