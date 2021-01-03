# SSH tunnelでつなぐメモ

おうちサーバーに対して外から接続したいけどGlobalIPもない、という状況でどうやっておうちに繋げるか

## したいこと
```
[PC] - (internet) -> [おうちサーバ]
```

## 実現方法
SSH port forwardを組み合わせて使う。中間サーバーをどこかに用意する必要がある。
```
[PC] -[local port fwd]-> [中間サーバー]
                         [中間サーバー] <-[remote port fwd]- [おうちサーバー]

```

- PC側
```
Host a
Hostname x.x.x.x
Port xxxx
LocalForward 10000 127.0.0.1:10000
```
- おうちサーバー側
    - 事前対応
    パスなしのSSH keyを作って中間サーバーのauthorized_keysに入れておく
    - .ssh/config
```
Host a
  Hostname x.x.x.x
  Port xxxx
  RemoteForward 10000 127.0.0.1:22
  ExitOnForwardFailure yes
```
    - このshellをcronでまわす
```
# /bin/bash
COMMAND="ssh -N -f a"
pgrep -f -x "$COMMAND" > /dev/null 2>&1 || $COMMAND > /dev/null 2>&1
```

こうすることでPC側で`ssh a`したあと`ssh 127.0.0.1:10000`で中間サーバーのport10000を経由しておうちサーバーのprt22に繋がる。

おうちサーバーのほうをcronにしてるのはIPが変わったりしたときに自動再接続できるように。pgrepでプロセスの存在確認してからsshしているのでcronタイミングは毎分でも問題ないはず、たぶん

いろんなポートで繋げたいとかなってくると素直にVPN貼った方がいいと思う。

---

# おうちESXiにVMRC
## したいこと
手元のPCでVMRC経由でおうちESXiの上のVMのコンソールを開きたい。主にMacの¥キー問題。
```
[PC] - (internet) -> [おうちESXi]
```

## ダメだった方法
上記と同様にESXi:443へのポートフォワードを作成する方法は画面転送周りで失敗

## 実現方法
おうちサーバーにsquidを入れてそこにポートフォワード
ESXiは192.168.0.0/24にある前提

- PC
    - ssh
    仕上がりローカルの3128にフォワード
```
Host a
Hostname x.x.x.x
Port xxxx
LocalForward 3128 127.0.0.1:13128
```
    - VMRC  
    接続プロキシの設定を変更  
    リモート仮想マシンの接続プロキシを有効化にチェック  
    アドレスに127.0.0.1  
    ポートに3128
    - chromeあたりにブックマーク  
    指定するべきアドレスは`vmrc://root@192.168.0.x/?moid=xxx`。moidはESXiを開いて対象の仮想マシンを参照。[このへん参考](https://qiita.com/crowz/items/0e779c972300375f2c57)

- おうちサーバー
    - squid
    squidを入れて設定。以下はCentOS7の場合  
```
# yum -y install squid

# vi /etc/squid/squid.conf
下記のように書き換える。いちおうlocalnetからインターネットへの転送はしないように
dstlocalを定義してhttp_accessのlocalnetの部分を書き換えて宛先制限する

27,28d26
< acl dstlocal dst 192.168.0.0/24
<
54c52
< http_access allow localnet dstlocal
---
> http_access allow localnet

# systemctl start squid
# systemctl status squid
# systemctl enable squid
```

    - .ssh/config  
    上記のフォワード先を変えるだけ。あくまでProxyのフォワード  
```
Host a
  Hostname x.x.x.x
  Port xxxx
  RemoteForward 13128 127.0.0.1:3128
  ExitOnForwardFailure yes
```

これで`ssh a`したあとにchromeでvmrcのリンクを開くと無事繋がるはず  

```
vmrc
- Proxy (
 -> PC:3128
 -> 中間サーバー:13128
 -> おうちサーバー: 3128
) -> おうちESXi:443
```

VPNで繋いだらこんなことしなくてもいいのは事実