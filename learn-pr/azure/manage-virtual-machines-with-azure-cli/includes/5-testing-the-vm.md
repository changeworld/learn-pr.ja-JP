仮想マシンを作成すると、インターネット経由でアクセス可能なパブリック IP アドレスと、Azure データ センター内で使用されるプライベート IP アドレスが割り当てられてます。 `ssh` ツールを使用して、Linux VM が稼働していることを簡単にテストできます。 ここでは、管理者名を `aldis` に設定したので、この名前を指定する必要があります。

```azurecli
ssh 168.61.54.62 -l aldis
```

> [!NOTE]
> VM の作成の一環として SSH キー ペアを生成したので、パスワードは不要です。 初めて VM でシェルを実行すると、ホストの信頼性に関するプロンプトが表示されます。 
> 
> これは、ホスト名ではなく IP アドレスを直接入力したためです。 "yes" と答えると、IP が接続の有効なホストとして保存され、接続の続行が許可されます。

```output
The authenticity of host '168.61.54.62 (168.61.54.62)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '168.61.54.62' (RSA) to the list of known hosts.
```

ここで、リモート シェルが表示され、Linux コマンドを入力できます。

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

練習としていくつかのコマンドを試し、終了したら、アカウントからサインアウトします (シェルで `logout` または `exit` を入力)。