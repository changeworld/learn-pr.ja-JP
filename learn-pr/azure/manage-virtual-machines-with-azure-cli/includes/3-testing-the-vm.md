<span data-ttu-id="df523-101">仮想マシンを作成すると、インターネット経由でアクセス可能なパブリック IP アドレスと、Azure データ センター内で使用されるプライベート IP アドレスが割り当てられてます。</span><span class="sxs-lookup"><span data-stu-id="df523-101">When you create a virtual machine, it gets assigned a public IP address that is reachable over the Internet, and a private IP address used within the Azure data center.</span></span> <span data-ttu-id="df523-102">`create` コマンドから返される JSON ブロックでこれらの両方の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="df523-102">You get both of those values in the returning JSON block from the `create` command:</span></span>

```json
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## <a name="connecting-to-the-vm-with-ssh"></a><span data-ttu-id="df523-103">SSH を使用した VM への接続</span><span class="sxs-lookup"><span data-stu-id="df523-103">Connecting to the VM with SSH</span></span>

<span data-ttu-id="df523-104">パブリック IP アドレスにより、Secure Shell (`ssh`) ツールを使用して Linux VM が稼働していることを簡単にテストできます。</span><span class="sxs-lookup"><span data-stu-id="df523-104">With the public IP address we can quickly test that the Linux VM is up and running using the Secure Shell (`ssh`) tool.</span></span> <span data-ttu-id="df523-105">ここでは、管理者名を `aldis` に設定したので、この名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="df523-105">Remember that we set our admin name to `aldis`, so we have to specify that.</span></span> <span data-ttu-id="df523-106">_ご自分の_実行中のインスタンスのパブリック IP アドレスを必ず使用してください。</span><span class="sxs-lookup"><span data-stu-id="df523-106">Make sure to use the public IP address from _your_ running instance.</span></span>

```azurecli
ssh <public-ip-address> -l aldis
```

> [!NOTE]
> <span data-ttu-id="df523-107">VM の作成の一環として SSH キー ペアを生成したので、パスワードは不要です。</span><span class="sxs-lookup"><span data-stu-id="df523-107">We don't need a password because we generated an SSH key pair as part of the VM creation.</span></span> <span data-ttu-id="df523-108">初めて VM でシェルを実行すると、ホストの信頼性に関するプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="df523-108">The first time you shell into the VM, it will give you a prompt about the authenticity of the host.</span></span> 
> 
> <span data-ttu-id="df523-109">これは、ホスト名ではなく IP アドレスを直接入力したためです。</span><span class="sxs-lookup"><span data-stu-id="df523-109">This is because we are hitting an IP address directly instead of a host name.</span></span> <span data-ttu-id="df523-110">"yes" と答えると、IP が接続の有効なホストとして保存され、接続の続行が許可されます。</span><span class="sxs-lookup"><span data-stu-id="df523-110">Answering "yes" will save the IP as a valid host for connection and allow the connection to proceed.</span></span>

```output
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

<span data-ttu-id="df523-111">ここで、リモート シェルが表示され、Linux コマンドを入力できます。</span><span class="sxs-lookup"><span data-stu-id="df523-111">Then you'll be presented with a remote shell where you can enter Linux commands.</span></span>

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

<span data-ttu-id="df523-112">練習としていくつかのコマンドを試し、終了したら、シェルからサインアウトします (シェルで `logout` または `exit` を入力)。</span><span class="sxs-lookup"><span data-stu-id="df523-112">Try a few commands as practice and when you are finished, sign out of the shell (type `logout` or `exit` in the shell).</span></span>