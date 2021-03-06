複数のリージョンでデータをレプリケートした後は、Azure Cosmos DB で提供されている自動フェールオーバー ソリューションを利用できます。 自動フェールオーバーは、いずれかの読み取りまたは書き込みリージョンがオフラインになる障害またはその他のイベントが発生すると有効になる機能です。これにより、オフラインのリージョンから第 2 優先のリージョンへ要求がリダイレクトされます。 

たとえば、米国東部、米国西部、および英国西部にのみレプリケートされているオンライン衣料品サイトでは、米国東部がオフラインになった場合に、遅延を抑えるために読み取りを英国西部ではなく米国西部にリダイレクトできるよう、優先順位を設定することができます。 

このユニットでは、フェールオーバーの仕組みと、会社のデータがレプリケートされているリージョンの優先順位を設定する方法について学習します。

## <a name="failover-basics"></a>フェールオーバーの基本

Azure のリージョン内障害やデータセンターの停止はめったに発生しませんが、発生した場合、Azure Cosmos DB により、影響を受けるリージョンに存在するすべての Azure Cosmos DB アカウントのフェールオーバーが自動的にトリガーされます。

**読み取りリージョンで障害が起きた場合**

影響を受けるリージョンのいずれかに読み取りリージョンを指定している Azure Cosmos DB アカウントは、自動的にそれらの書き込みリージョンから切断され、オフラインとしてマークされます。 Cosmos DB SDK ではリージョン内探索プロトコルが実装されており、リージョンが使用可能になったことを自動的に検出し、優先リージョンの一覧に従って次の使用可能なリージョンに読み込み呼び出しをリダイレクトすることができます。 優先リージョンの一覧にあるいずれのリージョンも使用可能でない場合、呼び出しは自動的に現在の書き込みリージョンに戻ります。 リージョン内フェールオーバーを処理するアプリケーション コードは、変更する必要はありません。 この処理全体において、Azure Cosmos DB によって一貫性の保証が継続的に実現されています。

影響を受けたリージョンが障害から復旧したら、そのリージョン内で影響を受けたすべての Azure Cosmos DB アカウントは、サービスにより自動的に復旧されます。 そして、影響を受けたリージョンに読み取りリージョンがあった Azure Cosmos DB アカウントは、自動的に現在の書き込みリージョンと同期してオンラインになります。 Azure Cosmos DB SDK では、新しいリージョンが使用可能かを検出し、そのリージョンを現在の読み取りリージョンとして選択すべきかどうか、アプリケーションで構成された優先リージョンの一覧に基づいて判断します。 それ以降の読み取りは、アプリケーション コードを変更しなくても、復旧したリージョンにリダイレクトされます。

**書き込みリージョンで障害が起きた場合**

影響を受けたリージョンが現在の書き込みリージョンであり、Azure Cosmos DB アカウントの自動フェールオーバーが有効になっている場合、リージョンは自動的にオフラインとしてマークされます。 そして、影響を受けた Azure Cosmos DB アカウントでは、代替リージョンが書き込みリージョンとして昇格されます。

自動フェールオーバー中、Azure Cosmos DB では、指定された優先順位に基づいて、指定の Azure Cosmos DB アカウント用の次の書き込みリージョンが自動的に選択されます。 アプリケーションでは、DocumentClient クラスの WriteEndpoint プロパティを使用して、書き込みリージョン内の変更を検出できます。

影響を受けたリージョンが障害から復旧したら、そのリージョン内で影響を受けたすべての Cosmos DB アカウントは、サービスにより自動的に復旧されます。

では、データベースの読み取りリージョンを変更してみましょう。

## <a name="set-read-region-priorities"></a>読み取りリージョンの優先順位を設定する

1. Azure portal の **[データをグローバルにレプリケートする]** 画面で、**[自動フェールオーバー]** をクリックします。 自動フェールオーバーは、データベースが複数のリージョンに既にレプリケートされている場合にのみ有効になります。
2. **[自動フェールオーバー]** 画面で、**[自動フェールオーバーの有効化]** を **[ON]** にします。
3. **[読み取りリージョン]** セクションで、**[米国東部]** 行の左側の部分をクリックし、先頭位置までドラッグ アンド ドロップします。
4. **[日本東部]** 行の左側の部分をクリックし、2 番目の位置までドラッグ アンド ドロップします。
5. **[OK]** をクリックします。

    ![ポータルで読み取りリージョンを変更する](../media/4-change-priorities/change-read-priorities.gif)

## <a name="summary"></a>まとめ

このユニットでは、自動フェールオーバーによって提供される内容、予想外の障害から保護するための使用方法、および読み取りリージョンの優先順位を変更する方法を学習しました。