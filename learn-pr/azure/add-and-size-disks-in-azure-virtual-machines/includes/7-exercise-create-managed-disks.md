この短い演習では、管理されていない既存の VHD を管理対象 VHD を移行する方法説明します。 

## <a name="sign-in-to-azure"></a>Azure へのサインイン

1. [Azure portal](https://portal.azure.com/?azure-portal=true) にサインインします。

## <a name="migrate-our-disks-to-managed-disks"></a>ディスクを managed disks に移行します。

1. 左側のナビゲーションで、Azure portal で次のように選択します。**仮想マシン**します。

1. 仮想マシンの一覧で、仮想マシンを選択します。 **MailSenderVM**します。

1. **MailSenderVM**  ウィンドウで、**設定**を選択します**ディスク**します。

1. **MailSenderVM - ディスク**] ページで、[ **managed disks に移行**します。

1. **Managed disks に移行**] ページで、[**移行**します。 Azure VM を停止するには、ディスクを移行し、VM を再起動します。 このプロセスには数分かかることがあります。

この演習では管理ディスクにディスクを移行したとします。 管理ディスクを使用して、Azure では、それらを管理するために、これらのディスクのストレージ アカウントを構成する必要はありません。
