グローバルな製薬会社で、ネットワーク マネージャーとして働いているとします。この会社では、遺伝子の配列を決定して、独自の商業上部外秘である薬品の化学式を作成しています。 これらの化学式は、世界各地の製造工場で使用されます。 クラウド ベースの仮想マシンにその gene シーケンス処理関数の Microsoft Azure と転送の一部を実装するために考えています。 Gene シーケンス処理の結果は、世界各地の複数のリージョンに使用する必要があります。 データ処理は、仮想プライベート ネットワーク (VPN) 各リージョン内のデータ センターを接続するために使用すると、オンプレミスのデータ センターで現在に行われます。

Microsoft Azure ネットワークを実装する方法を評価して、データの転送に関する適切なセキュリティが、かどうかを識別するためにする仕事しました。 セキュリティで保護されたデータ転送が、オンプレミス データ センターと Microsoft Azure の間、および Microsoft Azure リージョン間で必要です。 Azure Virtual Network、Azure VPN Gateway、および Azure ExpressRoute テクノロジを使用します。

## <a name="learning-objectives"></a>学習の目的

このモジュールを完了すると、次のことができるようになります。

- Azure Virtual Network について説明する
- Azure VPN Gateway について説明する
- Azure ExpressRoute について説明する

## <a name="prerequisites"></a>前提条件

- Azure 仮想マシン作成の経験
- オンプレミス ネットワークの十分な理解
- Azure PowerShell
