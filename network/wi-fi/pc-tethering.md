# PCをWi-FiのAPとして使う
- ホテルやカフェでWi-Fi使いたいけど全部の保有機器で設定するのは面倒なのでノートPCをAPにして繋げられるようにしたい
- Bluetoothテザリングはいちいち選択しないと繋がらない＋早くないのでWi-Fiでつなぎたい

# WindowsでWi-Fiテザリング
- [記事](https://xtech.nikkei.com/atcl/nxt/column/18/00095/00022/)もあるくらい簡単
- 事前設定してたらタスクバーからモバイルホットスポットを選択するだけ

# MacでWi-Fiテザリング
[公式のQA](https://support.apple.com/ja-jp/guide/mac-help/mchlp1540/mac)でインターネット共有の設定方法は書いてある。  
Windowsと違ってMacでは本体内臓のアンテナが接続とAPの両方の動作をすることはできなさそうなので下記の対応
- [Macに対応したUSBの子機](https://www.tp-link.com/jp/home-networking/adapter/archer-t2u-nano/)と[Type-C Plug to Type-A Receptacleの変換アダプタ](https://www.ainex.jp/products/u30ca-lfadt/)を組み合わせて2つめのWi-Fi接続を追加
    - ちなみに逆のType-C Receptacle to Type-A Plugの方は[仕様違反](https://hanpenblog.com/6148)
- インターネット共有で追加したEthernetアダプタを元にWi-Fiのデバイスで共有

## Macの課題
- 追加したEthernetアダプタで[Secured Wi-Fi](https://support.ntt.com/ocn/support/pid2900000c7s)が接続できない
    - TP-LINKのドライバにはEAP-TTLSに対応していなさそう？[参考](https://community.tp-link.com/en/home/forum/topic/175678)
    - なのでEAP-TTLSなau_Wi-Fi2とかWi2eapとかもおそらく接続できないと思われる

- 上記とは逆のWi-Fiから追加したEthernetアダプタに共有するパターン
    - APとしての設定ができなさそうなので有線でしか共有できない？
    - 本体のWi-Fiが802.1X認証のときにエラーになる模様？

# 昔検討したこと
モバイルAPみたいなものを使うことも考えた
- [TL-WR902AC](https://www.tp-link.com/jp/home-networking/wifi-router/tl-wr902ac/)とか
    - カフェとかの認証が必要な系統がうまく動かない
- [Aterm W500P](https://www.aterm.jp/product/atermstation/product/warpstar/w500p/index.html)
    - おそらく動くけど終売製品を今から手に入れるか？というところで断念

結果、PCをテザリングする方向へ