<span data-ttu-id="50a2d-101">仮想マシンを作成すると、インターネット経由でアクセス可能なパブリック IP アドレスと、Azure データ センター内で使用されるプライベート IP アドレスが割り当てられてます。</span><span class="sxs-lookup"><span data-stu-id="50a2d-101">When you create a virtual machine, it gets assigned a public IP address that is reachable over the Internet, and a private IP address used within the Azure data center.</span></span> <span data-ttu-id="50a2d-102">`ssh` ツールを使用して、Linux VM が稼働していることを簡単にテストできます。</span><span class="sxs-lookup"><span data-stu-id="50a2d-102">We can quickly test that the Linux VM is up and running using the `ssh` tool.</span></span> <span data-ttu-id="50a2d-103">ここでは、管理者名を `aldis` に設定したので、この名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="50a2d-103">Remember that we set our admin name to `aldis`, so we have to specify that.</span></span>

```azurecli
ssh 168.61.54.62 -l aldis
```

> [!NOTE]
> <span data-ttu-id="50a2d-104">VM の作成の一環として SSH キー ペアを生成したので、パスワードは不要です。</span><span class="sxs-lookup"><span data-stu-id="50a2d-104">We don't need a password because we generated an SSH key pair as part of the VM creation.</span></span> <span data-ttu-id="50a2d-105">初めて VM でシェルを実行すると、ホストの信頼性に関するプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="50a2d-105">The first time you shell into the VM, it will give you a prompt about the authenticity of the host.</span></span> 
> 
> <span data-ttu-id="50a2d-106">これは、ホスト名ではなく IP アドレスを直接入力したためです。</span><span class="sxs-lookup"><span data-stu-id="50a2d-106">This is because we are hitting an IP address directly instead of a host name.</span></span> <span data-ttu-id="50a2d-107">"yes" と答えると、IP が接続の有効なホストとして保存され、接続の続行が許可されます。</span><span class="sxs-lookup"><span data-stu-id="50a2d-107">Answering "yes" will save the IP as a valid host for connection and allow the connection to proceed.</span></span>

```
The authenticity of host '168.61.54.62 (168.61.54.62)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '168.61.54.62' (RSA) to the list of known hosts.
```

<span data-ttu-id="50a2d-108">ここで、リモート シェルが表示され、Linux コマンドを入力できます。</span><span class="sxs-lookup"><span data-stu-id="50a2d-108">Then you'll be presented with a remote shell where you can enter Linux commands.</span></span>

```
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

<span data-ttu-id="50a2d-109">練習としていくつかのコマンドを試し、終了したら、アカウントからサインアウトします (シェルで `logout` または `exit` を入力)。</span><span class="sxs-lookup"><span data-stu-id="50a2d-109">Try a few commands as practice and when you are finished, sign out of your account (type `logout` or `exit` in the shell).</span></span>