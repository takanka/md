# androidからsshする

ほぼ[これ](https://qiita.com/class2glass/items/8ae0b34543a9e5d1ec2d)

まずローカルのデータにアクセスできるようにする
```
$ termux-setup-storage
```
opensshをインストール
```
$ apt update
$ apt upgrade
$ apt install openssh
$ ssh-keygen -t rsa -b 4096
$ cp .ssh/id_rsa.pub storage/downloads
```
.ssh/configを記載。vimがないのでインストール
```
$ apt install vim
$ vi .ssh/config
```
中身は適切に
```
Host hoge
  Hostname x.x.x.x
  User xxx
  Port xxx
```

一応やろうと思えばそのまま接続対象にパスワード認証で入ってcatするのも手なんだけどそうしないでSDカードなどにコピー
```
Settings → Storage → Files → Download → id_rsa.pubを長押し → Move to... → SDカードなど
```

winとかmacとかでid_rsa.pubを接続対象に持って行って
対象の上でauthorized_keysに追記
```
$ cat /tmp/id_rsa.pub >> .ssh/authorized_keys
```

termuxに戻って確認
```
$ ssh hoge
```

接続できたらOK