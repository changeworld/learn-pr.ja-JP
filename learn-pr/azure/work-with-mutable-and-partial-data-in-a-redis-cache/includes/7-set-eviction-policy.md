<span data-ttu-id="e972c-101">ここでは、Azure Redis Cache に削除ポリシーを追加します。</span><span class="sxs-lookup"><span data-stu-id="e972c-101">Here you'll add an eviction policy to our Azure Redis Cache.</span></span>

## <a name="set-an-eviction-policy"></a><span data-ttu-id="e972c-102">削除ポリシーを設定する</span><span class="sxs-lookup"><span data-stu-id="e972c-102">Set an eviction policy</span></span>

<span data-ttu-id="e972c-103">Azure で削除ポリシーを設定するには、ポータルでドロップダウン メニューを使用します。</span><span class="sxs-lookup"><span data-stu-id="e972c-103">To set an eviction policy in Azure, we simply use a drop-down menu in the portal.</span></span>

1. <span data-ttu-id="e972c-104">Azure portal で Redis Cache を開きます。</span><span class="sxs-lookup"><span data-stu-id="e972c-104">Open your Redis Cache in the Azure portal.</span></span>

1. <span data-ttu-id="e972c-105">**[詳細設定]** ブレードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e972c-105">Select the **Advanced settings** blade.</span></span>

1. <span data-ttu-id="e972c-106">**[maxmemory-policy]** ドロップダウン メニューを使用し、**[allkeys-random]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e972c-106">Use the **maxmemory-policy** drop-down menu and select **allkeys-random**.</span></span>

1. <span data-ttu-id="e972c-107">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e972c-107">Click **Save**.</span></span> 

<span data-ttu-id="e972c-108">この時点で、メモリ不足になると、Redis は新しいデータのための領域を確保するために、ランダム キーを選択して削除します。</span><span class="sxs-lookup"><span data-stu-id="e972c-108">At this point, if you run out of memory, Redis will select a random key to delete to make room for your new data.</span></span>